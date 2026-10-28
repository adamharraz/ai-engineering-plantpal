# Week 1 Project Scope - PlantPal

Version: 1.0 | Individual semester project | Updated: 28 October 2026

## 1. Project Name and Domain

**PlantPal - AI Plant Identification and Personal Care Assistant**

Domain: indoor ornamental plant care and personal plant management. Proposed platform: a mobile-friendly web application for beginner owners in Malaysia. This document defines a preliminary semester project; application development and evaluation have not yet been performed.

## 2. Problem Statement

Beginner indoor-plant owners need to identify a plant, interpret its care instructions and remember what care they have already provided. A photograph alone does not reveal soil moisture or the growing environment, while a calendar alone cannot determine whether watering is needed. PlantPal proposes one workflow for tentative photo identification, a user-confirmed plant collection, care records, editable reminders and explanations grounded in reviewed documents. Its initial audience is beginners in Malaysia.

AI suggests plant names from photos and explains information from the care guides. Normal application code manages collections, care records and reminders. The project will compare AI answers with a basic keyword search to check whether the AI adds value.

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

PlantPal will bring photo identification, a personal plant collection, care reminders and clear care advice into one app. Users will confirm the suggested plant name before receiving guidance for it.

Similar apps already exist. This project will focus on making these features work well together and testing whether its AI answers are more helpful than searching the same documents.

## 6. Initial Project Objectives

- **O1 – Design:** Plan a system for three plant types that connects photo identification, user confirmation, saved plant records and care guidance from trusted sources.
- **O2 – Develop:** Build a mobile-friendly English web app where users can identify plants, save them, set care reminders, record care activities and ask questions.
- **O3 – Evaluate:** Test identification accuracy, answer correctness, source links, privacy, reminders, response speed and running costs. Compare the care assistant with a basic keyword search.

## 7. Target Users and Initial Scope

PlantPal is intended for beginner indoor-plant owners in Malaysia, including students and people living in small homes.

The first version will use English and support three plants:

- **Golden pothos / money plant** (*Epipremnum aureum*)
- **Snake plant** (*Dracaena trifasciata*, also called *Sansevieria trifasciata*)
- **Spider plant** (*Chlorophytum comosum*)

The app will ask about the plant’s growing conditions because lighting, pots and care needs can differ between homes.

### How PlantPal will work

1. **Identify a plant:** Take or upload a photo. Review the photo and agree to send it for identification. The AI will suggest possible plant names.
2. **Confirm and save:** Confirm or correct the plant name. Choose **Add to My Plants** to save it with a nickname and an optional photo.
3. **Set reminders:** Provide basic details about lighting and pot drainage. Choose when to receive watering-check reminders. Fertilising reminders remain off until the user sets them.
4. **Track plant care:** Record watering, fertilising or skipped tasks. If the soil is still damp, postpone the reminder. Users can change, pause or turn off reminders. Email reminders will work even when the app is closed.
5. **Ask questions:** Ask about watering, lighting, drainage or repotting. Answers will include links to the supporting sources.

AI will suggest plant names and explain care information. The app will manage saved plants, care records and reminders.

A photo cannot tell whether the soil needs water. Reminders will therefore ask users to **check their plant before taking action**.

The first version will not diagnose diseases, recommend chemical doses, control watering equipment or use sensors. It will be a web app rather than a separate mobile app.

### Additional scope details

Saving a plant is optional. Manual selection is available if photo identification fails. Care plans are offered only for a user-confirmed supported plant. Saved collections are private; users can delete their records and stop reminders. The first version uses the Asia/Kuala_Lumpur timezone.

The app will not give pesticide advice, medical or veterinary advice, or decide whether a plant is safe to eat. Weather integrations, social sharing and model training are outside the first version. Fertilising intervals are entered by the user using reliable guidance or the product label; the app does not calculate a dose.

## 8. Public Data Sources

PlantPal will use three public care guides published by **54 Weng Thye Nursery in Sungai Buloh, Selangor**.

| Public document | Information PlantPal can use |
|---|---|
| [Money Plant Care Guide](https://wengthye.com/careguide/money-plant) | Lighting, watering, soil and feeding guidance for golden pothos. |
| [Snake Plant Care Guide](https://wengthye.com/careguide/snake-plant) | Lighting, watering, drainage and feeding guidance for snake plants. |
| [Spider Plant Care Guide](https://wengthye.com/careguide/spider-plant) | Lighting, watering, potting soil and feeding guidance for spider plants. |

These guides provide a small starting collection of information for the three supported plants. Suggested watering intervals will be treated as starting points. Users will still be reminded to check their plant and soil before watering.

These are commercial nursery guides, so their advice will be checked against an independent horticultural reference before being used in the app.

The two university PDFs already prepared locally will remain as supporting references for the lab’s PDF preparation requirement. Source links and details will be recorded in GitHub. Full documents and images will only be uploaded if their licences allow sharing.

The three main care documents are the Malaysian guides above. Their IDs and URLs are recorded in [sources.csv](../data/sources.csv). The two university PDFs are supporting references for independent checking and lab preparation, listed separately in [supporting_references.csv](../data/supporting_references.csv). They are not automatically included in the searchable care collection.

Before using a guide, review the advice, its licence and the species it applies to. Keep the source URL, heading, review date and approved passage ID with each care statement. Treat suggested intervals as starting points, not as measurements of a plant's needs. Exclude disease-treatment and chemical-dose instructions, shopping content and claims outside the project scope.

Basic repotting remains a question topic. If the reviewed guides do not support an answer, PlantPal will explain that it does not have enough information and point the user to a supporting reference. It will not invent instructions or silently add supporting documents to its knowledge base. See [data preparation notes](../data/README.md).

### Photo identification service

PlantPal will use an existing AI identification service rather than train a new model.

[Pl@ntNet](https://docs.plantnet.org/en/reference/api-plantnet/) is one possible service. Its access terms, costs and privacy practices will be checked before integration.

Identification results will be shown as suggestions. Users must confirm or correct the plant name before receiving a care plan.

Testing will use photos taken with permission or images whose licences allow their use. Private user photos will not be uploaded to the public GitHub repository.

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

The original group observation sheet and conceptual ChatGPT system map remain Week 1 Part A/B evidence. They are separate from PlantPal’s architecture. [Week 2 requirements](week2_requirements.md) provide the current, more specific acceptance targets.
