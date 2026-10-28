# PlantPal - AI Plant Identification and Personal Care Assistant

> A proposed mobile-friendly web app for beginner plant owners in Malaysia: identify a plant provisionally, optionally save it to a private collection, track care, receive editable check reminders and ask questions supported by gardening documents.

**Domain:** Indoor plant care and personal plant management.

**Stage:** Week 1 scope and Week 2 requirements, evaluation plan and V1 architecture. The application has not been implemented or evaluated.

**Submission:** Individual AI Engineering semester project. Updated 28 October 2026.

## Problem Statement

Beginner indoor-plant owners need to identify a plant, interpret its care instructions and remember what care they have already provided. A photograph alone does not reveal soil moisture or the growing environment, while a calendar alone cannot determine whether watering is needed. PlantPal proposes one workflow for tentative photo identification, a user-confirmed plant collection, care records, editable reminders and explanations grounded in reviewed documents. Its initial audience is beginners in Malaysia.

## Evidence of the Problem

- [University of Illinois Extension, Watering](https://extension.illinois.edu/houseplants/watering) describes the difficulty of under- and overwatering and explains that plant and environmental conditions affect water needs. This supports moisture-check reminders rather than automatic instructions to water on a date.
- [University of Tennessee Extension, Caring for Indoor Plants, W 1128-B](https://uthort.tennessee.edu/wp-content/uploads/sites/228/2023/11/W1128B.pdf), especially the watering and repotting sections, documents how care depends on containers, growing media and conditions. This supports collecting a small amount of context and explaining care steps.

These sources establish practical care difficulties, not the prevalence of the problem among Malaysian students. Forgetfulness, demand for a collection feature and improved usefulness are project hypotheses. A later small task-based usability study can investigate them; no interviews or outcomes are claimed here.

## Initial Related Work and Existing Approaches

| Study / existing solution | What it already addresses | Limitation relevant to PlantPal |
|---|---|---|
| [Pl@ntNet identification service](https://docs.plantnet.org/en/reference/api-plantnet/) | Image-based identification with ranked candidate species and confidence scores | A candidate is not a verified identity or a measurement of soil moisture. PlantPal still needs confirmation, supported-species checks and a care workflow. |
| [Planta](https://getplanta.com/) | Its official product page advertises identification, care reminders and plant organisation | Similar features already exist. That page does not establish performance for PlantPal's three-species test set or the proposed citation and reminder controls; no claim is made that Planta lacks these controls. |
| [University of Illinois Extension houseplant guidance](https://extension.illinois.edu/houseplants/watering) | Expert-authored care information for readers | The inspected guidance is a reference document; maintaining a personal collection and tracking due tasks remain application work. Some guidance needs climate and species review. |
| [Lewis et al. (2020), Retrieval-Augmented Generation for Knowledge-Intensive NLP Tasks](https://arxiv.org/abs/2005.11401) | Studies combining retrieved knowledge and language generation | Its results do not validate PlantPal's images, horticultural corpus or care safety. The proposed retrieval-and-generation pipeline requires its own evaluation. |

## Engineering Gap or Limitation

PlantPal will bring photo identification, a personal plant collection, care reminders and clear care advice into one app. Users will confirm the suggested plant name before receiving guidance for it.

Similar apps already exist. This project will focus on making these features work well together and testing whether its AI answers are more helpful than searching the same documents.

## Initial Project Objectives

- **O1 – Design:** Plan a system for three plant types that connects photo identification, user confirmation, saved plant records and care guidance from trusted sources.
- **O2 – Develop:** Build a mobile-friendly English web app where users can identify plants, save them, set care reminders, record care activities and ask questions.
- **O3 – Evaluate:** Test identification accuracy, answer correctness, source links, privacy, reminders, response speed and running costs. Compare the care assistant with a basic keyword search.

## Target Users and Supported Plants

PlantPal is intended for beginner indoor-plant owners in Malaysia, including students and people living in small homes.

The first version will use English and support three plants:

- **Golden pothos / money plant** (*Epipremnum aureum*)
- **Snake plant** (*Dracaena trifasciata*, also called *Sansevieria trifasciata*)
- **Spider plant** (*Chlorophytum comosum*)

The app will ask about the plant’s growing conditions because lighting, pots and care needs can differ between homes.

## How PlantPal Will Work

1. **Identify a plant:** Take or upload a photo. Review the photo and agree to send it for identification. The AI will suggest possible plant names.
2. **Confirm and save:** Confirm or correct the plant name. Choose **Add to My Plants** to save it with a nickname and an optional photo.
3. **Set reminders:** Provide basic details about lighting and pot drainage. Choose when to receive watering-check reminders. Fertilising reminders remain off until the user sets them.
4. **Track plant care:** Record watering, fertilising or skipped tasks. If the soil is still damp, postpone the reminder. Users can change, pause or turn off reminders. Email reminders will work even when the app is closed.
5. **Ask questions:** Ask about watering, lighting, drainage or repotting. Answers will include links to the supporting sources.

AI will suggest plant names and explain care information. The app will manage saved plants, care records and reminders.

A photo cannot tell whether the soil needs water. Reminders will therefore ask users to **check their plant before taking action**.

The first version will not diagnose diseases, recommend chemical doses, control watering equipment or use sensors. It will be a web app rather than a separate mobile app.

## Proposed Data Sources

PlantPal will use three public care guides published by **54 Weng Thye Nursery in Sungai Buloh, Selangor**.

| Public document | Information PlantPal can use |
|---|---|
| [Money Plant Care Guide](https://wengthye.com/careguide/money-plant) | Lighting, watering, soil and feeding guidance for golden pothos. |
| [Snake Plant Care Guide](https://wengthye.com/careguide/snake-plant) | Lighting, watering, drainage and feeding guidance for snake plants. |
| [Spider Plant Care Guide](https://wengthye.com/careguide/spider-plant) | Lighting, watering, potting soil and feeding guidance for spider plants. |

These guides provide a small starting collection of information for the three supported plants. Suggested watering intervals will be treated as starting points. Users will still be reminded to check their plant and soil before watering.

These are commercial nursery guides, so their advice will be checked against an independent horticultural reference before being used in the app.

The two university PDFs already prepared locally will remain as supporting references for the lab’s PDF preparation requirement. The three main guides are recorded in [data/sources.csv](data/sources.csv), and the two PDFs are listed separately in [data/supporting_references.csv](data/supporting_references.csv). Full documents and images will only be uploaded if their licences allow sharing.

Repotting answers will be given only when the approved guides provide enough information. Otherwise, PlantPal will explain the information gap and point to a supporting reference.

## Photo Identification Service

PlantPal will use an existing AI identification service rather than train a new model.

[Pl@ntNet](https://docs.plantnet.org/en/reference/api-plantnet/) is one possible service. Its access terms, costs and privacy practices will be checked before integration.

Identification results will be shown as suggestions. Users must confirm or correct the plant name before receiving a care plan.

Testing will use photos taken with permission or images whose licences allow their use. Private user photos will not be uploaded to the public GitHub repository.

## Responsible AI and Evaluation

Main risks are incorrect identification, unsupported care advice, over-reliance on reminders, photo/collection privacy and unwanted notifications. Planned controls include confirmation, limited care coverage, citations, uncertainty, owner checks, image-metadata removal, short temporary retention, explicit notification consent and read-only AI. See [requirements](docs/week2_requirements.md).

Planned core tests use 30 labelled photos and 30 answerable questions, ten per species, plus uncertainty, collection, notification, privacy and injection cases. Targets include at least 24/30 top-3 identification matches, at least 24/30 correct care answers and at least 95% factual-claim support. These are acceptance targets, not results. The [evaluation plan](docs/week2_evaluation_criteria.md) defines workloads and a keyword-search baseline.

## Comparison with EduAdvisor

Both projects use bounded documents, cited answers, controls and evaluation. PlantPal concerns indoor plants and adds provisional photo identification, personal care records and opt-in reminders. EduAdvisor concerns academic information. PlantPal's scheduler is separate from the language model.

## Repository Structure

```text
ai-engineering-plantpal/
|-- README.md
|-- docs/
|   |-- week1_observation_sheet.md
|   |-- week1_system_map.png
|   |-- week1_project_scope.md
|   |-- week2_requirements.md
|   |-- week2_evaluation_criteria.md
|   |-- week2_architecture_v1.png
|   |-- week2_architecture_v1.drawio
|   `-- week2_architecture_decisions.md
|-- data/          # Source URLs, metadata and preparation notes
|-- src/           # .gitkeep; implementation later
|-- notebooks/     # .gitkeep; course notebooks later
|-- evaluation/    # .gitkeep; measured evidence later
`-- prompts/       # .gitkeep; prompts later
```

## Weekly Progress

| Week | Focus | Deliverables | Status |
|---|---|---|---|
| W1 | Project framing and repository setup | Scope, README, group evidence copies and required folders | Individual Part C reframed for PlantPal. Original group files retained; observation-sheet evidence gaps remain. |
| W2 | Requirements, evaluation and conceptual architecture | Four required files, editable Draw.io diagram, decisions, traceability and reflections | Planning documents prepared for PlantPal; implementation and evaluation runs remain future work. |

Moodle submission is handled by the student. This is the same semester repository originally used for the scholarship project; the project change preserves Git history.

## Week 1 Group Evidence Status

The observation sheet and ChatGPT conceptual map are copies of the original Part A/B group activity, not PlantPal's system design. The supplied observation sheet still needs verified test responses, 2-3 pieces of behavioural evidence, group names and a real other-group observation. File presence alone does not establish that this group evidence is complete. Those findings have not been invented.

## Deliverable Links

- [Week 1 project scope](docs/week1_project_scope.md)
- [Week 2 requirements, Sections 0-3](docs/week2_requirements.md)
- [Week 2 evaluation criteria](docs/week2_evaluation_criteria.md)
- [V1 architecture PNG](docs/week2_architecture_v1.png) and [editable Draw.io source](docs/week2_architecture_v1.drawio)
- [Architecture decisions, trade-offs and traceability](docs/week2_architecture_decisions.md)

Keep API keys, passwords, email destinations, household photos, private conversations and restricted documents out of GitHub. No application deployment, notification account or paid service is created by this planning submission.
