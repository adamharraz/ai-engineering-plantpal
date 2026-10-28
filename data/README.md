# PlantPal Data Sources

PlantPal has **three main care documents**, all published by 54 Weng Thye Nursery in Sungai Buloh, Selangor. The two university PDFs are **supporting references** for checking advice and preparing for the lab.

## Main care documents

| ID | Public document | Supported plant |
|---|---|---|
| WT-P | [Money Plant Care Guide](https://wengthye.com/careguide/money-plant) | Golden pothos / money plant (*Epipremnum aureum*) |
| WT-S | [Snake Plant Care Guide](https://wengthye.com/careguide/snake-plant) | Snake plant (*Dracaena trifasciata*, also *Sansevieria trifasciata*) |
| WT-C | [Spider Plant Care Guide](https://wengthye.com/careguide/spider-plant) | Spider plant (*Chlorophytum comosum*) |

[sources.csv](sources.csv) contains these three records. They are selected sources, not an already approved or indexed knowledge base. The guide pages were located through web search on 28 October 2026; independent checking, rights review and passage approval remain to be completed before ingestion.

## Two supporting PDFs for lab preparation

| ID | Supporting document | Purpose / parsing note |
|---|---|---|
| UT-A | [Selecting Indoor Plants for the Home or Office, W 1128-A](https://uthort.tennessee.edu/wp-content/uploads/sites/228/2023/11/W1128A.pdf) | Independent background reference. Ten pages; species tables span pages and use colour, wrapped names and repeated headings. |
| UT-B | [Caring for Indoor Plants, W 1128-B](https://uthort.tennessee.edu/wp-content/uploads/sites/228/2023/11/W1128B.pdf) | Independent care reference. Seven pages; numbered sections, captions, page headers and a media-mix table need careful parsing. |

[supporting_references.csv](supporting_references.csv) records their URLs, page counts and local file hashes separately. Both were downloaded locally on 28 October 2026 and reviewed for initial parsing issues. They satisfy the local PDF preparation work but are not automatically included in the app's searchable care documents. US seasonal advice must not become a default Malaysian schedule.

## Review before using a source

1. Check the licence and access terms. Public access does not automatically allow copying the full text or images into GitHub. Keep permitted local copies in an ignored folder such as data/local/.
2. Check each nursery statement against an independent horticultural reference before approving it. Commercial care guides are useful starting points, not proof that every statement suits every home.
3. Keep species, care topic, growing conditions, source heading and passage ID together. Map the snake-plant synonym carefully and do not confuse golden pothos with other plants called money plant.
4. Exclude shopping content, disease-treatment instructions, chemical doses and claims outside the project scope. Do not turn a symptom into a confirmed diagnosis.
5. Treat watering and feeding intervals as starting points. The app asks the owner to check conditions and approve reminders; a photo does not measure soil moisture or nutrient needs.
6. Keep unsupported questions separate from supported answers. Basic repotting can be asked about, but without a supporting approved passage the app must explain that it lacks enough information. The supporting PDFs may be linked for further reading without silently becoming part of the AI's searchable collection.
7. Record the URL, species, heading, retrieval date, rights status, review outcome and source-release version for every approved passage. Give the AI and keyword baseline the same release.

## Images and external services

Use self-taken labelled images with consent or images with an appropriate licence for testing. Do not copy nursery photos just because they are visible online. Private user photos, email addresses and care records do not belong in the public repository.

[Pl@ntNet](https://docs.plantnet.org/en/reference/api-plantnet/) is a possible identification service. Its [access terms](https://my.plantnet.org/terms_of_use), attribution, costs and privacy practices must be checked before integration. It is separate from the three care documents. No identification or email service has been activated by this documentation update.
