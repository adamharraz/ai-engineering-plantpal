# PlantPal - AI Plant Identification and Personal Care Assistant

> A proposed mobile-friendly web app for beginner plant owners in Malaysia: identify a plant provisionally, optionally save it to a private collection, track care, receive editable check reminders and ask questions supported by gardening documents.

**Domain:** Indoor plant care and personal plant management.

**Stage:** Week 1 scope and Week 2 requirements, evaluation plan and V1 architecture. The application has not been implemented or evaluated.

**Submission:** Individual AI Engineering semester project. Updated 28 October 2026.

## Problem Statement

Beginner indoor-plant owners need to identify a plant, interpret its care instructions and remember what care they have already provided. A photograph alone does not reveal soil moisture or the growing environment, while a calendar alone cannot determine whether watering is needed. PlantPal proposes one workflow for tentative photo identification, a user-confirmed plant collection, care records, editable reminders and explanations grounded in reviewed documents. Its initial audience is beginners in Malaysia; Malaysian demand and time savings have not yet been measured.

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

The bounded engineering gap is to integrate uncertain photo identification with confirmed species, private care records, user-controlled check reminders and verifiable explanations for a small indoor-plant corpus. The project will test whether these parts work reliably together and whether the care assistant improves on keyword search over the same sources. Existing apps already cover much of this workflow, so this is an integration and evaluation project rather than a claim of scientific novelty or a first-of-its-kind product.

## Initial Project Objectives

- **O1 - Design:** Define a three-species knowledge base and a traceable workflow connecting tentative identification, user confirmation, personal records and source-backed care checks.
- **O2 - Develop:** Implement a mobile-friendly English web prototype with photo suggestions, optional collection saving, editable watering-check and fertilising reminders, care history and cited care Q&A.
- **O3 - Evaluate:** Assess identification, answer correctness, source support, collection isolation, reminder behaviour, latency and operating cost against frozen reference cases and a keyword-search baseline for Q&A.

## Target Users and Supported Plants

The initial audience is beginner indoor-plant owners in Malaysia, including students and people keeping a few plants in rooms or small homes. V1 supports English and three care species: **golden pothos (*Epipremnum aureum*), snake plant (*Dracaena trifasciata*, also *Sansevieria trifasciata*) and spider plant (*Chlorophytum comosum*)**. The app will ask about conditions rather than assume all Malaysian homes are alike.

## How PlantPal Will Work

1. **Identify:** Take/upload a photo, preview it and consent to an identification request. View possible species, not a guaranteed identity.
2. **Confirm and optionally save:** Correct/select a supported species, then choose Add to My Plants. Give it a nickname and optionally save its photo.
3. **Configure care checks:** Supply light/drainage context and approve an editable watering-check interval. Optional fertilising reminders stay off until explicitly configured; no chemical doses are generated.
4. **Track and remember:** View due tasks and care history; record watered, still damp, skipped or fertilised. Snooze/pause/disable reminders. Opt-in email delivery will work without keeping the browser open.
5. **Ask and verify:** Ask about watering, lighting, drainage or basic repotting and inspect the cited evidence.

AI suggests identities and explains retrieved guidance. Ordinary application logic manages collections, ownership, logs and schedules. A photo does not measure moisture, and a date does not prove watering is needed. V1 excludes disease diagnosis, treatment/chemical dosing, sensors, automatic watering and a native mobile release.

## Proposed Data Sources

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

Source links checked 28 October 2026. The two university PDFs are prepared locally for course review; redistribution rights are not established, so the public repo contains links and metadata. Overseas seasonal advice and species conflicts require review before indexing. See [data preparation and parsing notes](data/README.md) and the [full scope](docs/week1_project_scope.md).

Photo identification will use a pre-trained service rather than a newly trained model. [Pl@ntNet](https://docs.plantnet.org/en/reference/api-plantnet/) is a researched candidate; selection, current terms, attribution, credentials and integration remain later work. Only consented or appropriately licensed labelled photos will be used for testing.

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
