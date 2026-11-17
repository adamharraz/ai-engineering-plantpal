# Week 3 Data Inventory — PlantPal

## Requirements and data needs

| Requirement ID | Needed information / feature | Candidate source | Why usable / constraint |
|---|---|---|---|
| FR8 | Source-supported answers about the three supported plants. | S01–S03: Malaysian nursery care guides. | Relevant English care information; advice and permission for public redistribution still need review. |
| FR1 | Readable plant photos for the identification input. | S04–S06: CC0 iNaturalist photos. | One source-labelled photo per supported species; suitable for a pipeline sample, not an accuracy evaluation. |
| FR11 | Source identity, versions and review records. | All six sources and their metadata. | URLs, snapshots, owners, rights and SHA-256 hashes support later review and updates. |

The selected pathways are **A — documents** and **C — images**. These expand C3 (care documents) and the C1/C2 image input in the [Week 2 architecture](week2_architecture_v1.png). EC01, EC04 and EC07 describe later identification, care-answer and source-update checks; this lab does not establish those results.

## Source inventory

| Source ID | Filename / URL | Type and purpose | Version / date / owner | Access / rights | Inspection evidence |
|---|---|---|---|---|---|
| S01 | `documents/money_plant.txt`; [Money Plant Care Guide](https://wengthye.com/careguide/money-plant) | Web page converted to UTF-8 TXT; golden pothos care. | Visible English page captured 17 November 2026; no publication date shown; 54 Weng Thye Nursery. | Public reading; redistribution licence unconfirmed. Full text and derived text kept privately. | Readable plant name and care headings; 1,789 bytes / 1,780 characters; 5 selected chunks. |
| S02 | `documents/snake_plant.txt`; [Snake Plant Care Guide](https://wengthye.com/careguide/snake-plant) | Web page converted to UTF-8 TXT; snake plant care. | Visible English page captured 17 November 2026; no publication date shown; 54 Weng Thye Nursery. | Public reading; redistribution licence unconfirmed. Full text and derived text kept privately. | Readable botanical name including the Sansevieria synonym; 1,876 bytes / 1,867 characters; 6 selected chunks. |
| S03 | `documents/spider_plant.txt`; [Spider Plant Care Guide](https://wengthye.com/careguide/spider-plant) | Web page converted to UTF-8 TXT; spider plant care. | Visible English page captured 17 November 2026; no publication date shown; 54 Weng Thye Nursery. | Public reading; redistribution licence unconfirmed. Full text and derived text kept privately. | Readable plant name and care headings; 1,556 bytes / 1,549 characters; 5 selected chunks. |
| S04 | `images/money_plant.jpg`; [iNaturalist observation 410724008](https://www.inaturalist.org/observations/410724008), photo 755444484 | JPEG; source label *Epipremnum aureum*. | Observed 16 May 2026; API snapshot 17 November 2026; `leaf0605`. | [CC0 1.0](https://creativecommons.org/publicdomain/zero/1.0/), confirmed in the photo's API record. Prepared photo may be shared. | Pillow verification and preview succeeded; RGB, 1536 × 2048; 854,980 bytes; 0 EXIF entries. |
| S05 | `images/snake_plant.jpg`; [iNaturalist observation 351175835](https://www.inaturalist.org/observations/351175835), photo 640813687 | JPEG; source label *Sansevieria trifasciata*, recorded under PlantPal's *Dracaena trifasciata*. | Observed 19 April 2026; API snapshot 17 November 2026; `pedrotterres`. | [CC0 1.0](https://creativecommons.org/publicdomain/zero/1.0/), confirmed in the photo's API record. Prepared photo may be shared. | Pillow verification and preview succeeded; RGB, 1152 × 2048; 1,768,682 bytes; 0 EXIF entries. |
| S06 | `images/spider_plant.jpg`; [iNaturalist observation 414058600](https://www.inaturalist.org/observations/414058600), photo 762175482 | JPEG; source label *Chlorophytum comosum*. | Observed 10 November 2026; API snapshot 17 November 2026; `helenlgc`. | [CC0 1.0](https://creativecommons.org/publicdomain/zero/1.0/), confirmed in the photo's API record. Prepared photo may be shared. | Pillow verification and preview succeeded; RGB, 1536 × 2048; 2,075,609 bytes; 0 EXIF entries. |

## Ingestion and storage

The three nursery pages opened in the browser. Their visible English text was saved as TXT before Colab upload because the lab's document loader accepts TXT/PDF rather than HTML. Collapsed answers were not expanded. The snapshots include navigation, shopping text and care advice, so they are prepared candidates awaiting quality review, not an approved care collection.

The photos were obtained from the public iNaturalist API, filtering for research-grade observations with CC0 photos. The original-size variant follows the [official API documentation](https://github.com/inaturalist/iNaturalistAPI/blob/main/lib/views/swagger_v1.yml.ejs). Source labels have not been independently checked. JPEGs were checked for EXIF before upload; no EXIF segments were present, so their bytes were unchanged. No coordinates from the observation records were copied into the submission.

In Google Colab, Cell 1 created `/content/week3_project`. Six source files and `source_metadata.json` were uploaded through the Files panel into `data/raw`, then organised into `documents/` and `images/`. Cell 3 listed all seven files; A3 inspected all six sources, including text samples and image previews. The input listing, comparisons and validation results are retained in [the notebook](../notebooks/week3_pipeline.ipynb).

| Storage | Location |
|---|---|
| Temporary Colab inputs | `/content/week3_project/data/raw/` |
| Part B outputs | `/content/week3_project/data/processed/` |
| Part B lineage | [data/week3_manifest.json](../data/week3_manifest.json) |
| Part C outputs and run manifests | [documents](../data/pipeline_v1/documents/pipeline_manifest.json) and [images](../data/pipeline_v1/images/pipeline_manifest.json) |
| Public prepared images and labels | [Part B images](../data/processed/images/), [image manifest](../data/processed/image_manifest.csv) and [Part C images](../data/pipeline_v1/images/) |
| Persistent private source copies | Local project workspace: `work/topic3_inputs/upload/`, outside the GitHub repository. |
| Persistent private full text and exports | Local project workspace: `work/topic3_private_outputs/data/`, outside the GitHub repository. Full workspace root: `C:\Users\Adam Harraz\Documents\Codex\2026-10-27\instructions-this-assignment-covers-part-c`. |

The two full `prepared_text.jsonl` exports stay in private local storage because nursery redistribution permission is unconfirmed. Under the lab's restricted-data exception, the public notebook, counts, hashes and manifests provide non-sensitive execution evidence. `/content` is temporary; the saved local copies are used for later sessions. Rerunning the notebook requires the six private source snapshots and their metadata file.
