# Malaysian Scholarship Information Assistant

> A document-grounded assistant that helps Malaysian students understand undergraduate scholarships for study in Malaysia, with answers supported by official provider documents.

**Domain:** Malaysian higher education / undergraduate scholarship information  
**Stage:** Week 2 requirements, evaluation plan and conceptual architecture; the application has not been implemented.  
**Submission:** Individual AI Engineering semester project.

## Problem Statement

Malaysian pre-university students and undergraduates must interpret scholarship requirements across different foundations' websites, programme pages and FAQs. Mixing study levels, intake years or local and overseas pathways can produce misleading answers. The proposed assistant will retrieve official passages and explain academic criteria, approved institutions, coverage and application-document requirements for a small set of Malaysian scholarships.

AI is useful for paraphrased questions and explanations spanning multiple passages. Direct links and keyword search remain a baseline. Scholarship providers retain responsibility for eligibility and award decisions.

## Evidence of the Problem

- [Khazanah Watan guidance](https://www.yayasankhazanah.com.my/scholarship-programmes/khazanah-watan-scholarship-programme) separates requirements by study level and lists approved universities and subject areas. Answering a question requires retrieving the correct undergraduate section, not an unrelated postgraduate condition.
- YTAR publishes its [scholarship overview and requirements](https://www.yayasantar.org.my/ytar-tar-scholarship) separately from its [application FAQ](https://www.yayasantar.org.my/ytar-scholarship-faq). Applicants may need both documents to understand the programme and supporting-document process.

These official sources show distributed, conditional information. User confusion and time savings remain hypotheses to validate in Topic 2. Sources checked on 27 October 2026.

## Initial Related Work / Existing Approaches

| Study / solution | Relevant approach | Key limitation for this project |
|---|---|---|
| [Official Khazanah Watan guidance](https://www.yayasankhazanah.com.my/scholarship-programmes/khazanah-watan-scholarship-programme) | Provider-authored rules and linked guidance | The inspected guidance concerns one provider; applicants must consult other Malaysian providers separately. This is a project-specific assessment. |
| [Lewis et al. (2020), Retrieval-Augmented Generation for Knowledge-Intensive NLP Tasks](https://arxiv.org/abs/2005.11401) | Combines retrieval with generation for knowledge-intensive tasks | The study does not establish correctness for Malaysian scholarship intakes, local study pathways or this corpus. Local evaluation is required. |
| [Liu et al. (2023), Lost in the Middle](https://arxiv.org/abs/2307.03172) | Evaluates how models use relevant information in long contexts | Performance in the tested models varies with evidence position. Supplying entire handbooks alone is therefore an insufficient design assumption; the chosen model needs testing. |

## Engineering Gap / Limitation

For a small, curated scholarship corpus, the project will investigate whether retrieval filtered by Malaysian provider, intake/year, study level and local study pathway, followed by citation-grounded explanation, can answer questions more usefully than keyword search while avoiding unsupported conclusions. Official sources are authoritative, but cross-source explanation and version handling remain implementation work. This is a bounded engineering gap, not a claim that no existing scholarship chatbot addresses it.

## Project Objectives

- **O1:** Design and document a curated corpus of three official Malaysian scholarship documents, recording provider, scheme, intake/year, study level, local study pathway, source URL and retrieval date.
- **O2:** Develop a retrieval-augmented English Q&A prototype that cites supporting passages, asks for missing scheme/intake/study-level context, and abstains when evidence is absent or conflicting.
- **O3:** Evaluate answer correctness, citation support, retrieval quality, latency and abstention using a frozen set of 40 manually checked questions and a keyword-search baseline. Proposed metrics and targets are in the [project scope](docs/week1_project_scope.md).

## Target Users

Malaysian pre-university students preparing for a local bachelor's degree and current undergraduates, including UTM students, who want to understand published scholarship requirements. Parents and student advisers are secondary users checking official information. Each scheme has its own restrictions; the assistant must not imply that all students or institutions qualify.

## Data Sources

| Document | Provider / scope | Public URL |
|---|---|---|
| Khazanah Watan Scholarship Programme guidance | Yayasan Khazanah; undergraduate sections for Malaysian universities | [Official guidance](https://www.yayasankhazanah.com.my/scholarship-programmes/khazanah-watan-scholarship-programme) |
| Tunku Abdul Rahman Scholarship overview and requirements | Yayasan Tunku Abdul Rahman (YTAR); undergraduate study in Malaysia | [Official guidance](https://www.yayasantar.org.my/ytar-tar-scholarship) |
| Biasiswa PRIME Scholarship requirements | Yayasan Tenaga Nasional (YTN); local university pathways only | [Official guidance](https://ytn.tnb.com.my/biasiswa-prime-scholarship/) |

These are three publicly accessible HTML documents, checked on 27 October 2026. The [YTAR application FAQ](https://www.yayasantar.org.my/ytar-scholarship-faq) is a supporting fourth document. Pin permitted snapshots and record section, scheme, study level, study location, intake/year and retrieval date before ingestion. The YTN page contains several pathways and legacy material; only clearly identified local-pathway sections may be indexed, and unclear conditions require provider confirmation. Inclusion does not imply that applications are currently open or that every student is eligible. Source text is not redistributed here.

## Scope and Responsible AI

The prototype explains published requirements and application-document lists for Khazanah Watan, the Tunku Abdul Rahman Scholarship and local YTN PRIME pathways. Initial Q&A will be in English; Bahasa Melayu support is a possible later extension. It will not select candidates, determine personal eligibility, predict success, submit applications, collect passports/transcripts or provide financial, immigration or legal advice. Main risks are invented requirements, outdated or mixed-cycle evidence, uneven coverage and disclosure of applicant data. Planned controls include passage citations, metadata filtering, explicit corpus limits, abstention and synthetic evaluation profiles. See the [project scope](docs/week1_project_scope.md#responsible-ai-risks).

## Comparison with EduAdvisor

Both projects propose document retrieval, cited explanations, guardrails and evaluation. EduAdvisor addresses courses, prerequisites and graduation requirements. This assistant addresses Malaysian scholarship publications across providers and must preserve intake/year, study level and local study pathway; it does not issue eligibility decisions.

## Repository Structure

```text
ai-engineering-scholarship-assistant/
├── README.md
├── docs/
│   ├── week1_observation_sheet.md
│   ├── week1_system_map.png
│   ├── week1_project_scope.md
│   ├── week2_requirements.md
│   ├── week2_evaluation_criteria.md
│   ├── week2_architecture_v1.png
│   ├── week2_architecture_v1.drawio
│   └── week2_architecture_decisions.md
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
| W1 | Initial framing and engineering record | README, project scope, group evidence copies and folder structure | Part C documents published; scope refined to Malaysian undergraduate scholarships. Group observation sheet still has missing names, unverified test outcomes/evidence and an outstanding group-sharing observation. Moodle submission pending. |
| W2 | Requirements, evaluation criteria and V1 architecture | Four required documents, editable diagram, design trade-offs, traceability and reflections | Documents prepared; implementation and evaluation runs remain future work. Moodle submission is handled by the student. |

## Week 2 Deliverables

- [Requirements specification](docs/week2_requirements.md): Sections 0-3, six functional requirements, five non-functional requirements and five Responsible AI requirements.
- [Evaluation criteria](docs/week2_evaluation_criteria.md): eleven criteria with measurement methods, metrics, acceptance thresholds and expected outcomes.
- [V1 architecture diagram](docs/week2_architecture_v1.png), with an [editable Draw.io source](docs/week2_architecture_v1.drawio).
- [Architecture decisions and traceability](docs/week2_architecture_decisions.md): four decisions, alternatives, trade-offs and an objective-to-evidence matrix.

The Week 2 documents refine Week 1's preliminary acceptance targets and are the current specification. Reflections are included in the three Markdown deliverables. Targets describe planned validation; no prototype results or user-study findings are claimed.

## Week 1 Evidence Status

The supplied Group 2 files are copied under the required names. Their existing limitations are preserved: file presence does not establish that the group observations are complete. Complete the outstanding Part A evidence before treating the whole Week 1 submission as finished. The system map marks inferred components and hypothetical failures.

GitHub will progressively store requirements, architecture, source metadata, code, prompts, evaluation evidence and later deployment/monitoring documentation. Never commit credentials, private applicant information or documents that cannot be redistributed.
