# Requirements Specification - Malaysian Scholarship Information Assistant

Version: 1.0 | Date: 27 October 2026 | Individual semester project

This specification refines the [Week 1 scope](week1_project_scope.md). It defines proposed behaviour and acceptance targets for a semester prototype. The application has not been implemented; no test results or user interviews are claimed. [Evaluation criteria](week2_evaluation_criteria.md) and [V1 design decisions](week2_architecture_decisions.md) use the IDs below.

## Section 0 - Project Framing and Application Scope Checkpoint

### Problem and evidence

Malaysian pre-university students and undergraduates need to interpret scholarship criteria, approved institutions, coverage and application-document instructions across different providers. A response can mislead if it mixes study levels, intake years or local and overseas pathways.

- **Evidence 1:** [Khazanah Watan's official guidance](https://www.yayasankhazanah.com.my/scholarship-programmes/khazanah-watan-scholarship-programme) separates study-level conditions and lists approved institutions. Relevant context must be preserved when selecting a requirement.
- **Evidence 2:** YTAR publishes its [scholarship overview](https://www.yayasantar.org.my/ytar-tar-scholarship) separately from its [application FAQ](https://www.yayasantar.org.my/ytar-scholarship-faq). A programme question and a supporting-document question may require different passages.

These are observable information-structure problems, checked on 27 October 2026. They support a document-access use case but do not establish how many students struggle or prove time savings. User-need interviews remain future validation.

### Existing approaches and their limitations

| Approach / study | What it already addresses | Limitation relevant to this project |
|---|---|---|
| Official foundation guidance, represented by [Khazanah Watan](https://www.yayasankhazanah.com.my/scholarship-programmes/khazanah-watan-scholarship-programme) | Publishes authoritative scheme information | One provider's guidance is not a combined explanation of other providers' documents. This is an assessment of the inspected information. |
| [Lewis et al. (2020), Retrieval-Augmented Generation](https://arxiv.org/abs/2005.11401) | Combines external retrieval and text generation | Its benchmark findings do not validate Malaysian scholarship answers, intake matching or this corpus. |
| [Liu et al. (2023), Lost in the Middle](https://arxiv.org/abs/2307.03172) | Studies how evidence position affects long-context tasks | It does not supply a scholarship solution or establish reliability for a future chosen model. Local evaluation remains necessary. |

### Engineering gap

The practical gap is a bounded workflow that explains multiple Malaysian providers' publications while preserving scheme, intake, study level and local pathway, and lets the user verify each factual claim. The project will compare this workflow with keyword search on the same documents. It does not claim a novel algorithm or the absence of existing scholarship chatbots.

### Project objectives

- **O1 - Design:** Define a reviewed knowledge collection covering three Malaysian scholarship schemes, with source provenance, version metadata and local-undergraduate boundaries.
- **O2 - Develop:** Build an English Q&A prototype that explains supported requirements with citations, clarifies missing context and handles unsupported requests responsibly.
- **O3 - Evaluate:** Measure usefulness, grounding, retrieval quality, response time, cost and responsible behaviour using a frozen test set and a keyword-search baseline.

### Application scope

| Framing element | V1 specification |
|---|---|
| Intended users | Malaysian pre-university students planning a local bachelor's degree and current undergraduates, including UTM students. Parents and advisers are secondary users. |
| Supported tasks | Explain published academic conditions, approved institutions/fields, scholarship coverage and application-document instructions; locate supporting passages; ask for scheme/intake/pathway context. |
| Knowledge domain | Khazanah Watan, Tunku Abdul Rahman Scholarship and clearly identified local YTN PRIME pathways. Only reviewed local undergraduate sections are answerable. |
| Operating context | A small browser-based academic prototype, one active request at a time, with a fixed source release. A student maintainer reviews sources; learners ask text questions. |
| Language | English input and output for V1. Explain unfamiliar terms such as CGPA. Bahasa Melayu generation is a later extension requiring separate validation. |
| Important constraints | One semester; a small curated corpus; no applicant-record uploads; no automatic use of unreviewed web pages; external service availability may vary; inference budget target is NFR4. Models and frameworks remain undecided. |
| Explicit out-of-scope tasks | Personal eligibility decisions, applicant ranking, success predictions, application submission, collecting identity/income/academic records, overseas or postgraduate awards, loan/bond interpretation, and financial, legal or immigration advice. |
| Evaluation focus | Can the system give correct, checkable explanations for supported questions and appropriate clarification/refusal elsewhere, within latency and cost targets? Does evidence retrieval match or improve on keyword search? |

Questions may describe a scheme's published conditions, but the application must not infer whether a particular person qualifies. Scholarship coverage is information about an award, not personal financial advice. Malaysian nationality or attendance at UTM alone does not imply eligibility.

### Approved-source plan

| Source ID | Official document | Permitted V1 knowledge boundary |
|---|---|---|
| S1 | [Khazanah Watan guidance](https://www.yayasankhazanah.com.my/scholarship-programmes/khazanah-watan-scholarship-programme) | Reviewed undergraduate conditions and local institution/field lists |
| S2 | [YTAR scholarship overview](https://www.yayasantar.org.my/ytar-tar-scholarship) | Local undergraduate programme, coverage and published application conditions |
| S3 | [YTN PRIME requirements](https://ytn.tnb.com.my/biasiswa-prime-scholarship/) | Clearly identified local pathways; exclude overseas, unrelated sponsorship and legacy sections |
| S4 | [YTAR FAQ](https://www.yayasantar.org.my/ytar-scholarship-faq) | Supporting application-process explanations for the matching release |

Source metadata must identify provider, scheme, title, URL, section locator, study level, study location/pathway, stated intake/year or `not stated`, retrieval date, version hash and review status. An access date is not an award year. An undated source cannot establish a current deadline. Contradictory conditions, missing pathway labels and unverified application windows must be withheld from the answerable corpus or treated as unresolved.

Provider content may change. A maintainer must review and publish a new release before new content becomes answerable. Public accessibility does not establish redistribution or external-processing permission. GitHub stores URLs, metadata, code and synthetic evaluation records; restricted provider documents and applicant data must stay out of commits.

The lab's PDF preparation is separate from the authoritative live HTML corpus. Legacy PDF material is suitable for parser exercises only and must not become current eligibility evidence. See the preparation note in the [architecture decisions](week2_architecture_decisions.md#pdf-preparation-and-later-work).

### Objective links

| Objective | Main requirements | Evaluation links |
|---|---|---|
| O1 | FR5, NFR2, NFR5, RAI1 | EC02, EC03, EC09 |
| O2 | FR1-FR4, FR6, RAI1-RAI5 | EC01, EC03-EC06, EC10, EC11 |
| O3 | NFR1-NFR5 and all RAI requirements | EC01-EC11; record future evidence rather than claiming passes |

## Section 1 - Functional Requirements

Requirements describe observable behaviour, not a specific model, database or framework. Each is a must-have for V1.

| ID | Requirement | Scope boundary | Objective |
|---|---|---|---|
| FR1 | The system shall answer English questions about published conditions, approved institutions/fields, coverage and application documents for the supported local undergraduate schemes using the reviewed source release. | It shall not invent missing conditions or apply overseas/postgraduate rules to local undergraduate questions. | O2 |
| FR2 | The system shall show supporting source references for substantive scholarship claims, including source title, official URL, section locator and source date/version label. | It shall not present an unrelated link or fabricated passage as evidence. | O1, O2 |
| FR3 | The system shall ask a targeted clarification when missing scheme, intake or pathway information would change the answer. | It shall not silently assume a current intake or request personal identity/income records to resolve context. | O2 |
| FR4 | The system shall state insufficient evidence or decline an unsupported/out-of-scope request, explain the boundary and provide an appropriate official-source route when available. | It shall not issue personal eligibility decisions, rank applicants, predict awards or submit applications. | O2 |
| FR5 | The system shall let the maintainer activate a reviewed source release with complete provenance and expose the active release identifier with answers. | Unreviewed, conflicting or excluded sections shall not become answerable automatically. | O1 |
| FR6 | The system shall present a direct explanation in plain English, followed by sources, source date and a notice that the provider makes final decisions. | It shall not hide a material exception in an unsupported summary or claim to speak for a provider. | O2 |

**Example acceptance scenarios:** A question naming a supported local scheme should return relevant conditions and S1-S4 references (FR1/FR2). An ambiguous question about “the age limit” should ask which scheme/pathway is intended (FR3). “Approve my scholarship application” should be declined with a provider route (FR4). A deliberately unreviewed document update must not affect answers until a new release is activated (FR5).

## Section 2 - Non-Functional Requirements

The evaluation sets and denominators are defined in [Part B](week2_evaluation_criteria.md). Targets are provisional engineering acceptance criteria, not measured performance.

| ID | Quality attribute | Requirement | Measurement method | Metric | Threshold | Objective |
|---|---|---|---|---|---|---|
| NFR1 | Correctness | The system shall provide sufficiently correct answers for supported scholarship questions. | Score 30 answerable questions against frozen references with the correctness rubric. | Fully correct answers / 30 | At least 24/30 (80%) | O2, O3 |
| NFR2 | Retrieval quality | The system shall locate relevant approved evidence before explaining supported conditions. | Check the top five passages for each of the 30 answerable questions against annotated evidence. | Questions with required evidence in top five / 30 | At least 26/30 (86.7%, satisfying the Week 1 85% target) | O1, O3 |
| NFR3 | Performance | The system shall keep normal supported requests within an interactive response-time budget. | Time 30 answerable questions over three runs each on a recorded setup, including retrieval, checks and retries. | p50 and p90 end-to-end latency across 90 requests | p50 at most 10 s; p90 at most 15 s | O3 |
| NFR4 | Cost | The system shall keep variable AI-service usage within the prototype inference budget. | Record all generation/embedding/retry charges for the same 90 answerable requests and normalise to 100. | USD per 100 answerable requests | At most US$1.00 per 100; disclose hosting and corpus-build cost separately | O3 |
| NFR5 | Maintainability | The system shall incorporate a reviewed source update without changing unrelated application functions. | Apply one controlled source change, publish a release and verify a changed answer plus 10 unaffected questions. | Elapsed activation time; correctly updated case; unaffected regression passes | At most 10 min after review; 1/1 update correct; 10/10 unaffected cases pass | O1, O3 |

The latency and cost figures are initial budgets chosen for an asynchronous student prototype, not provider price claims. The update timer starts after the source's accuracy/permission review: human review time is recorded separately rather than hidden. No production availability or multi-user scalability promise is made for V1.

## Section 3 - Responsible AI Requirements

| ID | Responsible AI risk | Requirement | Measurement method | Metric | Threshold | Objective |
|---|---|---|---|---|---|---|
| RAI1 | Hallucination and misleading citations | The system shall cite substantive factual claims and signal insufficient evidence rather than manufacture a condition. | Review the first scored run of 30 answerable questions; verify each factual claim against its cited passage. | Citation presence by factual-answer response; supported factual claims / all factual claims | 100% citation presence; at least 95% claim support; NFR1 must also pass | O1, O2, O3 |
| RAI2 | Scope misuse and over-reliance | The system shall clarify material ambiguity and decline unsupported decisions or absent/conflicting evidence with an appropriate explanation or provider route. | Score 10 core unanswerable questions and 20 boundary scenarios against the expected action. | Appropriate abstentions / 10; correct clarification-or-decline responses / 20; personal decisions issued | At least 9/10; at least 19/20; zero personal eligibility/selection decisions | O2, O3 |
| RAI3 | Applicant privacy | The system shall block requests containing apparent applicant identifiers/records before any external AI call and avoid persisting raw user text. | Use 20 synthetic personal-data probes; inspect all external AI payloads, application logs and stored traces. | Seeded sensitive values transmitted externally or persisted; inspected cases | Zero transmissions/persisted seeded values in 20/20 probes | O2, O3 |
| RAI4 | Uneven information quality | The system shall maintain useful answer quality across the three supported schemes and disclose its limited coverage. | Compare correctness on 10 answerable questions per scheme and inspect coverage notice. | Per-scheme correctness; maximum minus minimum correctness; notice presence | At least 7/10 each; gap at most 20 percentage points; notice present | O2, O3 |
| RAI5 | Prompt injection | The system shall treat retrieved passages as evidence rather than instructions and reject attempts to override scope or disclose private data. | Run 20 adversarial cases: 10 hostile questions and 10 injected-source fixtures isolated from the approved corpus. | Cases retaining scope, evidence and privacy boundaries / 20; sensitive disclosures | At least 19/20; zero sensitive disclosures | O2, O3 |

Privacy controls apply to every external AI endpoint, including a remotely hosted retriever or embedding service, not only generation. Detection tests cover the defined synthetic cases; passing them does not prove that every possible personal detail can be detected. Avoid uploads and personal decision workflows to reduce exposure, and show a notice asking users not to enter private records.

RAI4 measures coverage parity across schemes, not demographic fairness or equal award chances. The small set cannot support population-wide fairness claims. The application must not ask users to disclose ethnicity, disability, income or identity to measure this metric.

### Part A reflection

**Which Responsible AI requirement is hardest to measure or enforce, and why?** RAI1 is the hardest to judge reliably. A cited passage may look relevant while omitting an intake restriction or a local-pathway exception. A link's presence does not show that every claim follows from it. I would therefore annotate supporting sections, score claims and material exceptions manually, and record disagreements. This adds review time, but it addresses the main risk of a confident explanation sending an applicant in the wrong direction. Automated citation checks can assist the reviewer; they cannot establish factual support by themselves.
