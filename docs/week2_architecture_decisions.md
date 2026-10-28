# Week 2 V1 Architecture Decisions - PlantPal

Version: 1.1 | Updated: 28 October 2026 | Status: conceptual design; application not implemented

[Architecture PNG](week2_architecture_v1.png) | [Editable Draw.io source](week2_architecture_v1.drawio) | [Requirements](week2_requirements.md) | [Evaluation criteria](week2_evaluation_criteria.md)

## Starting point

Start with User -> Web application -> AI capability -> Response. PlantPal also needs reviewed care information, saved plant records and reminders that work when the browser is closed. The V1 diagram therefore has seven main components. Privacy checks, source review and evaluation appear as notes beside the relevant components.

The boxes show responsibilities. The web application includes the interface and backend rules; it can start as one application. Specific models, databases, frameworks and hosting services will be chosen later.

## Seven main components

| Component | What it does | Requirements supported |
|---|---|---|
| C0 User | Takes a photo or asks a question, confirms the plant name, chooses whether to save it and approves reminders. | Intended user; FR1-FR7 |
| C1 Web application | Shows upload, confirmation, My Plants, history, reminder settings, due tasks and chat. Checks consent, photo metadata, ownership and scope before requests. Saves care actions only after the owner's choice. | FR1-FR10, RAI1, RAI3-RAI6 |
| C2 Plant identification AI | Receives a consented photo with metadata removed and returns possible plant names and scores, or a clear failure state. | FR1, FR2, NFR1, NFR3, RAI1 |
| C3 Reviewed care documents | Provides approved passages and care notes with species details and source links. Source review and updates are maintainer tasks attached to this component. | FR4, FR8, FR11, NFR2, NFR6, RAI2 |
| C4 AI care assistant | Finds relevant approved passages, explains them, checks citations and returns an answer or an explanation of missing information through C1. It cannot change records or send email. | FR8, FR9, NFR2, NFR3, RAI2, RAI3, RAI6 |
| C5 Plant collection database | Stores private plant records, optional saved photos, care history, approved reminder settings, consent and due-event status. C1 checks ownership for user access. | FR3, FR5-FR7, FR10, RAI4 |
| C6 Scheduler and email notifications | Runs independently of the open browser. Reads due events, checks current consent and record status, prevents duplicates and sends opted-in care-check emails. | FR5-FR7, NFR4, RAI5 |

## How information moves

1. **Identify:** C0 -> C1 -> C2 -> C1. The user previews the photo and agrees to identification. C1 checks the image and removes metadata before sending it. C1 shows the returned suggestions and requires confirmation of a supported plant. Failure or uncertainty allows manual selection.
2. **Save and track:** C1 <-> C5. The user separately chooses Add to My Plants. C1 checks ownership and validates the care settings before saving. Watered, still damp, skipped and fertilised events follow the transitions in the requirements.
3. **Care guidance:** C3 -> C1 supplies approved care notes. The user approves editable check intervals; the app does not infer moisture or generate a chemical dose from a photo.
4. **Ask a question:** C0 -> C1 -> C4 -> C1 -> C0, with C3 -> C4 supplying approved evidence. C1 screens input before any external AI call. If the approved passages do not support an answer, C4 returns an explanation of the gap without generating unsupported advice. C1 can ask for missing details or decline a request before calling C4.
5. **Remind:** C5 <-> C6 -> C0. Before sending, C6 checks the latest consent, due state, paused/deleted status and unique event ID. The app's due list reads the same records through C1. Delivery failure does not record a care action as completed.

## Controls and evaluation notes

- **Privacy and confirmation inside C1:** Obtain photo consent, remove metadata, confirm a supported species and keep saving optional. Only owners can access their saved records. Email addresses and private room notes stay out of AI input. Apply the deletion and retention targets in RAI4.
- **Source review beside C3:** A maintainer checks rights, species, growing conditions and conflicting advice before approving a source release. Only approved passages become available to C4. This is a review task, not a separate service.
- **Care checks beside C6:** Reminders ask users to check conditions first. Fertilising reminders start off. Users can snooze, pause or disable reminders; changes take effect before dispatch. The model has no write or email tools.
- **Offline evaluation note:** Test identification, answers, citations, ownership, privacy, reminders, source updates, speed and cost using EC01-EC14. Collect safe test outcomes and aggregate timing/usage data from the relevant components. Keep raw live images, email addresses and conversations out of diagnostic logs.

Grouping these responsibilities keeps the diagram small while retaining the controls in the requirements. It does not require a separate deployment for each box.

## Care sources

The three main care guides are [money plant](https://wengthye.com/careguide/money-plant), [snake plant](https://wengthye.com/careguide/snake-plant) and [spider plant](https://wengthye.com/careguide/spider-plant), published by 54 Weng Thye Nursery in Selangor. They are recorded in [the main source register](../data/sources.csv).

The two university PDFs in [the supporting-reference register](../data/supporting_references.csv) remain references for independent checking and lab preparation. They are not automatically part of C3's searchable collection. If the approved guides cannot answer a question, including a repotting question, C4 explains the gap. The AI and keyword-search baseline use the same approved source release.

## Architecture decisions and trade-offs

| Decision | Requirement / constraint | Alternatives considered | Selected option | Trade-off accepted |
|---|---|---|---|---|
| ADR1 - Use an existing identification service | FR1, FR2, RAI1; individual semester project | Manual selection only; train a new model; use an existing identification service | Existing service with user confirmation and manual fallback | Less model-training work, but provider access, errors, costs and privacy need checking. Confirmation can still be wrong. |
| ADR2 - Use reviewed care documents | FR8, FR11, NFR2, RAI2 | Model answers alone; static care cards; reviewed documents with AI explanation | Three reviewed Malaysian guides with source-linked answers | Requires review and may leave some questions unanswered, but users can check the supporting evidence. |
| ADR3 - Use fixed application rules for reminders | FR5-FR7, NFR4, RAI3, RAI5 | AI-chosen action dates; browser-only timers; stored user-approved settings with a scheduler | User-approved editable intervals and a scheduler with opt-in email | Adds scheduling and delivery checks, but supports reminders with the browser closed. A due date is not proof that watering is needed. |
| ADR4 - Keep AI separate from private actions | FR3, FR10, RAI4, RAI6 | Give AI tools to edit records/send messages; require explicit app actions | C1 handles owner-approved changes; C4 only explains care | Requires explicit user steps and ownership checks, but limits accidental changes and prompt-injection effects. |

[Pl@ntNet](https://docs.plantnet.org/en/reference/api-plantnet/) remains a possible identification provider. Its [terms](https://my.plantnet.org/terms_of_use), attribution, costs and privacy practices must be checked before integration. No service account or paid plan is created by this design document.

## Initial requirements traceability matrix

| Objective | Requirement | Evaluation criterion | Architecture element | Later evidence |
|---|---|---|---|---|
| O1 | FR1, FR2, NFR1, RAI1 | EC01, EC02 | C1, C2 | Labelled-photo results and confirmation/fallback tests. |
| O2 | FR3, FR6, FR10 | EC03, EC09 | C1, C5 | Two-owner collection tests, care history and deletion results. |
| O2 | FR5-FR7, NFR4, RAI5 | EC04, EC10 | C1, C5, C6 | Due-event timing, duplicate prevention, consent and opt-out tests. |
| O1, O2 | FR4, FR8, NFR2, RAI2 | EC05-EC07 | C1, C3, C4 | Reference questions, answer scores and claim-support checks. |
| O2 | FR9, RAI3 | EC08 | C1, C4, C6 | Scope tests and reviewed care-check message templates. |
| O2 | FR1, FR10, RAI4 | EC09 | C1, C5 | Photo-consent, metadata, ownership and retention checks. |
| O3 | NFR3, NFR5 | EC11, EC12 | C1, C2, C4; offline evaluation note | Timed request runs, usage totals and cost calculations. |
| O1, O3 | FR11, NFR6 | EC13 | C3; source-review and evaluation notes | Source release record and eleven update/regression results. |
| O2 | RAI6 | EC14 | C1, C4 | Injection tests and confirmation that AI has no write/send tools. |

All evidence entries describe future tests. No application results or user-study findings are claimed.

## Architecture review

- Seven main components cover the requested photo, collection, chat and reminder features.
- Privacy checks, source review and offline evaluation are visible as annotations and explained above.
- The web application can contain the input checks, confirmation steps and care rules in one backend.
- The scheduler is required for closed-browser email reminders. The database is required for saved plants, history and reminder state.
- Specific technologies, sensors, model training, weather services and extra notification channels are outside this V1 design.

## Reflection

ADR3 has the most important trade-off. Users need reminders even when the browser is closed, which requires a scheduler and delivery checks. However, a reminder date cannot show whether a plant needs water. I therefore keep the message as a care check and let users postpone or stop it. This follows FR7, RAI3 and RAI5 while keeping the AI focused on explaining care information.
