# Week 2 V1 Architecture and Design Decisions - PlantPal

## Architecture needs

The starting flow is User -> Web application -> AI capability -> Response. The following requirements justify the additional components shown in the [V1 diagram](week2_architecture_v1.png).

| Requirement ID | What does the requirement need? | Architecture response |
|---|---|---|
| FR1, FR2, NFR1, RAI1 | Plant suggestions with user confirmation. | C0 User, C1 Web application and C2 Plant identification AI. |
| FR4, FR8, FR9, FR11, NFR2, NFR6, RAI2 | Reviewed plant information and answers with source links. | C3 Reviewed care documents supplies passages to C4 AI care assistant and care notes to C1. |
| FR3, FR6, FR10, RAI4 | Private collections, history and deletion. | C1 checks ownership and stores records in C5 Plant collection database. |
| FR5-FR7, NFR4, RAI5 | Reminders with the browser closed and current consent checks. | C6 Scheduler and email reads due records from C5 and sends opted-in care checks to C0. |
| FR9, RAI3, RAI6 | Scope checks and protection against unwanted AI actions. | C1 checks input and owner actions; C4 cannot write records or send messages. |
| O3, NFR1-NFR6 | Evidence of quality, response time and cost. | Offline evaluation checks the relevant components using EC01-EC07. |

## Architecture decisions and trade-offs

| Decision | Requirement / constraint | Alternatives considered | Selected option | Trade-off accepted |
|---|---|---|---|---|
| ADR1 - Use an existing identification service | FR1, FR2, RAI1; individual semester project | Manual selection only; train a new model; use an existing identification service | Existing service with user confirmation and manual fallback | Less model-training work, but provider access, errors, costs and privacy need checking. Confirmation can still be wrong. |
| ADR2 - Use reviewed care documents | FR8, FR11, NFR2, RAI2 | Model answers alone; static care cards; reviewed documents with AI explanation | Three reviewed Malaysian guides with source-linked answers | Requires review and may leave some questions unanswered, but users can check the supporting evidence. |
| ADR3 - Use fixed application rules for reminders | FR5-FR7, NFR4, RAI3, RAI5 | AI-chosen action dates; browser-only timers; stored user-approved settings with a scheduler | User-approved editable intervals and a scheduler with opt-in email | Adds scheduling and delivery checks, but supports reminders with the browser closed. A due date is not proof that watering is needed. |
| ADR4 - Keep AI separate from private actions | FR3, FR10, RAI4, RAI6 | Give AI tools to edit records/send messages; require explicit app actions | C1 handles owner-approved changes; C4 only explains care | Requires explicit user steps and ownership checks, but limits accidental changes and prompt-injection effects. |

## Initial requirements traceability matrix

| Objective | Requirement | Evaluation criterion | Architecture element | Later evidence |
|---|---|---|---|---|
| O1, O3 | FR1, FR2, NFR1, RAI1 | EC01 | C0, C1, C2 | Labelled-photo results and confirmation/uncertainty checks. |
| O2 | FR3, FR6, FR10 | EC02 | C1, C5 | Collection, care-history and two-owner test results. |
| O2, O3 | FR5-FR7, FR10, NFR4, RAI5 | EC03 | C1, C5, C6 | Reminder timing, duplicate prevention and consent-change results. |
| O1-O3 | FR4, FR8, FR9, NFR2, RAI2 | EC04 | C1, C3, C4 | Care-answer scores, source checks, clarity and keyword-search comparison. |
| O2 | FR1, FR9, FR10, RAI3, RAI4, RAI6 | EC05 | C1, C4, C5, C6 | Safety, privacy, deletion and injection test results. |
| O3 | NFR3, NFR5 | EC06 | C1, C2, C4; evaluation point | Response timings, failed requests, usage and cost records. |
| O1, O3 | FR11, NFR6 | EC07 | C3, C4; source-review point | Source change record and update/regression answers. |

## Reflection

ADR3 has the most important trade-off. FR7 requires reminders to work when the browser is closed, so the system needs stored settings, a scheduler and delivery checks. However, a reminder date cannot show whether a plant needs water. The reminder therefore asks users to check conditions and lets them postpone or stop it, supporting RAI3 and RAI5.
