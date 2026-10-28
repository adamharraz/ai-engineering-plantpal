# Week 2 V1 Architecture Decisions - PlantPal

Version: 1.0 | Updated: 28 October 2026 | Status: conceptual design, not an implemented application

[Architecture PNG](week2_architecture_v1.png) | [Editable Draw.io source](week2_architecture_v1.drawio) | [Requirements](week2_requirements.md) | [Evaluation criteria](week2_evaluation_criteria.md)

## Starting architecture and justified additions

The starting interaction is User -> Application -> AI capability -> Response. It does not by itself support tentative identity confirmation, private collections, reviewed evidence or notifications while the browser is closed. The components below are added for those requirements. Labels describe functions; no database, model, UI framework or deployment vendor is selected at V1.

| Component | Responsibility | Justifying requirements |
|---|---|---|
| C0 User | Photograph/select a plant, confirm identity, choose save and opt into care checks. | Intended user, FR1-FR7 |
| C1 Application interface | Camera/upload preview, candidates and confirmation, My Plants, history, due list and source-linked chat. | FR1-FR10 |
| C2 Application controls | Authenticate owners for saved data; validate inputs, photo consent and metadata; check scope and route requests. | FR1-FR3, FR9, FR10, RAI1, RAI4 |
| C3 Image identification capability | Receive a consented sanitised photo and return ranked candidate names/scores; expose failures. | FR1, NFR1, NFR3 |
| C4 Confirmation and care-plan rules | Enforce supported identity and explicit Add; validate approved editable intervals, consent and care event transitions. | FR2, FR4-FR6, RAI1, RAI3 |
| C5 Private collection store | Owner-scoped plant/profile/history/optional-photo data, email destination reference, schedules and pending event state. | FR3, FR5-FR7, FR10, RAI4 |
| C6 Scheduler and notification gateway | Run without an open browser; recheck latest consent/due state, deduplicate, dispatch and record delivery status. | FR5-FR7, NFR4, RAI5 |
| C7 Reviewed knowledge access | Serve relevant passages and reviewed care templates with species/context/provenance metadata. | FR4, FR8, FR11, NFR2, NFR6, RAI2 |
| C8 Language-model explanation | Use a screened question, structured care context and approved evidence to draft a cited explanation; no write/send tools. | FR8, RAI2, RAI6 |
| C9 Answer validation and response | Check source IDs, species/context, scope and format; return answer, clarification or abstention through C1. | FR8, FR9, RAI2, RAI3 |
| C10 Source review and catalogue | Maintainer reviews public documents, permissions, conflicts and source release; activates passages/templates. | FR11, NFR6 |
| C11 Evaluation and operational observations | Run frozen cases; observe sanitised case outcomes, duration, usage and dispatch status. | O3, NFR1-NFR6, RAI1-RAI6 |

### Major information flows

1. **Identification:** C0 -> C1 -> C2 -> C3 -> C4 -> C1. C2 validates type/size, previews consent and removes metadata before any remote identification call. C4 presents candidates for explicit confirmation; failure/unsupported results use manual selection or abstention.
2. **Collection and care setup:** C1 -> C2 -> C4 <-> C5. C4 reads reviewed templates from C7, but saves only on explicit Add and approved scheduling choices. Identify-only photos expire even if the user never saves.
3. **Care reminders:** C5 <-> C6 -> opted-in user. C6 reads the latest record immediately before dispatch and uses a stable due-event ID to prevent duplicate sending. An app due list reads the same state through C2/C4. Browser timers alone are insufficient for closed-app delivery.
4. **Care Q&A:** C1 -> C2 -> C7 -> C8 -> C9 -> C1. Only a screened question and permitted structured context go to retrieval/AI services. No sufficient evidence follows C7 -> C9, bypassing generation. Missing context or prohibited input follows C2 -> C9 directly.
5. **Source review:** Public documents -> C10 -> C7. C10 activates a reviewed release; it does not ingest arbitrary live pages from the user's question.
6. **Evaluation:** C2, C3, C6 and C9 supply sanitised observations to C11. Test photos and synthetic cases live in an access-controlled evaluation workspace; no raw live images, messages or email addresses enter monitoring logs.

The diagram shows logical responsibilities, not a microservices deployment. C2/C4/C7/C9 can initially be modules in one backend. C5 is the only private persistent-data boundary; C3/C8 are external AI boundaries. The notification transport receives only the opted-in destination and minimal reminder text. It is never callable by C8.

## Architecture decisions and trade-offs

| Decision | Requirement / constraint | Alternatives considered | Selected option | Trade-off accepted |
|---|---|---|---|---|
| ADR1 - Provisional identification with explicit confirmation | FR1, FR2, RAI1; limited labelled data and semester time | Manual selector only; train a classifier; external pre-trained identification capability | Existing identification capability plus manual fallback and supported-species confirmation | Avoids collecting a training dataset, but adds provider dependency, latency, consent and error-handling work. User confirmation can still be wrong. |
| ADR2 - Reviewed evidence and bounded care coverage | FR8, FR11, NFR2, RAI2 | Base-model answers; static care cards only; unrestricted web search; reviewed knowledge access | Three-species reviewed corpus with retrieval and cited explanations | More curation and possible abstention, in exchange for traceable evidence and manageable tests. This does not guarantee truth automatically. |
| ADR3 - Deterministic checks and server-side reminders | FR5-FR7, NFR4, RAI3, RAI5 | AI-generated action dates; browser-only timers; user-approved rules plus persistent scheduler | Reviewed wording, user-approved editable intervals, persisted due events, one opt-in email channel | Requires a worker/transport and idempotency/state handling, but works when the app is closed. Exact biological watering need remains unknown; email arrival cannot be guaranteed. |
| ADR4 - Separate read-only AI from private actions | FR3, FR10, RAI4, RAI6 | Give the assistant write/send tools; public photo storage; owner-scoped records and explicit UI actions | Owner-checked private storage, short-lived uploads and explicit application-controlled changes | Adds access/deletion tests and confirmation steps, but limits prompt-injection effects and accidental exposure. |

Pl@ntNet is a researched identification candidate, not a technology commitment. Its [API documentation](https://docs.plantnet.org/en/reference/api-plantnet/) and [terms](https://my.plantnet.org/terms_of_use) must be checked for supported names, attribution, access and retention before integration. API accounts, keys and notification-provider subscriptions have not been created for this planning submission.

## Initial requirements traceability matrix

| Objective | Requirement | Evaluation criterion | Architecture element | Later evidence |
|---|---|---|---|---|
| O1 | FR1, FR2, NFR1, RAI1 | EC01, EC02 | C2, C3, C4 | Labelled-image manifest, provider results and confirmation-gate tests. |
| O2 | FR3, FR6, FR10 | EC03, EC09 | C1, C2, C4, C5 | Synthetic two-owner collection flows and denied cross-owner requests. |
| O2 | FR5-FR7, NFR4, RAI5 | EC04, EC10 | C4, C5, C6 | Clock-controlled due events, retry logs and opt-out/deletion scenarios. |
| O1, O2 | FR4, FR8, NFR2, RAI2 | EC05, EC06, EC07 | C7, C8, C9, C10 | Frozen passage references, answer rubric and claim-support annotations. |
| O2 | FR9, RAI3 | EC08 | C2, C4, C9 | Boundary cases and reviewed watering/fertilising reminder templates. |
| O2 | FR1, FR10, RAI4 | EC09 | C2, C5 | Consent, EXIF removal, minimised payload and retention/deletion checks. |
| O3 | NFR3, NFR5 | EC11, EC12 | C2, C3, C8, C11 | Timed request runs, failures, token/request counts and dated price calculation. |
| O1, O3 | FR11, NFR6 | EC13 | C7, C10, C11 | Versioned source update and eleven regression results. |
| O2 | RAI6 | EC14 | C2, C8, C9 | Injection cases, read-only permission inspection and absence of unauthorised actions. |

All later-evidence entries are plans. No passed application tests, user research, integrations or deployment results are claimed.

## Architecture review

- Every major component is justified in the component table. The scheduler is needed for closed-app reminders; the private store is needed for personal collections and history.
- Confirmation, metadata removal, ownership checks, consent rechecks, no-evidence bypasses and an evaluation point are represented.
- The initial deployment can combine application functions in one backend; queues, model training, sensors, weather services and multiple notification channels are not required at V1.
- A simpler manual-selector/care-card prototype would remove one AI path but would not satisfy the requested photo-identification workflow. Keep it as a fallback and baseline, not a replacement for FR1.
- The model cannot send notifications or alter schedules. Only owner-authorised application operations can do that.

## Reflection

ADR3 involves the most important trade-off. FR7 requires reminders even when the browser is closed, but RAI3 prevents treating a scheduled date as proof that a plant needs water. I therefore accept the extra scheduler and consent-state work while keeping the message as a check and letting the owner snooze or disable it. This creates a clearer boundary between AI explanation and routine application actions, at the cost of more integration testing.
