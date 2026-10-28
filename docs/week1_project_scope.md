# Week 1 Project Scope - PlantPal

Version: 1.0 | Individual semester project | Updated: 28 October 2026

## 1. Project Name and Domain

**PlantPal - AI Plant Identification and Personal Care Assistant**

Domain: indoor ornamental plant care and personal plant management. Proposed platform: a mobile-friendly web application for beginner owners in Malaysia. This document defines a preliminary semester project; application development and evaluation have not yet been performed.

## 2. Problem Statement

Beginner indoor-plant owners need to identify a plant, interpret its care instructions and remember what care they have already provided. A photograph alone does not reveal soil moisture or the growing environment, while a calendar alone cannot determine whether watering is needed. PlantPal proposes one workflow for tentative photo identification, a user-confirmed plant collection, care records, editable reminders and explanations grounded in reviewed documents. Its initial audience is beginners in Malaysia; Malaysian demand and time savings have not yet been measured.

AI is justified for photo-based candidate identification and interpreting paraphrased questions against care documents. Collection storage, care logs and scheduled reminders use ordinary application logic. The language model will explain evidence; it will not determine that a plant needs water from a photograph or execute care actions. A keyword-search baseline will test whether generation adds value.

## 3. Evidence of the Problem

- [University of Illinois Extension, Watering](https://extension.illinois.edu/houseplants/watering) describes the difficulty of under- and overwatering and explains that plant and environmental conditions affect water needs. This supports moisture-check reminders rather than automatic instructions to water on a date.
- [University of Tennessee Extension, Caring for Indoor Plants, W 1128-B](https://uthort.tennessee.edu/wp-content/uploads/sites/228/2023/11/W1128B.pdf), especially the watering and repotting sections, documents how care depends on containers, growing media and conditions. This supports collecting a small amount of context and explaining care steps.

These sources establish practical care difficulties, not the prevalence of the problem among Malaysian students. Forgetfulness, demand for a collection feature and improved usefulness are project hypotheses. A later small task-based usability study can investigate them; no interviews or outcomes are claimed here.

## 4. Initial Related Work and Existing Approaches

| Study / existing solution | What it already addresses | Limitation relevant to PlantPal |
|---|---|---|
| [Pl@ntNet identification service](https://docs.plantnet.org/en/reference/api-plantnet/) | Image-based identification with ranked candidate species and confidence scores | A candidate is not a verified identity or a measurement of soil moisture. PlantPal still needs confirmation, supported-species checks and a care workflow. |
| [Planta](https://getplanta.com/) | Its official product page advertises identification, care reminders and plant organisation | Similar features already exist. That page does not establish performance for PlantPal's three-species test set or the proposed citation and reminder controls; no claim is made that Planta lacks these controls. |
| [University of Illinois Extension houseplant guidance](https://extension.illinois.edu/houseplants/watering) | Expert-authored care information for readers | The inspected guidance is a reference document; maintaining a personal collection and tracking due tasks remain application work. Some guidance needs climate and species review. |
| [Lewis et al. (2020), Retrieval-Augmented Generation for Knowledge-Intensive NLP Tasks](https://arxiv.org/abs/2005.11401) | Studies combining retrieved knowledge and language generation | Its results do not validate PlantPal's images, horticultural corpus or care safety. The proposed retrieval-and-generation pipeline requires its own evaluation. |

## 5. Engineering Gap or Limitation

The bounded engineering gap is to integrate uncertain photo identification with confirmed species, private care records, user-controlled check reminders and verifiable explanations for a small indoor-plant corpus. The project will test whether these parts work reliably together and whether the care assistant improves on keyword search over the same sources. Existing apps already cover much of this workflow, so this is an integration and evaluation project rather than a claim of scientific novelty or a first-of-its-kind product.

## 6. Initial Project Objectives

- **O1 - Design:** Define a three-species knowledge base and a traceable workflow connecting tentative identification, user confirmation, personal records and source-backed care checks.
- **O2 - Develop:** Implement a mobile-friendly English web prototype with photo suggestions, optional collection saving, editable watering-check and fertilising reminders, care history and cited care Q&A.
- **O3 - Evaluate:** Assess identification, answer correctness, source support, collection isolation, reminder behaviour, latency and operating cost against frozen reference cases and a keyword-search baseline for Q&A.

## 7. Target Users and Initial Scope

Primary users are beginner indoor-plant owners in Malaysia, including students and people keeping a few potted plants in rooms or small homes. The first version uses English, user-reported indoor conditions and the Asia/Kuala_Lumpur timezone. It does not assume every Malaysian room has the same light, temperature or humidity.

| Supported plant | Canonical name / aliases | Evidence handling |
|---|---|---|
| Golden pothos / money plant | *Epipremnum aureum* | Match the botanical name; do not treat every plant called money plant or silver pothos as this species. |
| Snake plant | *Dracaena trifasciata*; synonym *Sansevieria trifasciata* | Normalise the synonym to the same supported species. |
| Spider plant | *Chlorophytum comosum* | Support this species; do not infer that genus-level guidance covers every related species. |

These are the three V1 care species. An identification provider may recognise more plants; PlantPal must label other results as outside its reviewed care coverage. A user can request another photo or manually choose a supported identity, with the choice recorded as user-confirmed rather than botanically verified.

### Proposed user journey

1. Take or upload a photo of one plant, preview it and consent to an identification request.
2. Inspect ranked suggestions and uncertainty, then confirm/correct a supported species. Manual selection is available when identification fails.
3. Choose **Add to My Plants**; identify-only use does not silently save the plant.
4. Give the plant a nickname and optionally retain the photo; provide light and drainage context. No GPS is required.
5. Approve an editable watering-check interval and optional fertilising reminder. A reminder is a check, not a claim of measured need.
6. Use an in-app due list and opt-in email reminders, including while the browser is closed. Record watered, still damp, skipped or fertilised; snooze, pause or disable as needed.
7. Ask care questions and inspect the supporting source links. Delete the plant and its associated schedules/history when no longer needed.

### Boundaries

V1 excludes automatic disease diagnosis, photo-based moisture or nutrient measurement, pesticides, fertiliser dose calculation, medical/veterinary advice, edible-plant identification, automatic watering hardware, weather integrations, model training, social features and native app-store releases. Symptoms may be discussed as uncertain possibilities with checks, never as a confirmed disease. Fertilising intervals are user-entered from the product label or reliable guidance and remain disabled until explicitly configured. Accounts and private ownership controls support collections; a full social or administration platform is outside scope.

## 8. Public Data Sources

| ID | Public document | Intended use / limitation |
|---|---|---|
| UT-A | [Selecting Indoor Plants for the Home or Office (W 1128-A)](https://uthort.tennessee.edu/wp-content/uploads/sites/228/2023/11/W1128A.pdf) - University of Tennessee Extension; PDF | Species tables and indoor environment; US seasonal and window-aspect advice requires review |
| UT-B | [Caring for Indoor Plants (W 1128-B)](https://uthort.tennessee.edu/wp-content/uploads/sites/228/2023/11/W1128B.pdf) - University of Tennessee Extension; PDF | Watering, containers, media and repotting; exclude chemical preparation and dosing instructions |
| IL-W | [Watering](https://extension.illinois.edu/houseplants/watering) - University of Illinois Extension; HTML | General water-need factors; do not apply one moisture depth universally |
| IL-P | [Pothos](https://extension.illinois.edu/houseplants/pothos) - University of Illinois Extension; HTML | Separate devil's ivy from the silver-pothos material on this page |
| IL-S | [Mother-in-Law Tongue](https://extension.illinois.edu/houseplants/mother-law-tongue) - University of Illinois Extension; HTML | Snake-plant care; do not copy a winter calendar to Malaysia |
| IL-C | [Spider Plant](https://extension.illinois.edu/houseplants/spider-plant) - University of Illinois Extension; HTML | Spider-plant care; resolve broad dryness wording against growth context |
| NP-P | [Epipremnum aureum species profile](https://www.nparks.gov.sg/florafaunaweb/flora/1/3/1390) - Singapore National Parks Board; HTML | Regional taxonomy, cultivation and handling cautions; not a Malaysian field trial |
| NP-S | [Dracaena trifasciata species profile](https://www.nparks.gov.sg/florafaunaweb/flora/2/4/2420) - Singapore National Parks Board; HTML | Regional taxonomy and cultivation; landscape sun labels need indoor interpretation |

Sources checked on 28 October 2026. These are candidate source documents selected for the preliminary design; a reviewed ingestion release has not yet been built. Record document ID, publisher, botanical species, topic, section/page, climate context, URL, retrieval date, rights status and release version before activation. General guidance must not override species-specific evidence silently. Where wording conflicts, preserve context and ask for clarification or state that the evidence is insufficient.

The US documents contain seasonal assumptions; the Singapore profiles provide regional context but do not validate a Malaysian watering schedule. No exact watering or fertiliser interval will be presented as derived from a photograph. Public accessibility does not establish redistribution permission. The two PDFs are prepared locally for course review; the public repository records links and metadata rather than redistributing their text or images. See [data preparation notes](../data/README.md).

Identification is a separate service dependency. [Pl@ntNet's API documentation](https://docs.plantnet.org/en/reference/api-plantnet/) is a feasibility reference, not an activated integration or care source. Verify access, supported names, attribution, current terms, quotas and retention before choosing a provider. Evaluation photographs must be self-taken with a known plant label or licensed with recorded attribution; no user photos or unlicensed reference photos belong in this public repository.

## 9. Comparison with EduAdvisor

Both projects use a bounded document collection, source-backed explanations, scope controls and measurable evaluation. EduAdvisor concerns courses, prerequisites and graduation rules. PlantPal adds uncertain image identification, explicit species confirmation, a private collection, care history and opt-in reminders. Its evidence must preserve species and growing context, while the scheduler must stay separate from generated language. Neither citations nor user confirmation alone guarantee correctness.

## 10. Responsible AI Risks

| Risk | Potential impact | Planned control |
|---|---|---|
| Wrong or overconfident identification | Care for the wrong species | Ranked suggestions, uncertainty disclosure, explicit user confirmation and manual correction; never force an unsupported image into one of the three plants. |
| Incorrect care or mixed climate/species evidence | Plant damage from inappropriate care | Reviewed sources and citations, context checks, no unsupported diagnosis or chemical doses, and watering-check wording. |
| Over-reliance on reminders | Watering when soil is still wet or unnecessary fertilising | User-confirmed intervals, moisture checks, snooze/skip, care history and optional fertilising reminders. |
| Photo and account privacy | Disclosure of home interiors, location metadata or another user's records | Preview and consent before identification, metadata removal, private ownership checks, minimal retention and deletion controls. |
| Unwanted or duplicate notifications | Disturbance and reduced trust | Explicit opt-in, pause/disable, deduplication and final consent/ownership checks before sending. |
| Instructions hidden in source text or user input | Unsupported advice or unauthorised changes | Treat text as data; the language model has no permission to change collections, schedules or send messages. |

## Preliminary Evaluation and Reflection

Week 2 will refine the targets into testable requirements and linked evaluation criteria. Planned core sets include 30 labelled photos (10 per supported species) and 30 source-answerable care questions (10 per species), plus uncertainty, privacy and reminder-state scenarios. No measured results are claimed. This small sample supports a prototype assessment, not broad botanical reliability or a claim of improved survival.

The main design lesson is that identification and care are different tasks. A plausible species suggestion should lead to confirmation and reviewed guidance, while a due date should lead to a check rather than automatic watering. This distinction keeps the project useful without implying knowledge that a photograph cannot supply.

The original group observation sheet and conceptual ChatGPT system map remain Week 1 Part A/B evidence. They are not PlantPal's architecture; known gaps in the supplied observation sheet remain disclosed. [Week 2 requirements](week2_requirements.md) provide the current, more specific acceptance targets.
