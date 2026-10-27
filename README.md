# Scholarship Information Assistant

> A document-grounded assistant that helps prospective postgraduate applicants understand published scholarship requirements and find the official evidence behind each answer.

**Domain:** Higher education / scholarship information  
**Stage:** Week 1 preliminary scope; the application has not been implemented.  
**Submission:** Individual AI Engineering semester project.

## Problem Statement

Prospective postgraduate applicants must interpret scholarship requirements across separate provider guidelines, eligibility pages and application instructions. Missing a condition or mixing information from different award years can lead to an unsuitable or incomplete application. The proposed assistant will retrieve relevant passages and explain them in plain English with citations, while leaving eligibility and award decisions to the scholarship provider.

AI is useful for paraphrased questions and explanations spanning multiple passages. Direct links and keyword search remain a baseline: a single known fact should be retrieved directly, and AI must demonstrate added value through evaluation.

## Evidence of the Problem

- Chevening separates its [eligibility guidance](https://www.chevening.org/resource-hub/guidance/eligibility/) from [work-experience guidance](https://www.chevening.org/scholarships/who-can-apply/work-experience/). Together they illustrate that a question about qualifying experience requires attention to both the general rules and detailed conditions.
- The [MEXT 2027 Research Students guidelines](https://www.studyinjapan.go.jp/en/_mt/2026/04/01-2027_Research_Guidelines_E.pdf) contain multiple qualification and application sections. The [official application page](https://www.studyinjapan.go.jp/en/smap-stopj-applications-research.html) additionally directs applicants to their diplomatic mission for country-specific procedures and schedules.

These sources demonstrate document fragmentation and conditional requirements. They do not establish how many students struggle or how much time is lost; those user-impact claims need validation in Topic 2. Sources checked on 27 October 2026.

## Initial Related Work / Existing Approaches

| Study / solution | Relevant approach | Key limitation for this project |
|---|---|---|
| [Official Chevening guidance](https://www.chevening.org/resource-hub/guidance/eligibility/) | Provider-authored rules and linked guidance | The inspected pages concern one scheme; users must consult other providers separately. This is a project-specific assessment. |
| [Lewis et al. (2020), Retrieval-Augmented Generation for Knowledge-Intensive NLP Tasks](https://arxiv.org/abs/2005.11401) | Combines retrieval with generation for knowledge-intensive tasks | The study does not establish correctness for scholarship cycles, country exceptions or this corpus. Local evaluation is required. |
| [Liu et al. (2023), Lost in the Middle](https://arxiv.org/abs/2307.03172) | Evaluates how models use relevant information in long contexts | Performance in the tested models varies with evidence position. Supplying entire handbooks alone is therefore an insufficient design assumption; the chosen model needs testing. |

## Engineering Gap / Limitation

For a small, curated scholarship corpus, the project will investigate whether retrieval filtered by scheme, award year and country context, followed by citation-grounded explanation, can answer questions more usefully than keyword search while avoiding unsupported conclusions. Official sources are authoritative, but cross-source explanation and version handling remain implementation work. This is a bounded engineering gap, not a claim that no existing scholarship chatbot addresses it.

## Project Objectives

- **O1:** Design and document a curated corpus of three official scholarship documents, recording provider, scheme, version or award year, country scope, source URL and retrieval date.
- **O2:** Develop a retrieval-augmented English Q&A prototype that cites supporting passages, asks for missing scheme/year context, and abstains when evidence is absent or conflicting.
- **O3:** Evaluate answer correctness, citation support, retrieval quality, latency and abstention using a frozen set of 40 manually checked questions and a keyword-search baseline. Proposed metrics and targets are in the [project scope](docs/week1_project_scope.md).

## Target Users

Final-year undergraduates and recent graduates exploring postgraduate scholarships, including UTM students. Users may consult any supported scheme, but inclusion in the corpus does not imply eligibility for any nationality. Student advisers are secondary users checking source passages.

## Data Sources

| Document | Provider / version | Public URL |
|---|---|---|
| Research Students Scholarship Application Guidelines | MEXT / 2027 award cycle; PDF | [Official guidelines](https://www.studyinjapan.go.jp/en/_mt/2026/04/01-2027_Research_Guidelines_E.pdf) |
| Australia Awards Scholarships Policy Handbook | DFAT / June 2026; PDF | [Official handbook](https://www.dfat.gov.au/sites/default/files/aus-awards-scholarships-policy-handbook.pdf) |
| Chevening eligibility criteria | Chevening / live HTML guidance; checked 27 October 2026 | [Official eligibility document](https://www.chevening.org/resource-hub/guidance/eligibility/) |

These are proposed data sources, not an assertion that applications are currently open. Record and pin each document version during implementation. Country-specific questions require appropriate official country guidance; otherwise the assistant must decline to infer an answer. URLs and source metadata are public here. Provider documents are not redistributed in this repository; permitted local processing and any external model use must be checked before ingestion.

## Scope and Responsible AI

The prototype explains published requirements and application-document lists. It will not select candidates, determine personal eligibility, predict success, submit applications, collect passports/transcripts or provide financial, immigration or legal advice. Main risks are invented requirements, outdated or mixed-cycle evidence, uneven coverage and disclosure of applicant data. Planned controls include passage citations, metadata filtering, explicit corpus limits, abstention and synthetic evaluation profiles. See the [full risk analysis](docs/week1_project_scope.md#responsible-ai-risks).

## Comparison with EduAdvisor

Both projects propose document retrieval, cited explanations, guardrails and evaluation. EduAdvisor addresses courses, prerequisites and graduation requirements. This assistant addresses scholarship publications across providers and must preserve award-year and country context; it does not issue eligibility decisions.

## Repository Structure

```text
ai-engineering-scholarship-assistant/
├── README.md
├── docs/
│   ├── week1_observation_sheet.md
│   ├── week1_system_map.png
│   └── week1_project_scope.md
├── data/
├── src/
├── notebooks/
├── evaluation/
└── prompts/
```

The five development folders contain `.gitkeep` files until implementation begins. The group observation sheet and ChatGPT conceptual system map are Part A/B learning evidence, not the architecture of the proposed scholarship assistant.

## Weekly Progress

| Week | Focus | Deliverable | Status |
|---|---|---|---|
| W1 | Initial framing and engineering record | README, project scope, group evidence copies and folder structure | Part C documents prepared. Group observation sheet still has missing names, unverified test outcomes/evidence and an outstanding group-sharing observation. Moodle submission pending. |
| W2 | Refine requirements and evaluation | Validate user need, confirm document permissions/versions and refine acceptance criteria | Planned |

## Week 1 Evidence Status

The supplied Group 2 files are copied under the required names. Their existing limitations are preserved: file presence does not establish that the group observations are complete. Complete the outstanding Part A evidence before treating the whole Week 1 submission as finished. The system map marks inferred components and hypothetical failures.

GitHub will progressively store requirements, architecture, source metadata, code, prompts, evaluation evidence and later deployment/monitoring documentation. Never commit credentials, private applicant information or documents that cannot be redistributed.
