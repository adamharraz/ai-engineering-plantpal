# Week 1 Project Scope

**Submission type:** Individual semester project  
**Status:** Preliminary; to be refined in Topic 2  
**Prepared:** 27 October 2026  
**Repository name:** `ai-engineering-scholarship-assistant`

## Project Name

Scholarship Information Assistant

## Domain

Higher education / scholarship information access, initially focused on published postgraduate scholarship requirements.

## Problem Statement

Final-year undergraduates and recent graduates exploring postgraduate funding need to interpret provider-specific requirements, application-document lists and award-cycle instructions. Information can be distributed across separate guidelines and linked pages. Users may overlook exceptions or combine conditions from different schemes or years.

The project proposes a small, document-grounded Q&A assistant that retrieves relevant official passages and explains them in plain English. Semantic retrieval can match everyday wording to formal policy language, while generation can connect several passages into a readable explanation. Citations let users inspect the evidence. A search-only baseline will test whether this added complexity is justified.

The semester prototype will cover three named schemes/documents rather than all scholarships. It explains published information; scholarship providers retain responsibility for personal eligibility and award decisions.

## Evidence of the Problem

### Evidence 1: Conditions are distributed across linked guidance

Chevening publishes general eligibility rules and a separate document explaining qualifying work experience. This provides observable evidence that applicants may need multiple pages to interpret one requirement.

**Sources / citations:** [Chevening eligibility criteria](https://www.chevening.org/resource-hub/guidance/eligibility/) and [Chevening work-experience guidance](https://www.chevening.org/scholarships/who-can-apply/work-experience/), accessed 27 October 2026.

### Evidence 2: General guidelines do not replace local application instructions

MEXT provides a detailed 2027 Research Students guideline, while its application page explains that procedures and schedules vary by nationality and directs applicants to the relevant Japanese diplomatic mission. This supports the need to preserve scope and country context when answering questions.

**Sources / citations:** [MEXT 2027 Research Students guidelines](https://www.studyinjapan.go.jp/en/_mt/2026/04/01-2027_Research_Guidelines_E.pdf) and [MEXT official application page](https://www.studyinjapan.go.jp/en/smap-stopj-applications-research.html), accessed 27 October 2026.

**Evidence boundary:** These are direct observations about the information structure. User confusion, adviser workload and time savings remain hypotheses, not measured findings. Topic 2 should validate the need through a small consent-based usability exercise or interviews without collecting sensitive applicant records.

## Initial Related Work (at least 3)

### 1. Official scholarship guidance and linked FAQs

**Study / solution:** Chevening's official eligibility guidance and linked supporting information.

**Source / citation:** [Chevening eligibility criteria](https://www.chevening.org/resource-hub/guidance/eligibility/), accessed 27 October 2026.

**Relevance:** Authoritative provider text should remain the source of truth and a direct-navigation baseline.

**Key limitation:** The inspected pages address Chevening rather than synthesising rules across different providers. A user still needs to locate and interpret other schemes' publications separately. This limitation is an assessment of the inspected pages, not a claim about every service Chevening offers.

### 2. Retrieval-Augmented Generation for Knowledge-Intensive NLP Tasks

**Study / solution:** Lewis et al. (2020), NeurIPS.

**Source / citation:** [Paper](https://arxiv.org/abs/2005.11401).

**Relevance:** Combines retrieved external evidence with language generation; motivates the prototype's document-grounded approach.

**Key limitation:** Its benchmark results do not validate this scholarship corpus or establish reliable handling of award years, country exceptions or eligibility explanations. These controls and tests must be implemented locally. This is a limitation of applying the study to this project, not a finding that the paper tested scholarships.

### 3. Lost in the Middle: How Language Models Use Long Contexts

**Study / solution:** Liu et al. (2023 preprint), accepted for TACL publication.

**Source / citation:** [Paper](https://arxiv.org/abs/2307.03172).

**Relevance:** Reports that relevant information's position can affect performance in tested long-context tasks. This motivates selective retrieval and careful evidence placement instead of assuming that a whole handbook will be used reliably.

**Key limitation:** It analyses particular models and tasks rather than delivering a scholarship assistant. Its results do not establish the performance of the future chosen model; this project needs its own retrieval and answer tests.

## Engineering Gap / Limitation

The practical challenge is to build and evaluate a small scholarship Q&A workflow that preserves provider, award year, document version and country context while giving explanations supported by specific passages. Official publications provide authority, but users must still navigate and relate their contents. General RAG research supplies an approach, not a complete solution for this context.

The project will test whether metadata-aware retrieval and cited explanations improve answer usefulness over keyword search without inventing conditions. It will not claim a novel retrieval algorithm or that no existing scholarship service solves similar problems.

## Initial Project Objectives (2–3)

- **O1: To design** a curated corpus of three official documents, with a source register containing title, provider, scheme, award year/version, country scope, URL, access date and permitted-use notes.
- **O2: To develop** an English retrieval-augmented Q&A prototype with passage citations, scheme/year clarification, context filters and abstention for missing or conflicting evidence.
- **O3: To evaluate** correctness, citation support, retrieval recall, latency and appropriate abstention against a frozen set of 40 manually checked questions and a keyword-search baseline.

### Initial evaluation plan

Build 30 answerable questions balanced across the three schemes and 10 questions whose answers are absent, conflicting or outside scope. Keep a separate development set. Annotate reference answers and supporting passages before evaluation, and keep scheme/year context fixed for a fair baseline comparison. Use synthetic scenarios only.

| Dimension | Proposed measurement | Preliminary target |
|---|---|---|
| Answer correctness | Fully correct answers / 30 answerable questions, using a written rubric | At least 80% |
| Citation support | Supported factual claims / all factual claims in generated answers, checked against cited passages | At least 90% |
| Retrieval quality | Answerable questions with a reference passage in the top 5 results / 30 | At least 85% |
| Abstention | Appropriate abstentions / 10 unanswerable or out-of-scope questions | At least 90% |
| Latency | Median submission-to-complete-response time on the recorded test setup | At most 10 seconds |

Record answer coverage alongside abstention so that refusing every question cannot count as success. Compare retrieval recall and time to locate evidence against keyword search; assess generated answer quality separately. Report denominators, model configuration and failures rather than implying these preliminary targets are achieved. Refine targets after an initial feasibility test in Topic 2.

## Target Users

**Primary users:** Final-year undergraduates and recent graduates researching postgraduate scholarships, including UTM students.

**Secondary users:** Student advisers who want to locate and verify published requirement passages.

The corpus can contain international schemes even when a particular user's nationality is not eligible. The interface must explain coverage and must not imply universal access to every scholarship.

## Data Sources (at least 2)

Three publicly accessible source documents have been identified. Two are PDFs and one is an official HTML guidance document. Access was checked on 27 October 2026.

| Source | Document and public URL | Intended use and context |
|---|---|---|
| 1 | [MEXT: 2027 Research Students Scholarship Application Guidelines](https://www.studyinjapan.go.jp/en/_mt/2026/04/01-2027_Research_Guidelines_E.pdf) | Qualification conditions and application-document guidance. Preserve the 2027 cycle; local schedules require additional country evidence. |
| 2 | [DFAT: Australia Awards Scholarships Policy Handbook](https://www.dfat.gov.au/sites/default/files/aus-awards-scholarships-policy-handbook.pdf) | Published general scholarship policy. The inspected PDF is dated June 2026; country eligibility needs country-specific documentation. |
| 3 | [Chevening: Eligibility criteria](https://www.chevening.org/resource-hub/guidance/eligibility/) | Published general eligibility conditions. Live HTML: save the retrieval date and pin a permitted snapshot at implementation time. |

**Source maintenance:** Before ingestion, check permitted local processing, quotation and any external model transmission. Public accessibility does not itself grant redistribution rights. Store URLs and metadata in GitHub; keep restricted source copies out of version control. The DFAT handbook explicitly reserves reproduction rights. The project does not require publishing provider PDFs.

**Version policy:** Record document hashes for permitted local snapshots. Separate different award cycles rather than overwriting metadata silently. Review sources before each corpus release. An archived guideline is not proof that an application is currently open. If no matching country/year source is indexed, explain the limitation and link to the official provider instead of guessing.

## Similar to EduAdvisor

Both systems serve students by retrieving official educational information and generating cited explanations. Both require document cleaning, chunking, retrieval, a language model, an interface, scope guardrails, evaluation and a feedback process. Accuracy, groundedness and latency are relevant to both.

## Different from EduAdvisor

EduAdvisor's example addresses course prerequisites and graduation rules within university documents. This project addresses scholarship requirements across three providers, making award-cycle separation and country context central concerns. It will explain requirements and cite sources, but will not determine personal eligibility or recommend an award based on sensitive applicant information. Its evaluation includes unanswerable eligibility questions and mixed-year evidence cases.

## Responsible AI Risks

| Risk | Who could be harmed and how | Planned engineering controls |
|---|---|---|
| Hallucinated or misread requirements | Applicants could omit required documents or rely on an invented condition | Cite supporting passages; evaluate factual claims; abstain when support is missing; refer final interpretation to the provider. |
| Outdated or mixed-cycle answers | Applicants could follow a rule from a different award year or country | Filter by scheme/year/country; show source version; ask clarifying questions; decline unsupported deadline claims. |
| Privacy leakage | Applicants could disclose passport, financial or academic records in chat or logs | Do not request document uploads or sensitive profiles; use synthetic cases; minimise/redact logs; keep credentials and private data out of GitHub. |
| Uneven coverage and over-reliance | Users of poorly covered schemes or language backgrounds could receive weaker explanations or mistake omitted information for ineligibility | State English-only, three-scheme coverage; report quality separately by scheme; do not rank users or issue eligibility decisions; disclose missing sources. |
| Prompt injection through source content | Malicious or altered content could redirect the assistant away from the evidence | Ingest an allowlisted corpus; treat retrieved passages as data; separate instructions from evidence; test adversarial queries. |

## Semester Feasibility and Boundaries

Start with three documents, English text questions and a simple Q&A interface. Proposed flow: official source metadata and permitted local documents -> text extraction and chunking -> indexed retrieval with context filters -> model explanation -> citation/scope checks -> user response. These are proposed components, not implemented features. Model and storage choices will follow a feasibility and cost check; Week 1 requires no model training, deployment or Colab coding.

Exclude application submission, success prediction, applicant ranking and autonomous decisions. Do not collect actual transcripts, identity documents or financial records. Keep a direct link to each official source available even when the model cannot answer.

## Week 1 Engineering Record

The repository contains this individual scope, its README, placeholder development folders and copies of the Group 2 Part A/B files. The existing observation sheet explicitly lacks verified behavioural evidence and some group details; its completion is still outstanding. The ChatGPT system map is group learning evidence and is not presented as this project's architecture. GitHub and Moodle completion must be verified separately from preparing these documents.
