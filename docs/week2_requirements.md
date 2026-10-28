# Requirements Specification - PlantPal

## Section 0 - Project Framing and Application Scope Checkpoint

### Problem and evidence

Beginner indoor-plant owners in Malaysia need help identifying plants, understanding care instructions and remembering care tasks. PlantPal will use AI to suggest plant names from photos and explain care information from reviewed documents. Users can confirm a plant, optionally save it to a private collection, record care and receive editable reminders to check their plants.

- [University of Illinois Extension, Watering](https://extension.illinois.edu/houseplants/watering) explains how under- and overwatering harm houseplants and how water needs vary with the plant and its environment.
- [University of Tennessee Extension, Caring for Indoor Plants, W 1128-B](https://uthort.tennessee.edu/wp-content/uploads/sites/228/2023/11/W1128B.pdf) explains how containers, growing media and conditions affect care.

These sources support the care problem. Demand among Malaysian users and any improvement from PlantPal still need to be tested.

### Existing approaches

| Study / existing solution | Existing approach | Limitation relevant to PlantPal |
|---|---|---|
| [Pl@ntNet](https://docs.plantnet.org/en/reference/api-plantnet/) | Suggests species from plant images. | A suggested name still needs confirmation and does not show soil moisture or care needs. |
| [Planta](https://getplanta.com/) | Offers plant identification, care reminders and plant organisation. | Similar features already exist; PlantPal must evaluate its own three-species coverage, source support and reminder controls. |
| [University of Illinois Extension houseplant guidance](https://extension.illinois.edu/houseplants/watering) | Provides expert plant-care information. | Readers still need to find relevant advice and manage their own records and reminders; some advice needs local-context review. |

### Engineering gap

PlantPal will bring photo identification, a personal plant collection, care reminders and clear care advice into one app. Users will confirm the suggested plant name before receiving guidance for it.

Similar apps already exist. This project will focus on making these features work well together and testing whether its AI answers are more helpful than searching the same documents.

### Objectives

- **O1 – Design:** Plan a system for three plant types that connects photo identification, user confirmation, saved plant records and care guidance from trusted sources.
- **O2 – Develop:** Build a mobile-friendly English web app where users can identify plants, save them, set care reminders, record care activities and ask questions.
- **O3 – Evaluate:** Test identification accuracy, answer correctness, source links, privacy, reminders, response speed and running costs. Compare the care assistant with a basic keyword search.

### Application scope

| Framing element | V1 scope |
|---|---|
| Intended users | Beginner indoor-plant owners in Malaysia; English interaction. |
| Supported tasks | Photo suggestions, species confirmation, optional private collection, care history, editable watering-check and optional fertilising reminders, and source-supported care questions. |
| Information / knowledge domain | Golden pothos (*Epipremnum aureum*), snake plant (*Dracaena trifasciata*, synonym *Sansevieria trifasciata*) and spider plant (*Chlorophytum comosum*). Topics include watering, lighting, drainage, general feeding and repotting when approved sources support an answer. |
| Operating context | Mobile-friendly web app with internet access, an in-app due list and opt-in email reminders that work with the browser closed. Use Asia/Kuala_Lumpur time. Sign-in is required for saved collections. |
| Important constraints | Individual semester prototype using an existing identification service and three reviewed Malaysian care guides. Check provider access, source rights and budget before use. Watering reminders ask users to check moisture; fertilising reminders start disabled. |
| Explicit out-of-scope tasks | Disease diagnosis, chemical doses, ingestion or pet-safety decisions, sensors, automatic watering, new model training and a native mobile app. |
| Evaluation focus | Do identification suggestions, care answers, private records and reminders meet the agreed targets? Does the care assistant help more than keyword search over the same approved documents? |

Care sources: [money plant](https://wengthye.com/careguide/money-plant), [snake plant](https://wengthye.com/careguide/snake-plant) and [spider plant](https://wengthye.com/careguide/spider-plant). Review the nursery advice before use. The two university PDFs prepared for the lab remain supporting references rather than automatically becoming searchable care sources.

## Section 1 - Functional Requirements

| ID | Requirement - The system shall | Scope boundary - shall not / condition |
|---|---|---|
| FR1 | Accept one JPEG/PNG photo up to 5 MB and show up to three ranked plant suggestions with scores and an uncertainty notice, or an error message. | Do not force an unsupported plant into the three supported species or present a score as guaranteed accuracy. |
| FR2 | Require the user to confirm or correct a supported plant name before setting up care; provide manual selection and a separate Add to My Plants action. | Identification alone must not save a plant or confirm its identity. |
| FR3 | Let a signed-in owner add, view, edit and remove plants with a nickname, confirmed species, optional photo and room label, and care history. | Another user must not access the collection. Saving is optional. |
| FR4 | Ask about light and drainage, allow unknown answers, and show source-supported guidance and an editable check schedule for approval. | Do not infer moisture or growing conditions from a photo. |
| FR5 | Let owners set, change, snooze, pause or disable watering-check reminders and optional fertilising reminders. | Validate dates and intervals; keep fertilising disabled until configured. Reminders must ask users to check conditions before acting. |
| FR6 | Record watered, still damp, skipped and fertilised events with timestamps and show the next check. | Record each action once. Watered/fertilised uses the approved interval for the next check; still damp or skipped asks for a snooze date or pause. |
| FR7 | Show due tasks and send opted-in email reminders, including with the browser closed. | Check verified destination, current consent, due state and event ID before sending. Do not mark care completed because an email was sent. |
| FR8 | Answer English care questions for supported plants using reviewed passages and source links. | Do not invent advice; answer repotting questions only when approved passages support them. |
| FR9 | Ask for missing context, explain insufficient or conflicting evidence, and redirect unsupported requests. | Do not give disease certainty, chemical doses, ingestion treatment or edible/pet-safe assurances. |
| FR10 | Restrict records to their owner and allow plant/account deletion and notification opt-out. | Deleting or pausing a plant must cancel unsent reminders; changing species must pause reminders until care settings are reviewed. |
| FR11 | Let a maintainer review and activate updated care passages with source links, species and context details. | Do not automatically use unreviewed web content or unresolved conflicting advice. |

## Section 2 - Non-Functional Requirements

| ID | Quality attribute | Requirement | Measurement method | Metric | Threshold |
|---|---|---|---|---|---|
| NFR1 | Identification quality | Suggestions shall meet the labelled-photo target. | Test 30 labelled photos, ten per supported species, using fixed service settings. | Correct species in top three suggestions. | At least 24/30 overall and 7/10 per species. |
| NFR2 | Care correctness | Answers shall match reviewed facts and the supplied context. | Score 30 source-answerable questions, ten per species; pass only if required facts are correct and no unsupported advice is added. | Correct answers overall and per species. | At least 24/30 overall and 7/10 per species. |
| NFR3 | Performance | Identification and care answers shall respond within the agreed time. | Measure submission-to-result time for three runs of the 30 photos and 30 questions; report failures separately. | p90 time for each path. | Photo at most 20 seconds; care answer at most 15 seconds. |
| NFR4 | Reminder reliability | Due reminders shall be sent once within the timing tolerance. | Test 20 due/not-due, restart and retry cases with a controlled clock and test email service. | Correct unique events; dispatch delay. | 20/20 correct; at least 19/20 within two minutes; zero early or duplicate sends. |
| NFR5 | Cost | External inference shall stay within the prototype budget. | Record 50 identification and 50 care requests, including retries and retrieval charges, using dated prices. | USD per 100 mixed requests. | At most US$2 for paid and list-price-equivalent inference cost; report hosting/email separately. |
| NFR6 | Maintainability | Approved source changes shall update relevant guidance without breaking unrelated answers. | Replace one reviewed fact and test one changed question plus ten unaffected questions. | Activation time and correct answers. | At most ten minutes after approval; 1/1 changed and 10/10 unaffected answers correct. |

## Section 3 - Responsible AI Requirements

| ID | Responsible AI risk | Requirement / control | Measurement method | Metric | Threshold |
|---|---|---|---|---|---|
| RAI1 | Wrong identity and false confidence | Show uncertainty and require confirmation of a supported species before care setup. | Test ten difficult/unsupported image cases and ten missing/conflicting-information cases through the app and direct requests. | Appropriate clarification or refusal; unconfirmed activations. | At least 18/20 appropriate, including 9/10 in each subset; zero unconfirmed activations. |
| RAI2 | Unsupported care advice | Cite approved evidence and explain when information is insufficient. | Check citations and every factual care claim in the 30 care answers against its source and species/context. | Citation presence and supported-claim rate. | 100% citation presence; at least 95% factual claims supported. |
| RAI3 | Harmful care and over-reliance | Use care-check wording and avoid disease certainty, chemical doses and ingestion treatment. | Test 20 requests covering these boundaries and review all reminder templates. | Compliant cases and prohibited instructions. | 20/20 cases comply; zero prohibited instructions in answers or templates. |
| RAI4 | Photo and account privacy | Obtain photo consent, remove location metadata, limit sharing to its purpose and enforce owner-only access and deletion. | Test 20 synthetic consent, metadata, outgoing-data, ownership and deletion cases using two accounts. | Passed cases; data leaks; deletion time. | 20/20 pass; zero leaks or cross-owner access; temporary photos deleted within 15 minutes and active saved records within 24 hours of deletion. |
| RAI5 | Unwanted notifications | Honour current opt-in, changes, pause and deletion before sending. | Test 20 consent, edit-before-send, pause, deletion and retry cases. | Correct send/block decisions; unwanted sends. | 20/20 correct; zero stale, duplicate or unauthorised sends. |
| RAI6 | Hidden instructions and unwanted actions | Treat source/input text as data and prevent AI from editing records or sending messages. | Test 20 injection attempts: five each in questions, source passages, notes and image-related text; inspect record and mail effects. | Boundary-preserving responses; unauthorised actions. | At least 19/20 preserve boundaries; zero unauthorised writes or sends. |

For RAI4, consented photos go only to the identification service; email addresses go only to the email service for opted-in delivery. Keep these, room labels and private notes out of language-model input and raw diagnostic logs. Remove or block personal details in questions before external AI calls. Saved photos require a separate save choice; backup expiry must be capped at 30 days, and provider retention must be checked before use.

## Reflection

RAI2 will be the hardest requirement to measure. A working source link does not prove that the advice matches the passage or applies to the selected plant and growing conditions. I will check individual claims against the sources and explain uncertainty when the evidence conflicts. This needs human review as well as automatic source-link checks.
