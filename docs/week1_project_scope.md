# Week 1 Project Scope

**Submission type:** Individual semester project  
**Status:** Preliminary; to be refined in Topic 2  
**Prepared:** 27 October 2026  
**Repository name:** `ai-engineering-scholarship-assistant`

## Project Name

Malaysian Scholarship Information Assistant

## Domain

Malaysian higher education / undergraduate scholarship information, initially covering scholarships for study at universities in Malaysia.

## Problem Statement

Malaysian pre-university students and undergraduates need to interpret scholarship criteria, approved institutions, benefits and supporting-document requirements across multiple providers. Conditions may differ by study level, intake year, university and pathway. Users may mistakenly apply an overseas or postgraduate condition to a local undergraduate question.

The project proposes a document-grounded Q&A assistant for three Malaysian schemes: Khazanah Watan, the Tunku Abdul Rahman Scholarship and local YTN PRIME pathways. Semantic retrieval can connect everyday questions to formal eligibility language, while generation can explain several relevant passages with citations. Keyword search will provide a baseline for judging added value.

The initial scope is local undergraduate study, using English Q&A. Overseas scholarships, postgraduate awards and Bahasa Melayu generation are outside the first prototype. The system explains published rules and never determines a person's eligibility, ranks applicants or submits applications.

## Evidence of the Problem

### Evidence 1: Requirements depend on the correct study-level section

Khazanah Watan's official guidance has separate qualification sections and approved-university information. This demonstrates a need to preserve section and study-level context when retrieving requirements.

**Source / citation:** [Yayasan Khazanah: Khazanah Watan Scholarship Programme](https://www.yayasankhazanah.com.my/scholarship-programmes/khazanah-watan-scholarship-programme), accessed 27 October 2026.

### Evidence 2: Application information spans multiple official documents

YTAR's scholarship page and separate FAQ contain programme information and practical application guidance. This demonstrates that an applicant's question may require more than one official document.

**Sources / citations:** [YTAR scholarship overview and requirements](https://www.yayasantar.org.my/ytar-tar-scholarship) and [YTAR scholarship FAQ](https://www.yayasantar.org.my/ytar-scholarship-faq), accessed 27 October 2026.

**Evidence boundary:** These are observations about the information structure, not measured evidence of widespread confusion or time loss. Topic 2 should validate user need with Malaysian students through a small consent-based usability exercise or interviews without collecting sensitive applicant records.

## Initial Related Work (at least 3)

### 1. Official Malaysian scholarship guidance

**Study / solution:** Yayasan Khazanah's official Khazanah Watan guidance.

**Source / citation:** [Khazanah Watan Scholarship Programme](https://www.yayasankhazanah.com.my/scholarship-programmes/khazanah-watan-scholarship-programme), accessed 27 October 2026.

**Relevance:** Authoritative provider text should remain the source of truth and a direct-navigation baseline.

**Key limitation:** The inspected guidance addresses one provider rather than explaining rules across different Malaysian scholarship providers. A user must consult other providers separately. This is a limitation of using the inspected document as a complete information source, not a claim about all services offered by the foundation.

### 2. Retrieval-Augmented Generation for Knowledge-Intensive NLP Tasks

**Study / solution:** Lewis et al. (2020), NeurIPS.

**Source / citation:** [Paper](https://arxiv.org/abs/2005.11401).

**Relevance:** Combines retrieved external evidence with language generation; motivates the prototype's document-grounded approach.

**Key limitation:** Its benchmark results do not validate this scholarship corpus or establish reliable handling of Malaysian intake years, study pathways or eligibility explanations. These controls and tests must be implemented locally. This is a limitation of applying the study to this project, not a finding that the paper tested scholarships.

### 3. Lost in the Middle: How Language Models Use Long Contexts

**Study / solution:** Liu et al. (2023 preprint), accepted for TACL publication.

**Source / citation:** [Paper](https://arxiv.org/abs/2307.03172).

**Relevance:** Reports that relevant information's position can affect performance in tested long-context tasks. This motivates selective retrieval and careful evidence placement instead of assuming that a whole handbook will be used reliably.

**Key limitation:** It analyses particular models and tasks rather than delivering a scholarship assistant. Its results do not establish the performance of the future chosen model; this project needs its own retrieval and answer tests.

## Engineering Gap / Limitation

The practical challenge is to build and evaluate a small scholarship Q&A workflow that preserves Malaysian provider, intake/year, document version, study level and local study pathway while giving explanations supported by specific passages. Official publications provide authority, but users must still navigate and relate their contents. General RAG research supplies an approach, not a complete solution for this context.

The project will test whether metadata-aware retrieval and cited explanations improve answer usefulness over keyword search without inventing conditions. It will not claim a novel retrieval algorithm or that no existing scholarship service solves similar problems.

## Initial Project Objectives (2–3)

- **O1: To design** a curated corpus of three official Malaysian scholarship documents, with a source register containing title, Malaysian provider, scheme, intake/year, version, study level, study location/pathway, URL, access date and permitted-use notes.
- **O2: To develop** an English retrieval-augmented Q&A prototype with passage citations, scheme/intake/study-level clarification, context filters and abstention for missing or conflicting evidence.
- **O3: To evaluate** correctness, citation support, retrieval recall, latency and appropriate abstention against a frozen set of 40 manually checked questions and a keyword-search baseline.

### Initial evaluation plan

Build 30 answerable English questions balanced across the three Malaysian schemes and 10 questions whose answers are absent, conflicting or outside scope. Include mixed-intake, local-versus-overseas, undergraduate-versus-postgraduate, approved-university and unsupported personal-eligibility cases. Keep a separate development set. Annotate reference answers and supporting passages before evaluation, and keep scheme/year context fixed for a fair baseline comparison. Use synthetic scenarios only.

| Dimension | Proposed measurement | Preliminary target |
|---|---|---|
| Answer correctness | Fully correct answers / 30 answerable questions, using a written rubric | At least 80% |
| Citation support | Supported factual claims / all factual claims in generated answers, checked against cited passages | At least 90% |
| Retrieval quality | Answerable questions with a reference passage in the top 5 results / 30 | At least 85% |
| Abstention | Appropriate abstentions / 10 unanswerable or out-of-scope questions | At least 90% |
| Latency | Median submission-to-complete-response time on the recorded test setup | At most 10 seconds |

Record answer coverage alongside abstention so that refusing every question cannot count as success. Compare retrieval recall and time to locate evidence against keyword search; assess generated answer quality separately. Report denominators, model configuration and failures rather than implying these preliminary targets are achieved. Refine targets after an initial feasibility test in Topic 2.

## Target Users

**Primary users:** Malaysian pre-university students planning a local bachelor's degree and current undergraduates seeking scholarship information, including UTM students.

**Secondary users:** Parents, school counsellors and university advisers checking official requirements.

Supported scholarships may restrict study stage, institution, subject or other criteria. Being Malaysian or studying at UTM does not establish eligibility for any particular award. The interface must show which scheme and local pathway an answer covers.

## Data Sources (at least 2)

| Document | Provider / scope | Public URL |
|---|---|---|
| Khazanah Watan Scholarship Programme guidance | Yayasan Khazanah; undergraduate sections for Malaysian universities | [Official guidance](https://www.yayasankhazanah.com.my/scholarship-programmes/khazanah-watan-scholarship-programme) |
| Tunku Abdul Rahman Scholarship overview and requirements | Yayasan Tunku Abdul Rahman (YTAR); undergraduate study in Malaysia | [Official guidance](https://www.yayasantar.org.my/ytar-tar-scholarship) |
| Biasiswa PRIME Scholarship requirements | Yayasan Tenaga Nasional (YTN); local university pathways only | [Official guidance](https://ytn.tnb.com.my/biasiswa-prime-scholarship/) |

These are three publicly accessible HTML documents, checked on 27 October 2026. The [YTAR application FAQ](https://www.yayasantar.org.my/ytar-scholarship-faq) is a supporting fourth document. Pin permitted snapshots and record section, scheme, study level, study location, intake/year and retrieval date before ingestion. The YTN page contains several pathways and legacy material; only clearly identified local-pathway sections may be indexed, and unclear conditions require provider confirmation. Inclusion does not imply that applications are currently open or that every student is eligible. Source text is not redistributed here.

**Source maintenance:** These written HTML guidance documents form the proposed initial corpus. Before ingestion, verify permitted processing, quotation and any external model transmission. Public access does not itself grant redistribution rights. Commit URLs and metadata; keep provider documents and applicant records out of GitHub.

**Version policy:** Record hashes for permitted snapshots, preserve section headings and exclude navigation/footer text and unrelated pathways. Separate intake years rather than overwriting metadata silently. Recheck sources before each release. For mixed-year, contradictory or undated criteria, abstain and direct the user to the provider. Recurring application months or old notices do not prove that a current application window is open.

## Similar to EduAdvisor

Both systems serve students by retrieving official educational information and generating cited explanations. Both require document cleaning, chunking, retrieval, a language model, an interface, scope guardrails, evaluation and a feedback process. Accuracy, groundedness and latency are relevant to both.

## Different from EduAdvisor

EduAdvisor's example addresses course prerequisites and graduation rules within university documents. This project addresses Malaysian undergraduate scholarship requirements across three providers. It must distinguish local study pathways, intake years, approved institutions and published criteria. It provides explanations and citations without personal eligibility decisions. Its evaluation includes mixed-intake evidence, unsupported personal-eligibility questions and confusion between local, overseas and postgraduate pathways.

## Responsible AI Risks

| Risk | Who could be harmed and how | Planned engineering controls |
|---|---|---|
| Hallucinated or misread requirements | Applicants could omit required documents or rely on an invented condition | Cite supporting passages; evaluate factual claims; abstain when support is missing; refer final interpretation to the provider. |
| Outdated or mixed-cycle answers | Applicants could follow a rule from a different intake year, study level or study pathway | Filter by scheme/intake/study level/local pathway; show source version; ask clarifying questions; decline unsupported deadline claims. |
| Privacy leakage | Applicants could disclose passport, financial or academic records in chat or logs | Do not request document uploads or sensitive profiles; use synthetic cases; minimise/redact logs; keep credentials and private data out of GitHub. |
| Uneven coverage and over-reliance | Users of poorly covered schemes or language backgrounds could receive weaker explanations or mistake omitted information for ineligibility | State English-only, three-Malaysian-scheme and local-undergraduate coverage; report quality separately by scheme; do not rank users or issue eligibility decisions; disclose missing sources. |
| Prompt injection through source content | Malicious or altered content could redirect the assistant away from the evidence | Ingest an allowlisted corpus; treat retrieved passages as data; separate instructions from evidence; test adversarial queries. |

## Semester Feasibility and Boundaries

Start with three official Malaysian scholarship documents, English text questions and a simple Q&A interface. Limit retrieval to local undergraduate pathways and use the YTAR FAQ as supporting material. Proposed flow: official source metadata and permitted local documents -> text extraction and chunking -> indexed retrieval with context filters -> model explanation -> citation/scope checks -> user response. These are proposed components, not implemented features. Model and storage choices will follow a feasibility and cost check; Week 1 requires no model training, deployment or Colab coding.

Exclude application submission, success prediction, applicant ranking and autonomous decisions. Do not collect actual transcripts, identity documents or financial records. Keep a direct link to each official source available even when the model cannot answer.

## Week 1 Engineering Record

The repository contains this individual scope, its README, placeholder development folders and copies of the Group 2 Part A/B files. The existing observation sheet explicitly lacks verified behavioural evidence and some group details; its completion is still outstanding. The ChatGPT system map is group learning evidence and is not presented as this project's architecture. The repository has been published on GitHub. The user will handle Moodle submission. The README and scope now focus on Malaysian undergraduate scholarships; group evidence remains subject to its recorded limitations.
