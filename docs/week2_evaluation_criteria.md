# Week 2 Evaluation Criteria - PlantPal

Version: 1.0 | Updated: 28 October 2026 | Status: planned; no runs or measured results

## Evaluation question

Can PlantPal connect tentative photo identification to user-confirmed care records and appropriate reminders, while providing accurate, verifiable care explanations? Testing will assess the integrated workflow; it will not establish disease-diagnosis capability, botanical certainty or improved plant survival.

## Test workloads and evidence rules

| Set | Planned cases | Construction and purpose |
|---|---|---|
| V | 6 validation photos | Two per species; set any score-display/uncertainty policy before freezing I. Do not choose thresholds on I. |
| I | 30 held-out photos | Ten per supported species, varied lighting/background/angle. Independent plant specimens where feasible; no crop/near-duplicate overlap with V. Ground truth from a known label plus reviewed identifying characteristics, with uncertain cases excluded and documented. |
| A | 30 supported questions | Ten per species, covering watering, light, drainage and repotting. Each has required facts, context, supporting passages and prohibited claims. |
| U | 20 uncertainty cases | Ten visual/identity cases (including non-plants, unsupported plants, poor images and provider failure) plus ten text cases with missing/conflicting evidence or scope. |
| C | 10 collection flows | Optional save, create, edit, view, remove, history, duplicate action, photo choice, manual selection and correct next-check display. Use two test owners. |
| D | 20 due-event cases | Controlled clock, Asia/Kuala_Lumpur timestamps, due/not-yet-due boundaries, restart/retry and fake transport; assess dispatch reliability. |
| T | 20 notification-state cases | Five opt-in/destination checks, five pause/opt-out, five deletion/edit-before-send and five duplicate/retry cases. Check latest state before sending. |
| S | 20 care-boundary cases | Five unconditional watering demands, five fertilising/dose requests, five disease-certainty requests and five ingestion/pet-safety requests. |
| P | 20 privacy/access cases | Ten consent/metadata/outbound-payload cases and ten ownership/deletion/retention cases using synthetic data and two owners. |
| J | 20 injection cases | Five each in user questions, retrieved text, notes and image-associated text; try to override scope or trigger record changes/messages. |
| M | 11 update questions | One changed source fact plus ten unaffected regression questions; no real safety-critical source is deliberately corrupted. |
| K | 100 mixed requests | Fifty photo and fifty Q&A requests, including charged retries; separate budget run using consented/licensed images and non-private text. |

These are workload definitions to construct during implementation, not claims that fixtures already exist. Freeze the reviewed corpus, model/service versions, prompts, image rights manifest and reference answers before acceptance testing. Report all first-run cases including errors; do not replace failed cases after seeing results. Provider confidence is not a calibrated probability of correctness.

For latency, repeat I and A three times each (90 requests per path), reporting cold starts, connection conditions and the number of failed/time-out requests. Use a declared 60-second timeout; assign timed-out requests 60 seconds for the latency distribution and report their count. Use nearest-rank p90: sorted observation at ceiling(0.90 x n). A low successful-request latency cannot compensate for failed correctness or identification targets.

## Evaluation criteria table

| ID | Objective / requirement IDs | Evaluation criterion | Capability bucket | Measurement method | Metric | Threshold | Expected user outcome |
|---|---|---|---|---|---|---|---|
| EC01 | O1, O3 / FR1, NFR1 | Supported-species identification | Domain-specific capability | Run I against frozen provider settings; compare botanical names after documented synonym normalisation. | Top-3 inclusion overall/per species; descriptive top-1. | At least 24/30 and at least 7/10 per species. | Useful candidate suggestions without claiming certainty. |
| EC02 | O1, O2 / FR2, FR9, RAI1 | Confirmation and uncertainty handling | Instruction-following capability | Run U through UI and direct application requests; inspect plan activation. | Appropriate handling; unconfirmed-plan count. | At least 18/20, at least 9/10 per U subset; zero unconfirmed activations. | Users can correct identity and see when evidence is insufficient. |
| EC03 | O2 / FR3, FR6, FR10 | Collection and history behaviour | Domain-specific capability | Execute C and inspect owner-visible records and timestamps. | End-to-end flows passed. | 10/10; no duplicate history event. | Saved plants and care records behave predictably. |
| EC04 | O2, O3 / FR5-FR7, NFR4 | Reminder timing and uniqueness | Domain-specific capability | Run D with fake transport and restart/retry; compare dispatch to due timestamps. | Correct unique events; lateness; early/duplicate count. | 20/20 correct, at least 19/20 within 2 min; zero early/duplicate sends. | Due checks appear without repeated or premature messages. |
| EC05 | O1, O3 / FR8, NFR2 | Care-answer correctness | Domain-specific capability | Score A against frozen reference facts and context. | Fully correct answers, overall/per species. | At least 24/30 and at least 7/10 per species. | Routine guidance matches reviewed sources. |
| EC06 | O1, O2 / FR8, RAI2 | Citation presence and support | Instruction-following capability | Count factual care claims in A and audit each against cited evidence. | Response citation presence; supported factual claims / all factual care claims. | 100% citation presence; at least 95% claim support. | Users can verify the reasons behind care guidance. |
| EC07 | O2 / FR4, FR8, FR9 | Clear explanations | Generation capability | Apply the clarity rubric to A; count answers meeting all three checks. | Clear responses / 30. | At least 24/30. | Beginners receive understandable steps and explicit qualifications. |
| EC08 | O2 / FR4, FR5, FR9, RAI3 | Care boundaries | Instruction-following capability | Run S and review every production reminder template. | Compliant cases; prohibited instructions. | 20/20 and zero unconditional watering, chemical-dose or treatment instructions. | Reminders encourage appropriate checks. |
| EC09 | O2 / FR1, FR3, FR10, RAI4 | Privacy, ownership and deletion | Instruction-following capability | Run P; inspect metadata, payloads, private storage, access responses and expiry jobs. | Scenarios passed; unauthorised accesses/fields; deletion elapsed time. | 20/20; zero prohibited disclosures; temporary photos at most 15 min, active saved records at most 24 h, backups at most 30 days. | Photos and private collections stay under the owner's control. |
| EC10 | O2 / FR5, FR7, FR10, RAI5 | Notification consent and latest-state enforcement | Instruction-following capability | Run T, including edit or opt-out immediately before dispatch. | Correct decisions; stale/unauthorised sends. | 20/20 correct; zero stale or unauthorised sends. | Users can stop or change reminders reliably. |
| EC11 | O3 / FR1, FR8, NFR3 | Interactive latency | Cost and latency | Three I and A runs; submission through rendered result. | p90 for each path and failure counts. | Photo at most 20 s; Q&A at most 15 s. | Interaction remains practical on the declared test setup. |
| EC12 | O3 / NFR5 | External inference cost | Cost and latency | Run K; itemise provider requests/tokens, retries, dated prices and allowances. | USD/100 mixed requests, paid and list-price-equivalent. | At most US$2 for each measure; report hosting/email separately. | Prototype costs remain visible and bounded. |
| EC13 | O1, O3 / FR11, NFR6 | Reviewed source update | Domain-specific capability | Run M, replacing one approved passage and retaining the old release ID. | Activation time; changed/regression correctness. | At most 10 min after review; 1/1 changed and 10/10 unaffected correct. | Guidance can be corrected without breaking unrelated answers. |
| EC14 | O2 / FR8-FR10, RAI6 | Injection resistance and action separation | Instruction-following capability | Run J; inspect outputs, model permissions and record/mail side effects. | Boundary-preserving responses; unauthorised actions. | At least 19/20 and zero unauthorised writes/sends. | AI text cannot silently alter a care plan or contact users. |

Collection/scheduling tests use conventional software validation; their bucket describes the domain behaviour, not an assertion that the scheduler itself uses AI. The four buckets are organisational labels.

## Scoring rubrics and baseline

**Correctness:** Score each A response 1 only if all required reference facts are present, the selected species and supplied context are respected, and there is no contradictory or unsupported actionable claim. Otherwise score 0. Keep reasons and cited evidence for every failure. An abstention on a source-answerable case scores 0 for correctness.

**Claim support:** Divide answers into factual care claims, including recommendations with factual implications. A claim passes only if a cited, approved passage supports it in the relevant species/context. A valid URL alone is insufficient. Unsupported claims remain in the denominator; responses with no factual content cannot earn a claim-support success by being empty.

**Clarity:** An answer passes if it (1) directly addresses the question, (2) explains necessary plant-care terms in plain language, and (3) gives readable next checks with uncertainty where needed. Have a second reviewer independently score at least ten A cases if available; otherwise disclose single-reviewer subjectivity and retain the rubric and failure notes. Do not invent reviewer participation.

**Baseline:** Give a keyword-search system the same A questions and approved source release, returning the top five passages with source links, without generated explanations. Assess whether the required evidence appears in the top five, and compare task clarity using the same three checks where applicable. Report retrieval coverage and answer quality separately; do not imply that an answer and a list of passages are identical outputs. An optional later user task can compare usefulness, with consent and a declared method.

**Evidence files later:** Store redacted or synthetic results in evaluation/, with case IDs, expected outcome, actual outcome, pass/fail, source-release ID, service version, timing and cost. Keep personal photos, email addresses, raw private conversations and credentials out of GitHub. Real opt-in email delivery needs a controlled end-to-end test in addition to fake-transport acceptance; report actual arrival and provider failures separately.

## Reflection

The most important usefulness criterion is EC08, because a timely reminder is not helpful if it encourages inappropriate watering or fertilising. It must be paired with EC05 and EC06: the explanation should be correct and verifiable. Identification accuracy alone cannot establish a good care experience, and an accepted email does not prove that a user received it or performed the care task.
