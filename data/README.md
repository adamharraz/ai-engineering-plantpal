# PlantPal Source Preparation

The public repository records source URLs and metadata in [sources.csv](sources.csv). It does not redistribute the selected university PDFs or website photographs because redistribution permission has not been established. Download permitted course-use copies into an ignored local folder such as data/local/; do not force-add that folder to Git.

## Two PDF documents for lab preparation

| ID | Document | Public URL | Initial parsing observation |
|---|---|---|---|
| UT-A | Selecting Indoor Plants for the Home or Office, W 1128-A | [University of Tennessee PDF](https://uthort.tennessee.edu/wp-content/uploads/sites/228/2023/11/W1128A.pdf) | Ten pages; species tables span pages and use colour to indicate categories. Repeated column headings, wrapped botanical names, footcandle units and coloured backgrounds need preservation or explicit reconstruction. |
| UT-B | Caring for Indoor Plants, W 1128-B | [University of Tennessee PDF](https://uthort.tennessee.edu/wp-content/uploads/sites/228/2023/11/W1128B.pdf) | Seven pages; numbered watering subsections, photo captions, page headers and a media-mix table can disrupt chunk boundaries. Keep the species and condition attached to each care statement. |

The two documents were downloaded for local preparation on 28 October 2026 and their extracted text reviewed. A rendered species-table page and watering page were inspected for layout. Preparation is not an approved ingestion release or a redistribution licence.

## Review before ingestion

1. Check each source's rights and access terms. Where uploading PDFs is not permitted, keep local course copies private and retain only URLs/metadata publicly. The Week 3 preparation request to place PDFs in data/ remains subject to this restriction.
2. Assign passage IDs containing document ID, page/section, species and topic. Exclude navigation text, captions that lack context and unrelated species.
3. Preserve canonical botanical names and aliases. The Illinois pothos page also discusses silver pothos; do not merge it into golden pothos. Map Sansevieria trifasciata to Dracaena trifasciata deliberately.
4. Review climate and growth assumptions. US winter/month-based advice and window-aspect rules must not become default Malaysian schedules.
5. Resolve source wording differences before activation. For example, the Tennessee spider-plant table refers to relatively moist media during active growth, while the Illinois page uses broader drying wording. Preserve the growth context; do not create a universal interval or blend the statements silently.
6. Exclude disease treatment, pesticides and chemical preparation/dose instructions even when a source contains them. Only reviewed in-scope passages become searchable.
7. Record retrieval date, rights status, file hash where a permitted snapshot exists, section/page, review outcome and release ID. Current sources.csv records selection metadata, not completed rights approval or a full ingestion audit.

## Images, private data and external services

Use self-taken labelled plant images with consent or appropriately licensed images with attribution records for evaluation. Do not download website photos merely because they are visible. User uploads are private application data, never GitHub fixtures; remove EXIF metadata before identification and implement the retention targets in the requirements.

[Pl@ntNet API documentation](https://docs.plantnet.org/en/reference/api-plantnet/) and [access terms](https://my.plantnet.org/terms_of_use) are references for the later provider review. Recheck access, attribution, retention and quotas at integration time. No API key, account setup, paid plan, notification subscription or actual user-image transmission is part of Week 1/2 planning.
