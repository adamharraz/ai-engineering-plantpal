# Requirements Specification - PlantPal

Version: 1.0 | Updated: 28 October 2026 | Status: proposed and unimplemented

## Section 0 - Project Framing and Application Scope Checkpoint

### Problem and evidence

Beginner indoor-plant owners need to identify a plant, interpret its care instructions and remember what care they have already provided. A photograph alone does not reveal soil moisture or the growing environment, while a calendar alone cannot determine whether watering is needed. PlantPal proposes one workflow for tentative photo identification, a user-confirmed plant collection, care records, editable reminders and explanations grounded in reviewed documents. Its initial audience is beginners in Malaysia; Malaysian demand and time savings have not yet been measured.

- [University of Illinois Extension, Watering](https://extension.illinois.edu/houseplants/watering) describes the difficulty of under- and overwatering and explains that plant and environmental conditions affect water needs. This supports moisture-check reminders rather than automatic instructions to water on a date.
- [University of Tennessee Extension, Caring for Indoor Plants, W 1128-B](https://uthort.tennessee.edu/wp-content/uploads/sites/228/2023/11/W1128B.pdf), especially the watering and repotting sections, documents how care depends on containers, growing media and conditions. This supports collecting a small amount of context and explaining care steps.

These sources establish practical care difficulties, not the prevalence of the problem among Malaysian students. Forgetfulness, demand for a collection feature and improved usefulness are project hypotheses. A later small task-based usability study can investigate them; no interviews or outcomes are claimed here.

### Existing approaches

| Study / existing solution | What it already addresses | Limitation relevant to PlantPal |
|---|---|---|
| [Pl@ntNet identification service](https://docs.plantnet.org/en/reference/api-plantnet/) | Image-based identification with ranked candidate species and confidence scores | A candidate is not a verified identity or a measurement of soil moisture. PlantPal still needs confirmation, supported-species checks and a care workflow. |
| [Planta](https://getplanta.com/) | Its official product page advertises identification, care reminders and plant organisation | Similar features already exist. That page does not establish performance for PlantPal's three-species test set or the proposed citation and reminder controls; no claim is made that Planta lacks these controls. |
| [University of Illinois Extension houseplant guidance](https://extension.illinois.edu/houseplants/watering) | Expert-authored care information for readers | The inspected guidance is a reference document; maintaining a personal collection and tracking due tasks remain application work. Some guidance needs climate and species review. |
| [Lewis et al. (2020), Retrieval-Augmented Generation for Knowledge-Intensive NLP Tasks](https://arxiv.org/abs/2005.11401) | Studies combining retrieved knowledge and language generation | Its results do not validate PlantPal's images, horticultural corpus or care safety. The proposed retrieval-and-generation pipeline requires its own evaluation. |

### Engineering gap

The bounded engineering gap is to integrate uncertain photo identification with confirmed species, private care records, user-controlled check reminders and verifiable explanations for a small indoor-plant corpus. The project will test whether these parts work reliably together and whether the care assistant improves on keyword search over the same sources. Existing apps already cover much of this workflow, so this is an integration and evaluation project rather than a claim of scientific novelty or a first-of-its-kind product.

### Objectives

- **O1 - Design:** Define a three-species knowledge base and a traceable workflow connecting tentative identification, user confirmation, personal records and source-backed care checks.
- **O2 - Develop:** Implement a mobile-friendly English web prototype with photo suggestions, optional collection saving, editable watering-check and fertilising reminders, care history and cited care Q&A.
- **O3 - Evaluate:** Assess identification, answer correctness, source support, collection isolation, reminder behaviour, latency and operating cost against frozen reference cases and a keyword-search baseline for Q&A.

### Application scope

| Framing element | V1 commitment |
|---|---|
| Intended users | Beginner indoor-plant owners in Malaysia; English interaction. |
| Supported tasks | Photo suggestions; explicit species confirmation; optional private collection; context profile; care records; editable watering checks and optional fertilising reminders; sourced care Q&A. |
| Knowledge domain | Golden pothos, snake plant and spider plant; watering, light, drainage and basic repotting. |
| Operating context | Mobile-friendly browser, internet connection, private user collections, one email notification channel plus an in-app due list, Asia/Kuala_Lumpur local time. Camera use is optional; upload/manual entry remain available. |
| Constraints | Individual semester prototype; pre-trained identification service; small reviewed corpus; limited operating budget; third-party access and rights must be verified. No new model training. |
| Explicit exclusions | Disease diagnosis, chemical dosage, toxicity/ingestion treatment, edible-plant decisions, moisture sensing, autonomous watering, weather prediction, native mobile releases and social sharing. |
| Evaluation focus | Can the prototype identify supported plants provisionally, preserve ownership and consent, explain care accurately, and issue appropriate non-duplicate reminders? |
| Evidence status | Sources support care difficulties. Local demand, usability improvements, identification accuracy and plant-health benefits are not established. |

| Supported plant | Canonical name / aliases | Evidence handling |
|---|---|---|
| Golden pothos / money plant | *Epipremnum aureum* | Match the botanical name; do not treat every plant called money plant or silver pothos as this species. |
| Snake plant | *Dracaena trifasciata*; synonym *Sansevieria trifasciata* | Normalise the synonym to the same supported species. |
| Spider plant | *Chlorophytum comosum* | Support this species; do not infer that genus-level guidance covers every related species. |

These are the three V1 care species. An identification provider may recognise more plants; PlantPal must label other results as outside its reviewed care coverage. A user can request another photo or manually choose a supported identity, with the choice recorded as user-confirmed rather than botanically verified.

### Objective traceability

| Objective | Principal requirements | Evaluation criteria | Architectural responsibility |
|---|---|---|---|
| O1 | FR1, FR2, FR4, FR11, RAI1, RAI2 | EC01, EC02, EC05, EC13 | Identification gateway, confirmation gate and reviewed knowledge. |
| O2 | FR3-FR10, RAI3-RAI6 | EC03, EC04, EC06-EC10, EC14 | Application, private records, care rules, scheduler and explanation path. |
| O3 | NFR1-NFR6 | EC01-EC14 | Offline evaluator and safe timing/usage observations. |

### Schedule semantics

A watering reminder says to check moisture against the relevant species guidance. It does not assert that watering is due physiologically. The user explicitly chooses/approves a check interval (1-30 days), time and start date. The interval is an operational preference rather than a scientifically validated watering prediction. A still-damp or skip event offers an explicit snooze date; it does not silently learn a new schedule.

Fertilising reminders are off by default. The user may enter an interval (1-365 days), start date and optional product-label note; PlantPal provides no dose or product recommendation. Missing interval or date blocks activation. The system asks the user to review whether fertilising is appropriate before acting. The language model cannot enable or modify either schedule.

| Owner action | Required schedule transition |
|---|---|
| Mark watered | Record once; next watering check is the action timestamp plus the approved check interval. This does not assert a future water need. |
| Mark fertilised | Record once; next fertilising review is the action timestamp plus its approved interval, only if that reminder is enabled. |
| Still damp / skip | Record the event; request an explicit future snooze date or offer pause. Do not log watering or silently alter the base interval. |
| Change species | Pause both plans and require review/approval for the new supported identity. |
| Pause / opt out / delete | Cancel queued unsent events immediately; deletion also starts the retention-removal process. |
| Delivery failed | Keep the care action uncompleted; expose failure and retain the same event identity for any bounded retry. |

## Section 1 - Functional Requirements

| ID | Requirement - The system shall | Scope boundary / observable condition |
|---|---|---|
| FR1 | Accept one JPEG/PNG photo up to 5 MB from upload or supported camera capture, and display up to three provider-ranked species candidates with scores and an uncertainty notice, or a clear failure state. | One plant per request; never silently force a non-plant or unsupported species into the supported list. Scores are provider scores, not calibrated accuracy probabilities. |
| FR2 | Require an explicit user selection or correction of a supported species before offering its care plan; provide manual selection and a separate Add to My Plants action. | Photo identification alone neither confirms identity nor creates a saved collection record. |
| FR3 | Allow a signed-in owner to create, view, edit and remove their plant records, including nickname, confirmed species, optional photo and room label, and care history. | Identify-only use need not sign in or save. Collection data belongs only to its owner. |
| FR4 | Collect light and drainage context with an unknown option; display source-backed care guidance and an editable check schedule requiring user approval. | Missing plant identity blocks care-plan activation. Unknown context triggers general guidance or clarification rather than invented measurements. |
| FR5 | Let the owner set, change, snooze, pause or disable watering-check reminders and independently configure optional fertilising reminders. | Validate interval/date fields; fertilising is initially disabled; reminder text requests assessment rather than unconditional watering or fertilising. |
| FR6 | Record watered, still damp, skipped and fertilised events with timestamps, and display history and the resulting next check. | A still-damp or skip event requires an explicit snooze choice; marking an event twice must not duplicate it. |
| FR7 | Show due items in the app and send email reminders only after opt-in and destination verification, including when the app is closed. | Check current consent, ownership, due state and event ID immediately before dispatch. Failed delivery is visible and does not mark a care action as completed. |
| FR8 | Answer English care questions for supported plants using relevant reviewed source passages, with source links and section/page references when available. | Answers cover watering, lighting, drainage and basic repotting; do not derive care quantities from photos. |
| FR9 | Ask for necessary missing context or explain insufficient/conflicting evidence, and redirect unsupported requests appropriately. | No confirmed disease diagnosis, chemical dosing, ingestion treatment or assurance that a plant is edible or pet-safe. |
| FR10 | Enforce owner-only access and provide deletion of a saved plant, retained photo, history and schedules; permit notification opt-out and account-data deletion. | Deletion cancels queued unsent reminders; another owner cannot read or modify the record using a guessed ID. |
| FR11 | Let a maintainer review and activate a source release with species/topic/context metadata and source provenance. | No automatic live-web ingestion; unresolved contradictions or unreviewed seasonal rules remain inactive. |

## Section 2 - Non-Functional Requirements

Thresholds below are provisional prototype acceptance targets, not observed results. Workload definitions and scoring rules are in [the evaluation plan](week2_evaluation_criteria.md).

| ID | Quality attribute | Requirement | Measurement method | Metric | Threshold |
|---|---|---|---|---|---|
| NFR1 | Identification quality | Supported-species suggestions shall meet the labelled-photo target. | Freeze provider/configuration after a separate validation set; run image set I once. | Top-3 species inclusion, overall and per species; report top-1 separately. | At least 24/30 overall and at least 7/10 per species; no top-1 acceptance claim. |
| NFR2 | Care correctness | Care answers shall be consistent with the reviewed release and supplied context. | Score all A questions using the defined rubric. | Fully correct answer rate and per-species rate. | At least 24/30 overall and at least 7/10 per species. |
| NFR3 | Interactive performance | Photo suggestions and care answers shall meet interactive latency targets. | Measure submission-to-render time for I and A, three runs each, with file/network conditions recorded. | p90 photo latency; p90 Q&A latency, nearest-rank. | Photo at most 20 s; Q&A at most 15 s; failures count as unsuccessful requests and are reported. |
| NFR4 | Reminder reliability | A due reminder shall be dispatched once within the tolerance on a controlled running service. | Run D with an instrumented clock, worker restart/retry and fake mail transport. | Correct unique events; dispatch lateness; duplicates/early sends. | All 20 correct and unique; at least 19/20 within 2 min; zero early or duplicate sends. |
| NFR5 | Operating cost | External inference shall stay within the initial evaluation budget. | Meter 50 image requests and 50 Q&A requests with retries, retrieval/embedding charges and dated unit prices. | USD per 100 mixed requests. | At most US$2; report actual paid and list-price-equivalent cost separately, plus hosting/email costs separately. |
| NFR6 | Maintainability | A reviewed source replacement shall update the relevant guidance without changing unrelated functionality. | Run M using one controlled factual update and regression questions. | Review-to-activation time and correct regression cases. | At most 10 min after review approval; 1/1 changed case and 10/10 unaffected cases correct. |

NFR4 concerns application dispatch/transport acceptance, not guaranteed arrival in an email inbox. An end-to-end delivery check is reported separately. Initial demonstration scale is two test accounts with up to five saved plants each, not a production scalability claim.

## Section 3 - Responsible AI Requirements

| ID | Risk | Requirement / control | Measurement method | Metric | Threshold |
|---|---|---|---|---|---|
| RAI1 | Misidentification and false confidence | Display suggestions as uncertain, require FR2 confirmation, and withhold plans for unsupported/unconfirmed results. | Run U through UI and direct backend requests, including ambiguous images, failures and unsupported species. | Appropriate clarification/abstention; automatic plan activation count. | At least 18/20 appropriate, with at least 9/10 in each U subset; zero plans activated without a user-confirmed supported species. |
| RAI2 | Invented or mismatched care evidence | Cite approved passages for factual care claims and disclose missing or conflicting evidence. | Audit all factual claims in A against cited passages and source metadata. | Citation presence; supported-claim ratio. | 100% of source-answerable responses have citations; at least 95% of factual care claims supported. |
| RAI3 | Harmful care and over-reliance | Frame scheduled care as checks and avoid disease certainty, chemical doses and ingestion treatment. | Review S cases and all reminder templates, including prompts demanding unsafe certainty. | Boundary-compliant outputs; unconditional action/dose violations. | 20/20 compliant S cases and zero prohibited instructions. |
| RAI4 | Image, account and household privacy | Obtain purpose-specific photo consent, strip metadata, minimise external payloads and restrict retained data to its owner. | Run P with synthetic metadata and two test owners; inspect outbound payloads, storage and deletion. | Privacy/access scenarios passed; forbidden payload fields or cross-owner accesses. | 20/20 pass; zero forbidden fields or cross-owner accesses; unsaved temporary photos removed within 15 min and saved-plant active records within 24 h of deletion. |
| RAI5 | Unwanted notifications | Send only current, owner-authorised due reminders; honour pause, opt-out and deletion before dispatch. | Run T across consent changes, queued events, duplicate retries and edits. | Correct notification-state decisions; unauthorised or stale sends. | 20/20 correct; zero sends after opt-out/pause/deletion takes effect. |
| RAI6 | Prompt injection / unauthorised actions | Treat image text, notes, retrieved text and user questions as untrusted data; keep the language model read-only. | Run J attacks and inspect tool permissions and actual record/mail side effects. | Boundary-preserving responses; unauthorised writes/sends. | At least 19/20 responses preserve boundaries; zero unauthorised writes or sends. |

Photo pixels are transmitted only to the chosen identification provider after consent; email addresses only to the notification service for opted-in delivery. Neither belongs in the language-model/retrieval payload. Free-text notes and room labels are not sent to AI services; care Q&A uses only the selected species, structured care context, screened question and evidence. Apparent personal details in questions are blocked or removed before any external call. No raw live photos or conversations are written to diagnostic logs.

Retain a collection photo only after a separate save choice. Temporary uploads are private and deleted within 15 minutes of completion/cancellation. Deleted account/plant records are removed from active storage within 24 hours, and any backup expiry must be documented and capped at 30 days before deployment; cancellation of unsent reminders is immediate. Provider retention and attribution terms must be reviewed before activation; app-side deletion cannot promise deletion from third-party systems. Use synthetic profiles and consented/appropriately licensed images for evaluation.

## Reflection

The hardest requirement to enforce is RAI2: a valid source link does not prove that the advice follows from the cited passage. Two documents may use different moisture wording or assume different growing conditions. I will preserve species and context metadata, use a manual claim-support rubric and abstain on unresolved conflicts. Automated source-ID checks can prevent invented references, but human evaluation is still needed for meaning. RAI3 also requires reviewing reminders as user-facing advice, not just checking that the scheduler ran.

## References

See the linked sources above and the [Week 1 source table](week1_project_scope.md#8-public-data-sources). [Pl@ntNet API access and retention terms](https://my.plantnet.org/terms_of_use) are a dependency-review source, not approval to upload user photos. Diagram and decision references are in [the architecture decisions](week2_architecture_decisions.md).
