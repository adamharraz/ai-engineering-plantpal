# Week 3 Pipeline Decisions — PlantPal

## Requirements, sources and architecture

FR8 needs care passages from S01–S03; FR1 needs inspectable photos from S04–S06; FR11 needs source identity, versions and provenance for all six sources. The [inventory](week3_data_inventory.md) records their URLs and rights. Pathways A and C expand Week 2 C3 care documents and the C1/C2 image input. The [pipeline diagram](week3_pipeline_diagram.png) shows only those data branches.

## Part B comparison and selected preparation

| Approach / Configuration A | Approach / Configuration B | Observed Evidence | Selected Approach and Trade-off |
|---|---|---|---|
| Fixed chunks of 500 characters after whitespace cleanup. | Paragraph-aware chunks with a maximum of 500 characters after the same cleanup. | Money plant: 4 fixed / 5 paragraph chunks. Snake plant: 4 / 6. Spider plant: 4 / 5. Fixed first chunks ended inside words for all three sources; paragraph first chunks ended at paragraph boundaries. The snake guide contains a paragraph longer than 500 characters, so the paragraph method still uses a fixed split there. | **B**, supporting FR8. It keeps more paragraph boundaries and produced 16 records, compared with 12 fixed records. Accepted limitation: more, uneven chunks; long paragraphs can still split, and a heading may be separated from its detail. This is not semantic chunking. |
| Convert to RGB and resize each image to 224 × 224. | Convert to RGB, keep original dimensions (`resize=None`) and discard copied metadata. | All originals were RGB. Two were 1536 × 2048 and one was 1152 × 2048. A changed their width/height ratios from 0.75 or 0.5625 to 1.0. The displayed comparison showed the square resize widening the plant. B retained the original dimensions; reloaded PNGs had no EXIF or other image metadata. | **B**, supporting FR1. Preserve leaf shape and detail until the identification service's input requirements are known. Accepted limitation: larger files—5,461,083, 3,622,547 and 7,411,359 bytes. These are internal preparation outputs; all three source JPEG uploads were below FR1's 5 MB limit. |

Whitespace cleanup used the lab's `[ \t]+` replacement and trimming. Character counts remained 1,780, 1,867 and 1,549. The visible-page snapshots already used normal spacing; this step did not remove navigation or shopping content.

Parts B and C use the same settings and preparation functions:

| Pathway | Selected configuration | Output |
|---|---|---|
| A — documents | `chunk_method=paragraph`, `chunk_size=500`; UTF-8 TXT read, whitespace cleanup; `page=None`. | One `prepared_text.jsonl` containing 16 records linked to their original snapshot names and hashes. |
| C — images | `convert_rgb=True`, `resize=None`, `strip_metadata=True`. | Three PNGs; Part B also exports `image_manifest.csv` with dimensions, source labels, owners, URLs and processing notes. |

## Workflow decisions

**ETL:** extract the selected TXT/image files, transform them, then load the prepared outputs into separate storage. Raw snapshots remain unchanged. This is appropriate because this small lab produces prepared artefacts rather than loading raw data into an analytical warehouse.

**Batch:** process a fixed snapshot of three care guides and three photos. These sources do not need live streaming for this lab. A later source update requires a new reviewed snapshot and rerun; no automated source monitoring is implemented.

**Storage and provenance:** Part B uses `data/processed/` and `data/week3_manifest.json`. Part C uses `data/pipeline_v1/documents/` and `data/pipeline_v1/images/`, each with `pipeline_manifest.json`. Three document sources map to one text export. Each photo maps to its corresponding PNG, and Part B additionally links each photo to the image metadata CSV.

The public repository includes the licensed PNGs, metadata and manifests. Full nursery text and both derived JSONL files remain in the private locations recorded in the [inventory](week3_data_inventory.md), as allowed by the lab's restricted-data exception. Hashes identify the captured version; they do not establish permission, correctness or freshness. The English page snapshots omit collapsed answers. Advice needs independent review before C3 can supply it to users.

## Stage contracts and failure responses

| Stage | Contract / evidence | Failure response |
|---|---|---|
| Ingest | Explicit source paths exist; rights are recorded; uploaded bytes match the selected source hashes. Cell 3 listed seven files, including the metadata sidecar. | Stop for missing files or a fingerprint mismatch; check the upload and selected version. Keep material with unconfirmed redistribution rights private. |
| Inspect | All three TXT snapshots have readable text and expected plant names. All three JPEGs pass Pillow verification; dimensions, mode and sample previews are inspected. | Stop for empty/unreadable text or corrupt images. Check conversion or replace the input; flag incomplete page capture for review. |
| Prepare | Selected paragraph settings produce text records no longer than 500 characters. Selected image settings produce three RGB PNGs with unchanged dimensions and no copied metadata. | Stop or report a transformation failure; inspect the source and selected settings before retrying. |
| Validate | Reloaded outputs are non-empty; text IDs are unique; counts, modes, dimensions and metadata checks pass. Output SHA-256 hashes and byte sizes are recorded. | Stop for missing, empty or structurally invalid outputs. Do not treat file creation as proof of data quality. |
| Record lineage | Nine Part B source-to-output links validate. Each Part C branch records source hashes, settings, output hashes, sizes and three source-to-output mappings. | Stop/report missing mappings or metadata; correct registration before saving the manifest. |

## Execution and repeatability evidence

The notebook ran in a Google Colab Python 3 CPU runtime, across midnight on 17–18 November 2026 (Malaysia time). Cell 6B reported **COMMON LINEAGE VALIDATION: PASS**, verifying nine links. Cell 8A reported **3 inputs / 1 output**; Cell 8C reported **3 inputs / 3 outputs**. Both Part C exports matched the corresponding Part B output hashes.

Cell 9 reran both registered pathways and reported **PASS** for documents and images. Source fingerprints, prepared-output hashes, byte sizes, output counts and mappings matched. Timestamps can differ. These are repeatability smoke checks, not proof of care safety, identification accuracy or reproducibility across different environments.

The recorded outputs and execution counts are retained in [week3_pipeline.ipynb](../notebooks/week3_pipeline.ipynb). After browser downloads failed, its visible Colab text outputs were copied through the UI; image previews were omitted from the retained notebook view. Prepared data were also recovered through visible output views, and every saved file was checked against its Colab manifest hash. The temporary recovery cells were removed.

## Engineering decision: GO WITH CONDITIONS

Internal prototype work can proceed because the prepared data passed structural and repeatability checks. Before enabling public care answers or making identification accuracy claims, complete these checks:

1. Remove repeated menu and shopping text, recover the missing answers from collapsed sections, cross-check the care advice against university extension guides, and confirm reuse rights before approving the passages for public use.
2. Evaluate the identification API with at least 10 independently labelled photos per supported species. Include varied lighting and angles, plus a separate set of unclear or unsupported plant photos, as specified in [EC01](week2_evaluation_criteria.md).

## Traceability

| Topic 2 requirement | Source / branch | Week 3 evidence | Later check |
|---|---|---|---|
| FR8 | S01–S03 / documents | 16 records, source identities, [document run manifest](../data/pipeline_v1/documents/pipeline_manifest.json). | EC04: assess care-answer correctness and citations after source review. |
| FR1 | S04–S06 / images | Three verified RGB PNGs, preserved dimensions, [image manifest](../data/processed/image_manifest.csv). | EC01: evaluate identification on a larger, independently checked photo set. |
| FR11 | All six sources | [Common lineage](../data/week3_manifest.json), both run manifests and Cell 9 repeatability results. | EC07: review source changes and test updates. |
