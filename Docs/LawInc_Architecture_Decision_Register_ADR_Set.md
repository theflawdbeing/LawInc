# LawInc — Architecture Decision Register & ADR Set

**Document type:** Architecture Decision Register + Architecture Decision Records  
**Status:** Proposed governance baseline  
**Version:** 1.0  
**Date:** 17 August 2026  
**Project:** LawInc  
**Scope:** Central Indian law — Traffic & Motor Vehicle, Income Tax / GST / Financial Compliance, Cybercrime / Digital & Online Transactions

---

## 0. Purpose

This document makes the supplied **Universal Project Design Decision Framework** the governing methodology for LawInc architecture decisions.

The objective is not to prove that a technology is universally “best”. The objective is to make each consequential decision **defensible, contextual, evidence-based, explicit about uncertainty, and easy to revisit**.

The governing lifecycle is:

```text
IDENTIFY
   ↓
UNDERSTAND CONTEXT
   ↓
DEFINE CONSTRAINTS
   ↓
IDENTIFY DECISION DRIVERS
   ↓
GENERATE ALTERNATIVES
   ↓
ELIMINATE INVALID OPTIONS
   ↓
EVALUATE
   ↓
TRADE-OFF ANALYSIS
   ↓
POC / EVIDENCE
   ↓
SELECT
   ↓
DOCUMENT
   ↓
IMPLEMENT
   ↓
MONITOR
   ↓
REVIEW
   ↓
KEEP / MODIFY / REPLACE
```

### Decision rules adopted for LawInc

1. Requirements precede technology.
2. Hard constraints eliminate options before weighted scoring.
3. At least three credible alternatives should be considered for important decisions.
4. The simplest option satisfying the requirements is preferred.
5. Difficult-to-reverse decisions require stronger evidence.
6. Every accepted shortcut must have an explicit revisit condition.
7. Estimates are never presented as facts.
8. Architecture decisions must capture system-wide and second-order effects.
9. AI recommendations must be validated using LawInc-specific evaluation data.
10. An LLM may generate alternatives and analysis, but it does not make the final decision.
11. “What would change our mind?” is mandatory for material decisions.
12. A decision remains valid only while its requirements, constraints, evidence and assumptions remain valid.

---

# 1. Decision Context — LawInc Baseline

## 1.1 Product definition

LawInc is a legal-awareness application for Indian citizens focused only on three Central-law areas:

### Domain 1 — Traffic & Motor Vehicle Laws

Examples:

- driving without a valid licence;
- insurance requirements;
- PUC requirements;
- helmet/seat-belt rules;
- drunk driving;
- speed limits;
- wrong-side driving;
- parking/traffic violations;
- modified vehicles;
- excess passengers;
- driving another person's vehicle.

### Domain 2 — Income Tax / GST / Financial Compliance

Examples:

- taxability of income;
- TDS;
- ITR filing;
- capital gains;
- freelancing/side income;
- foreign income;
- interest income;
- deductions;
- GST registration;
- tax notices;
- penalties and interest.

### Domain 3 — Cybercrime / Digital & Online Transactions

Examples:

- UPI fraud;
- online scams;
- OTP/PIN sharing;
- identity theft;
- use of another person's account;
- impersonation;
- cyber harassment;
- threats;
- defamation;
- copyright infringement;
- illegal/private content;
- unauthorized access;
- fraudulent digital transactions.

## 1.2 Explicit exclusions

- State-specific law.
- Tenancy.
- Employment/labour law.
- Consumer law.
- General constitutional-law coverage as a standalone domain.
- Foreign-national/visa law.
- Court representation.
- Guaranteed legal outcomes.
- Automated legal filing/action on behalf of a user.

## 1.3 Current project constraints

| Constraint | Current value | Confidence |
|---|---|---|
| Team | Solo | High |
| Build timeline | 2 weeks | High |
| Initial real users | <50, primarily friends/testing | High |
| Budget | Strictly $0 / free-tier cloud | High |
| Hosting | Cloud, not local | High |
| End-user geography | India | High |
| Jurisdiction | Central law only | High |
| AI | External model/API | High |
| Compliance | Informational MVP; formal compliance work deferred | High |
| Future scale | Not quantified | Unknown |
| Commercial intent | Not yet explicitly fixed | Unknown |
| Required languages | English baseline; Hindi/Hinglish not yet fixed | Unknown |
| Retention period | Not yet fixed | Unknown |
| Availability target | No formal SLA | Unknown |

---

# 2. Architecture Decision Register

| ID | Decision | Status | Confidence | Outcome |
|---|---|---|---|---|
| ADR-001 | Freeze V1 scope and boundaries | Accepted | High | Keep |
| ADR-002 | Legal-source authority hierarchy | Accepted | High | Add formal hierarchy |
| ADR-003 | Source freshness and legal-version model | Accepted | High | Strengthen significantly |
| ADR-004 | Modular monolith vs microservices | Accepted | High | Keep modular monolith |
| ADR-005 | Cloud deployment model | Modified | High | Vercel only if non-commercial; otherwise portable runtime needed |
| ADR-006 | Frontend/backend stack | Accepted | High | Next.js + TypeScript |
| ADR-007 | Database | Accepted | High | PostgreSQL |
| ADR-008 | Vector storage | Accepted | High | pgvector for V1 |
| ADR-009 | Hybrid retrieval vs pure vector search | Modified | High | Hybrid retrieval required |
| ADR-010 | RAG vs fine-tuning | Accepted | High | RAG |
| ADR-011 | Embeddings | Proposed | Medium | POC required before final choice |
| ADR-012 | Primary LLM provider | Proposed | Medium | POC required; do not hard-freeze yet |
| ADR-013 | LLM fallback strategy | Accepted in architecture / implementation pending | Medium | Provider abstraction + fallback |
| ADR-014 | Prompt architecture | Accepted | High | Versioned structured prompts |
| ADR-015 | Citation/grounding validation | Accepted | High | Deterministic server-side validation |
| ADR-016 | Agents/tool calling | Accepted | High | Do not use in V1 |
| ADR-017 | Memory | Accepted | High | Short-lived session context only |
| ADR-018 | End-user authentication | Accepted | High | Anonymous sessions in V1 |
| ADR-019 | Admin authentication/authorization | Accepted | High | Protected admin identity |
| ADR-020 | Data retention/privacy | Proposed | Medium | Minimize retention; exact policy still required |
| ADR-021 | Corpus ingestion | Accepted | High | Versioned offline/CI pipeline |
| ADR-022 | Caching | Accepted | Medium | Conservative version-aware caching |
| ADR-023 | Asynchronous processing | Accepted | High | CI jobs first; queue later |
| ADR-024 | CI/CD | Accepted | High | GitHub Actions + deployment platform |
| ADR-025 | Observability | Accepted | High | Platform-native + structured app telemetry |
| ADR-026 | Backup/DR | Modified | High | Reproducible corpus + DB exports; paid PITR before real production |
| ADR-027 | Free-tier cost architecture | Accepted with caveats | High | $0 MVP target, not guarantee |
| ADR-028 | Scalability migration | Accepted | High | Scale when measured bottleneck appears |
| ADR-029 | AI evaluation | Accepted | High | Golden set is a release gate |
| ADR-030 | Legal safety / fail-closed policy | Accepted | High | No authoritative retrieval → no authoritative answer |

---

# 3. Re-evaluation Summary

The previous architecture was directionally correct but had several decisions that were too quickly frozen.

## Decisions retained

- modular monolith;
- TypeScript/Next.js;
- PostgreSQL;
- pgvector;
- RAG;
- provider abstraction;
- anonymous sessions;
- versioned prompt architecture;
- deterministic citation validation;
- no agents;
- no Kubernetes/microservices/Kafka/Redis in V1;
- CI-driven corpus ingestion.

## Decisions changed

### 1. Vercel is no longer an unconditional hosting decision

Current Vercel documentation states that the Hobby plan is for personal/non-commercial use. Vercel currently lists Hobby at $0 with bounded usage. Therefore it is suitable for a personal/testing MVP only if the project's actual usage remains within those terms. A public/commercial launch requires re-evaluation, likely toward a paid Vercel plan or another provider. [Vercel pricing](https://vercel.com/pricing) · [Hobby plan](https://vercel.com/docs/plans/hobby)

### 2. Pure vector retrieval is rejected

Legal questions frequently contain exact identifiers such as section numbers, notification numbers, Act names, dates and tax concepts. The retrieval architecture should therefore use **metadata + lexical/exact matching + vector similarity**, with optional reranking.

### 3. Static “Act + section” corpus is rejected

The current tax environment demonstrates why the corpus must model **Acts, rules, notifications, circulars, forms, amendments, publication dates, effective dates and supersession**. The Income Tax Department now publishes material for the Income Tax Act 2025, while CBIC's tax information portal explicitly includes acts, rules, notifications, circulars, orders and continuous updates. [Income Tax Department — Income Tax Act 2025](https://www.incometax.gov.in/iec/foportal/newdownloads/income-tax-act-2025) · [CBIC Tax Information Portal](https://taxinformation.cbic.gov.in/)

### 4. Embedding/LLM choices remain provisional

The previous document selected Gemini and Groq too early. They remain strong candidates, but the final choice should follow a LawInc-specific POC measuring quality, groundedness, latency, structured output and free-tier feasibility.

### 5. Legal-source hierarchy becomes a first-class architectural component

A government source is not automatically interchangeable with another government source. The system needs an authority hierarchy and conflict-resolution rules.

---

# 4. Standard ADR Format

Every LawInc ADR below follows the governing framework:

1. Decision
2. Classification
3. Context
4. Requirements
5. Constraints
6. Assumptions
7. Decision Drivers
8. Alternatives
9. Hard-Constraint Elimination
10. Evaluation Criteria
11. Decision Matrix
12. Trade-offs
13. Quantitative / Evidence Review
14. POC / Validation
15. Decision
16. Reversibility
17. Second-Order Effects
18. System-Wide Impact
19. Security/Privacy Impact
20. Cost Impact
21. Scalability Impact
22. Failure & Recovery
23. Technical Debt
24. Confidence
25. Revisit Conditions

---

# ADR-001 — Freeze the V1 Product Scope

**Status:** Accepted  
**Date:** 2026-08-17  
**Classification:** Product / Architecture  
**Reversibility:** Type 2 — moderately reversible

## Decision

LawInc V1 will support exactly three Central-law domains:

- Traffic & Motor Vehicle Laws
- Income Tax / GST / Financial Compliance
- Cybercrime / Digital & Online Transactions

## Context

The project previously contained six domains. The current scope reduces the product to three areas where users commonly experience financial, regulatory or legal consequences.

## Requirements

- Keep MVP buildable in two weeks.
- Avoid state-law branching.
- Avoid high-risk foreign-national/visa content.
- Focus corpus engineering on three domains.

## Constraints

- Solo developer.
- <50 test users.
- $0 budget.
- Cloud deployment.
- Two-week timeline.

## Assumptions

- Future domain expansion is not required for V1.
- Central-law-only is a hard product boundary.

## Decision Drivers

| Driver | Priority |
|---|---|
| Buildability | Critical |
| Legal correctness | Critical |
| Corpus maintainability | Critical |
| User usefulness | High |
| Future extensibility | Medium |

## Alternatives

A. Keep six domains.  
B. Use three domains.  
C. Start with one domain.

## Hard-Constraint Elimination

A is eliminated because it increases corpus complexity without fitting the current two-week constraint. C is feasible but underuses the intended product concept. B satisfies the requirements.

## Decision Matrix

| Criterion | Weight | A Six domains | B Three domains | C One domain |
|---|---:|---:|---:|---:|
| Buildability | 30% | 5 | 9 | 10 |
| Legal safety | 30% | 6 | 9 | 10 |
| User value | 20% | 8 | 9 | 5 |
| Maintainability | 20% | 5 | 9 | 10 |
| Weighted score | 100% | 5.8 | **9.0** | 8.7 |

## Trade-offs

Gain a more coherent MVP and deeper coverage. Sacrifice breadth.

## Evidence / Validation

Scope is a direct project constraint established before architecture selection.

## POC

Not required.

## Decision

**Choose three domains.**

## Second-Order Effects

The domain router, source metadata, evaluation datasets and UI all become simpler.

## System-Wide Impact

Frontend, backend, corpus, prompts, evaluation and analytics must use only the three-domain enum.

## Security/Privacy Impact

Smaller domain scope reduces the chance of users receiving unsupported legal guidance.

## Cost Impact

Lower corpus and inference volume.

## Scalability Impact

Reduces early retrieval index size.

## Failure & Recovery

Unsupported domains must return an explicit out-of-scope result.

## Technical Debt

Potential future expansion requires formal scope review rather than silently adding content.

## Confidence

**High.**

## Revisit When

- User demand consistently requires another domain.
- Legal review establishes that another domain can be safely supported.
- Engineering capacity increases substantially.

---

# ADR-002 — Legal Source Authority Hierarchy

**Status:** Accepted  
**Classification:** Data / Security / AI  
**Reversibility:** Type 3 — difficult to change after corpus growth

## Decision

LawInc will maintain an explicit source-authority hierarchy instead of treating all retrieved documents equally.

## Context

Legal correctness depends on both source authority and temporal validity.

## Requirements

- Identify authoritative Central sources.
- Preserve source metadata.
- Resolve conflicts.
- Prevent low-authority summaries from outranking primary law.

## Proposed hierarchy

```text
Tier 1 — Primary authoritative legal text
    Acts
    Rules
    Regulations
    Gazette notifications
    Official amendments

Tier 2 — Official administrative material
    Department circulars
    Official notifications
    Forms
    Instructions
    Official FAQs where legally relevant

Tier 3 — Official operational guidance
    Government portals
    Citizen manuals
    Government awareness material

Tier 4 — Secondary explanatory sources
    Only for discovery/context, not authoritative claims

Tier 5 — Unverified web content
    Not allowed as legal evidence in V1
```

## Alternatives

A. India Code only.  
B. Official-source hierarchy.  
C. General web search.

## Hard Constraints

C fails the correctness requirement. A is insufficient because tax and regulatory questions can depend on official notifications/circulars and department material.

## Decision

Choose B.

## Evidence

India Code exposes Central Acts and associated rules for the Motor Vehicles Act. citeturn117330search1turn117330search6 CBIC's current tax-information portal explicitly organizes Acts, Rules, Notifications, Circulars, Orders and FAQ material and states that content is continuously updated. citeturn698644search0turn698644search5 The National Cybercrime Reporting Portal is an official Government of India/MHA/I4C service with reporting and citizen guidance. citeturn117330search0turn117330search7

## Trade-offs

More ingestion and metadata work, but substantially better correctness and auditability.

## System-Wide Impact

Every chunk must carry:

- authority tier;
- source organization;
- document type;
- publication date;
- effective date;
- retrieval date;
- supersession status.

## Confidence

**High.**

## Revisit When

- Expert legal review establishes a different hierarchy.
- New official source systems materially change the information landscape.

---

# ADR-003 — Versioned Legal Corpus with Effective Dates

**Status:** Accepted  
**Classification:** Data Architecture  
**Reversibility:** Type 3

## Decision

Legal content will be modeled as **versioned, time-aware source records**, not just text chunks.

## Context

A source can be amended, replaced or cease to apply. Tax/compliance content is particularly dynamic. The current Income Tax Department publishes dedicated material for the Income Tax Act 2025, and CBIC exposes continuously updated tax-law material. citeturn117330search14turn698644search0

## Requirements

Every legal source should support:

- publication date;
- effective-from date;
- effective-to date where known;
- source version;
- amendment/supersession links;
- retrieved-at timestamp;
- content hash.

## Alternatives

A. Static corpus.  
B. Versioned temporal corpus.  
C. Live query against government websites.

## Hard Elimination

A fails freshness requirements.

C creates runtime dependency and reproducibility problems.

## Decision

Choose B.

## Decision Matrix

| Criterion | Weight | A | B | C |
|---|---:|---:|---:|---:|
| Correctness | 30% | 5 | 10 | 9 |
| Reproducibility | 20% | 8 | 10 | 3 |
| Runtime reliability | 20% | 10 | 9 | 4 |
| Maintenance | 20% | 9 | 7 | 5 |
| Simplicity | 10% | 10 | 7 | 5 |
| Weighted | 100% | 7.3 | **8.9** | 5.4 |

## Trade-offs

More metadata and ingestion complexity in exchange for historical correctness.

## POC

Create sample versions for:

- one Motor Vehicles source;
- one income-tax source;
- one GST notification;
- one cyber source.

Validate date-filtered retrieval.

## Security/Privacy

Versioning reduces the risk of silently presenting stale or withdrawn information.

## Confidence

**High.**

## Revisit When

Never remove temporal metadata; only simplify implementation if evidence shows it can be represented more efficiently.

---

# ADR-004 — Modular Monolith

**Status:** Accepted  
**Classification:** Architecture  
**Reversibility:** Type 2

## Decision

Use a modular monolith for V1.

## Alternatives

A. Microservices.  
B. Modular monolith.  
C. Single-file/simple application.

## Hard Constraints

A violates the two-week/solo operational constraint. C risks architectural coupling. B fits the middle ground.

## Decision Matrix

| Criterion | Weight | A | B | C |
|---|---:|---:|---:|---:|
| Development speed | 25% | 4 | 9 | 10 |
| Maintainability | 20% | 8 | 9 | 5 |
| Operational simplicity | 20% | 3 | 10 | 10 |
| Scalability path | 15% | 10 | 8 | 4 |
| Testability | 10% | 8 | 9 | 5 |
| Cost | 10% | 3 | 10 | 10 |
| Weighted | 100% | 5.5 | **9.1** | 7.1 |

## Decision

**Keep modular monolith.**

## Trade-off

Services cannot scale independently at first.

## Revisit When

- Independent module scaling becomes measurable.
- Team grows enough to own separate services.
- Deploy coupling becomes a demonstrated bottleneck.

---

# ADR-005 — Cloud Deployment Model

**Status:** Modified  
**Classification:** Infrastructure / DevOps  
**Reversibility:** Type 2

## Decision

Use a managed serverless web platform for V1, but do **not** hard-code Vercel as a permanent architecture dependency.

## Context

Vercel Hobby is currently $0 and includes bounded function resources, but Vercel states that Hobby is restricted to non-commercial personal use. citeturn239623search1turn157544search0turn157544search1

## Alternatives

A. Vercel Hobby.  
B. Paid Vercel.  
C. Cloudflare Workers.  
D. Generic free cloud VM/container platform.

## Hard Constraints

V1 budget is $0.

Therefore B is currently disallowed.

D may be operationally harder and may not provide persistent free compute reliably.

## Decision

Use Vercel for a **personal/non-commercial testing deployment** only. Keep the application portable to another Node.js runtime.

## Decision Matrix

| Criterion | Weight | Vercel Hobby | Cloudflare Workers | Free VM |
|---|---:|---:|---:|---:|
| Next.js integration | 25% | 10 | 7 | 6 |
| $0 fit | 20% | 10 | 10 | 8 |
| Operations | 20% | 10 | 9 | 4 |
| Portability | 15% | 7 | 6 | 9 |
| Runtime flexibility | 10% | 9 | 6 | 10 |
| Current constraints | 10% | 10 | 9 | 6 |
| Weighted | 100% | **9.4** | 7.7 | 6.6 |

## Important qualification

A future public/commercial LawInc deployment cannot assume Vercel Hobby is acceptable. Vercel's terms explicitly restrict Hobby to personal/non-commercial use. citeturn157544search3

## POC

Deploy a minimal Next.js API and verify:

- database connectivity;
- model streaming;
- timeout behavior;
- build size.

## Confidence

**High for personal MVP; medium for long-term hosting choice.**

## Revisit When

- LawInc becomes commercial.
- Hosting limits are hit.
- API timeout/CPU constraints interfere with RAG processing.

---

# ADR-006 — Application Stack

**Status:** Accepted  
**Classification:** Technology  
**Reversibility:** Type 2

## Decision

Use TypeScript + Next.js for both frontend and application/API code.

## Alternatives

A. Next.js/TypeScript.  
B. React + separate FastAPI.  
C. Django/React.

## Decision

A.

## Rationale

The current team is one developer and the product needs a frontend plus backend boundary. A single TypeScript codebase minimizes coordination and deployment overhead.

## Trade-offs

Python-specific ML libraries are less directly available at runtime. Heavy ingestion/AI experimentation therefore lives in separate scripts where needed.

## Confidence

**High.**

---

# ADR-007 — Primary Database

**Status:** Accepted  
**Classification:** Data  
**Reversibility:** Type 3

## Decision

Use PostgreSQL as the system-of-record database.

## Alternatives

A. PostgreSQL.  
B. MongoDB.  
C. DynamoDB.

## Decision Matrix

| Criterion | Weight | PostgreSQL | MongoDB | DynamoDB |
|---|---:|---:|---:|---:|
| Relational modelling | 20% | 10 | 6 | 5 |
| Transactions | 20% | 10 | 7 | 8 |
| Metadata/query flexibility | 15% | 9 | 10 | 7 |
| Vector integration | 15% | 10 | 6 | 4 |
| Managed free-tier fit | 15% | 9 | 8 | 7 |
| Portability | 15% | 10 | 8 | 5 |
| Weighted | 100% | **9.7** | 7.4 | 5.9 |

## Decision

Choose PostgreSQL.

## Evidence

Supabase currently offers managed Postgres on its Free plan with a 500 MB database allowance. citeturn418488search0turn418488search3

## Revisit

When relational workloads, storage or connection patterns justify a different database architecture.

---

# ADR-008 — Vector Storage in PostgreSQL

**Status:** Accepted  
**Classification:** AI/Data  
**Reversibility:** Type 2

## Decision

Use pgvector inside PostgreSQL for V1.

## Alternatives

A. pgvector.  
B. Dedicated vector DB.  
C. Search engine only.

## Decision

A.

## Rationale

The corpus is initially small, metadata is strongly relational and the team is one developer. One database is simpler than operating a second search service.

## Trade-offs

Dedicated vector databases may become more capable at very large vector workloads.

## Revisit

When:

- corpus size becomes large enough to degrade latency;
- hybrid search requirements outgrow Postgres;
- independent search scaling is required.

---

# ADR-009 — Retrieval Strategy

**Status:** Modified  
**Classification:** AI/Data  
**Reversibility:** Type 2

## Decision

Use **hybrid retrieval** rather than pure vector similarity.

```text
Query
 ├── exact/lexical matching
 ├── metadata filtering
 └── vector similarity
          ↓
     score fusion
          ↓
       rerank
          ↓
      top context
```

## Why

Legal queries contain exact identifiers:

- section numbers;
- Act names;
- notification numbers;
- dates;
- tax terminology.

Pure embeddings can miss exact symbolic matches.

## Alternatives

A. Pure vector.  
B. Hybrid lexical + vector.  
C. Search engine + vector platform.

## Decision Matrix

| Criterion | Weight | A | B | C |
|---|---:|---:|---:|---:|
| Legal term precision | 25% | 7 | 10 | 10 |
| Semantic recall | 25% | 10 | 10 | 10 |
| Simplicity | 20% | 10 | 8 | 5 |
| MVP cost | 15% | 10 | 10 | 6 |
| Future flexibility | 15% | 6 | 9 | 10 |
| Weighted | 100% | 8.5 | **9.4** | 7.8 |

## POC

Benchmark 50–100 representative queries against:

- vector only;
- lexical only;
- hybrid.

Metrics:

- Recall@5;
- MRR;
- citation support rate.

## Confidence

**High that hybrid is the right direction; exact weighting requires POC.**

---

# ADR-010 — RAG vs Fine-Tuning

**Status:** Accepted  
**Classification:** AI  
**Reversibility:** Type 2

## Decision

Use RAG. Do not fine-tune for V1.

## Reason

The central problem is **changing external legal knowledge**, not model style.

Fine-tuning does not provide reliable temporal updating.

## Alternatives

A. RAG.  
B. Fine-tuning.  
C. RAG + fine-tuning.

## Decision

A.

## Trade-off

RAG introduces retrieval failure modes, but those are observable and updateable.

## Revisit

Fine-tuning may become useful for:

- response style;
- classification;
- structured extraction;

after the corpus and evaluation pipeline are mature.

---

# ADR-011 — Embedding Model

**Status:** Proposed  
**Classification:** AI  
**Reversibility:** Type 2

## Decision

Do not permanently freeze the embedding model yet.

Gemini Embeddings are a strong candidate because Google documents a Gemini embedding model with API availability/free-tier access. However, the model must be evaluated on LawInc's legal corpus and language mix before adoption. citeturn239623search3

## Alternatives

A. Gemini embeddings.  
B. Self-hosted multilingual embedding model during ingestion.  
C. Another managed embedding API.

## Decision Driver Ranking

1. Retrieval quality — Critical
2. English legal terminology — Critical
3. Hindi/Hinglish robustness — High
4. Cost — High
5. Operational simplicity — High
6. Provider/data handling — High

## POC Required

Create a 100-query retrieval benchmark covering:

- exact legal citations;
- natural-language questions;
- Hinglish;
- tax terminology;
- cybercrime vocabulary.

## Decision

**Pending POC.**

## Confidence

**Medium.**

## Revisit

After benchmark results or if language requirements change.

---

# ADR-012 — Primary LLM Provider

**Status:** Proposed / POC Required  
**Classification:** AI  
**Reversibility:** Type 2

## Decision

Use a provider abstraction and postpone the final primary-provider decision until a LawInc-specific benchmark is run.

## Candidates

A. Gemini 2.5 Flash.  
B. Groq-hosted open model.  
C. Another managed model.  
D. Self-hosted model.

## Current evidence

Google currently lists Gemini 2.5 Flash with a free tier and a 1M-token context window. citeturn239623search3 Groq documents model-specific rate limits, including limits measured by RPM/RPD/TPM/TPD. citeturn418488search4

## Hard Constraints

Self-hosted inference is eliminated for this MVP because the project is cloud-hosted, $0 and two-week/solo.

## Required evaluation

Measure:

- factuality;
- groundedness;
- refusal behaviour;
- structured-output validity;
- citation selection;
- latency;
- quota headroom;
- cost;
- privacy/data-handling implications.

## Decision

**Gemini and Groq remain candidates, not yet permanently selected.**

## Confidence

**Medium.**

## Revisit

Immediately after the generation POC.

---

# ADR-013 — Multi-Provider LLM Abstraction

**Status:** Accepted  
**Classification:** AI / Reliability  
**Reversibility:** Type 2

## Decision

Define a stable internal interface:

```text
LLMProvider
 ├── generate()
 ├── stream()
 ├── structured_generate()
 └── health()
```

Implement at least two adapters.

## Why

Provider quotas, outages and pricing policies are outside LawInc's control.

## Alternatives

A. Single provider.  
B. Provider abstraction + fallback.  
C. Full model router from day one.

## Decision

B.

C is deferred because routing logic would be additional complexity without enough traffic to justify it.

## Failure

If both providers fail, return a safe retrieval-only response or service-unavailable state.

## Confidence

**High.**

---

# ADR-014 — Prompt Architecture

**Status:** Accepted  
**Classification:** AI  
**Reversibility:** Type 2

## Decision

Prompts are versioned artifacts with explicit:

- system policy;
- legal scope;
- grounding rules;
- source metadata;
- output schema;
- disclaimer policy;
- uncertainty behavior.

## Critical rule

Retrieved documents are treated as **evidence**, not executable instructions.

## Alternatives

A. One prompt string.  
B. Versioned structured prompt.  
C. Fully agentic prompt chain.

## Decision

B.

## Evidence

This is a correctness/control decision, not a provider preference.

## Confidence

**High.**

---

# ADR-015 — Citation and Grounding Validation

**Status:** Accepted  
**Classification:** AI/Security  
**Reversibility:** Type 2

## Decision

The model must reference retrieved source IDs, and the server must verify that every citation corresponds to a retrieved source chunk.

## Alternatives

A. Trust model-generated citations.  
B. Regex-check textual citations.  
C. Structured citation IDs + deterministic server validation.

## Decision

C.

## Why

A and B can validate appearance without validating provenance.

## Failure policy

- Invalid citation → one regeneration attempt.
- Second failure → no authoritative synthesized answer.

## Confidence

**High.**

---

# ADR-016 — Agents and Tool Calling

**Status:** Accepted — NOT NEEDED YET  
**Classification:** AI Architecture  
**Reversibility:** Type 2

## Decision

Do not use an agent framework in V1.

## Reason

The core workflow is deterministic:

```text
classify → retrieve → synthesize → validate
```

No autonomous planning is necessary.

## Alternatives

A. Deterministic RAG.  
B. Agent framework.  
C. Multi-agent legal system.

## Decision

A.

## Revisit

Only if future product requirements require:

- multi-step workflows;
- external transactional tools;
- document processing chains;
- human approval loops.

---

# ADR-017 — Conversation Memory

**Status:** Accepted  
**Classification:** Application/AI  
**Reversibility:** Type 2

## Decision

Use short-lived session context; do not implement persistent semantic memory.

## Why

Legal questions often need immediate conversational context but do not require a persistent user profile.

## Alternatives

A. Full persistent memory.  
B. Short-lived conversation context.  
C. No context.

## Decision

B.

## Trade-off

Less personalization in exchange for lower privacy and complexity.

## Revisit

When user accounts/saved conversations become product requirements.

---

# ADR-018 — End-User Authentication

**Status:** Accepted  
**Classification:** Security/Product  
**Reversibility:** Type 2

## Decision

No end-user account required for V1.

Use a random session identifier, ideally via secure HTTP-only cookie.

## Alternatives

A. Email/password.  
B. Social login.  
C. Anonymous session.

## Decision

C.

## Rationale

The user has not identified accounts as a requirement, and collecting identity data creates unnecessary attack/privacy surface.

## Revisit

When saved history, cross-device usage or personalization is required.

---

# ADR-019 — Admin Authentication and Authorization

**Status:** Accepted  
**Classification:** Security  
**Reversibility:** Type 2

## Decision

Protect corpus/admin operations with authenticated admin identity and server-side authorization.

## Minimum roles

```text
user
admin
```

No client-supplied role determines authorization.

## Alternatives

A. Hard-coded admin password.  
B. Managed auth/admin identity.  
C. No admin security.

## Decision

B.

## Confidence

**High.**

---

# ADR-020 — Data Retention and Privacy

**Status:** Proposed  
**Classification:** Security/Privacy/Data  
**Reversibility:** Type 3

## Decision

Minimize retention of user legal queries. Exact retention period remains an open product/privacy decision.

## Principle

Do not request or encourage submission of:

- passwords;
- OTPs;
- UPI PINs;
- card credentials;
- full bank-account numbers;
- Aadhaar/PAN unless explicitly necessary for a future validated workflow.

## Alternatives

A. Store all conversations indefinitely.  
B. Short retention.  
C. Store no user message content.

## Decision

B is preferred, but final period requires explicit product decision.

## Confidence

**Medium.**

## Revisit

Before broad public launch.

---

# ADR-021 — Corpus Ingestion Architecture

**Status:** Accepted  
**Classification:** Data/DevOps  
**Reversibility:** Type 2

## Decision

Use a reproducible ingestion pipeline executed outside the interactive request path.

```text
source
 → fetch
 → normalize
 → parse
 → classify
 → version
 → chunk
 → embed
 → validate
 → publish
```

## Alternatives

A. Runtime scraping.  
B. Manual database edits.  
C. Versioned ingestion pipeline.

## Decision

C.

## Why

Runtime scraping harms reproducibility and reliability. Manual updates do not scale.

## Required metadata

- source URL;
- authority;
- content hash;
- publication date;
- effective date;
- document type;
- domain;
- version;
- ingestion timestamp;
- supersession relation.

## Confidence

**High.**

---

# ADR-022 — Caching

**Status:** Accepted with restrictions  
**Classification:** Performance/Data  
**Reversibility:** Type 2

## Decision

Cache only when cache keys contain the relevant corpus version and prompt/model version.

```text
cache_key =
 normalized_query
 + corpus_version
 + prompt_version
 + model_version
```

## Reason

A legally correct answer can become stale after a source change.

## Alternatives

A. No caching.  
B. Permanent answer cache.  
C. Version-aware cache.

## Decision

C.

---

# ADR-023 — Asynchronous Processing

**Status:** Accepted  
**Classification:** Infrastructure  
**Reversibility:** Type 2

## Decision

Use GitHub Actions/CI jobs for V1 ingestion and evaluation work. Do not introduce a persistent message queue yet.

## Alternatives

A. Kafka.  
B. Redis queue.  
C. GitHub Actions / scheduled jobs.

## Decision

C.

## Revisit

When ingestion frequency, workload volume or user-triggered processing requires persistent workers.

---

# ADR-024 — CI/CD

**Status:** Accepted  
**Classification:** DevOps  
**Reversibility:** Type 2

## Decision

GitHub Actions + deployment-platform integration.

## Required pipeline

```text
push/PR
 → lint
 → typecheck
 → unit tests
 → build
 → dependency/security checks
 → deploy
 → smoke test
```

## Corpus CI

Separate pipeline:

```text
fetch
 → validate
 → ingest
 → retrieval evaluation
 → publish
```

Do not automatically publish a changed legal corpus unless validation passes.

---

# ADR-025 — Observability

**Status:** Accepted  
**Classification:** Operations  
**Reversibility:** Type 2

## Decision

Use platform-native telemetry plus structured application events.

## Required metrics

- request count;
- end-to-end latency;
- retrieval latency;
- LLM latency;
- provider failure rate;
- fallback rate;
- citation-validation failures;
- unsupported-query rate;
- token usage where available;
- corpus version served.

## NOT NEEDED YET

Prometheus + Grafana cluster.

## Revisit

When multiple services make platform-native telemetry insufficient.

---

# ADR-026 — Backup and Disaster Recovery

**Status:** Modified  
**Classification:** Reliability/Data  
**Reversibility:** Type 2

## Decision

For MVP:

- version all ingestion logic;
- version source manifests;
- keep migrations in Git;
- export database content/schema periodically;
- keep a reproducible corpus source.

For real production:

- require managed backups/PITR.

## Evidence

Supabase Free currently does not include automatic backups/PITR; its Pro plan includes daily backups stored for seven days, while PITR is an add-on. citeturn418488search0turn418488search1

## Confidence

**High.**

---

# ADR-027 — $0 Free-Tier Cost Strategy

**Status:** Accepted with explicit limitations  
**Classification:** Cost/Infrastructure  
**Reversibility:** Type 2

## Decision

Target $0/month for the testing MVP, but treat free-tier limits as dependencies that can change.

## Current candidates

- Vercel Hobby for personal/non-commercial testing.
- Supabase Free.
- Gemini free tier.
- Groq free tier.
- GitHub free allowances.

Supabase currently lists a $0 Free plan with 500 MB database storage, 5 GB egress and 1 GB file storage; projects pause after one week of inactivity. citeturn418488search0turn418488search1 Vercel currently lists Hobby at $0 with bounded functions and usage. citeturn239623search1turn239623search5 Google currently lists free-tier Gemini 2.5 Flash usage, while Groq documents explicit free/model rate limits. citeturn239623search3turn418488search4

## Trade-off

$0 lowers monetary cost but increases:

- quota risk;
- provider dependency;
- pausing risk;
- operational fragility.

## Confidence

**High for friends/testing; low for public production.**

---

# ADR-028 — Scalability and Migration Strategy

**Status:** Accepted  
**Classification:** Architecture  
**Reversibility:** Type 2

## Decision

Scale by measured bottleneck rather than by speculative architecture.

## 1× — <50 users

Current modular architecture.

## 10×

Optimize:

- caching;
- database connections;
- retrieval;
- rate limits;
- provider quotas.

## 100×

Consider:

- paid database;
- dedicated cache;
- dedicated worker process;
- stronger model routing.

## 1000×

Potentially split:

```text
Web
API
Retrieval service
Ingestion workers
LLM gateway
Search layer
```

## Revisit Trigger

A component is considered for extraction only when there is evidence that:

1. it needs independent scaling, or
2. it needs independent reliability, or
3. it has independent ownership, or
4. deployment coupling causes measurable delivery problems.

---

# ADR-029 — AI Evaluation as a Release Gate

**Status:** Accepted  
**Classification:** AI/Quality  
**Reversibility:** Type 2

## Decision

No model, prompt, retrieval or corpus change is considered production-ready without regression evaluation.

## Golden dataset

Minimum V1 target:

- 30 core questions;
- 10 adversarial questions;
- 10 out-of-scope questions;
- 10 time-sensitive tax/compliance questions.

## Metrics

- retrieval Recall@K;
- citation precision;
- groundedness;
- unsupported-claim rate;
- refusal accuracy;
- structured-output validity;
- p50/p95 latency.

## Decision

Evaluation becomes a release gate.

## Confidence

**High.**

---

# ADR-030 — Fail-Closed Legal Answering

**Status:** Accepted  
**Classification:** Safety/AI/Security  
**Reversibility:** Type 2

## Decision

When authoritative retrieval is insufficient, LawInc must not manufacture an authoritative-sounding answer.

## Response states

```text
SUPPORTED
SUPPORTED_WITH_CAVEAT
INSUFFICIENT_AUTHORITY
OUT_OF_SCOPE
SERVICE_UNAVAILABLE
```

## Alternatives

A. Always answer.  
B. Answer with generic disclaimer.  
C. Fail closed when evidence is inadequate.

## Decision

C.

## Why

For this product, false confidence is more dangerous than partial non-answer behavior.

## Consequence

Some legitimate questions will receive “insufficient authority” responses.

That is an intentional safety trade-off.

## Confidence

**High.**

---

# 5. Cross-Decision Matrix

The following is the consolidated architectural position after re-evaluation.

| Area | Previous choice | Re-evaluated decision | Status |
|---|---|---|---|
| Scope | Six domains | Three domains | **Accepted** |
| Source strategy | India Code-centric | Explicit multi-tier Central authority hierarchy | **Modified** |
| Source freshness | Static Act/section corpus | Temporal/versioned corpus | **Modified** |
| Architecture | Self-hosted modular stack | Cloud modular monolith | **Modified** |
| Frontend | Next.js | Next.js/TypeScript | **Accepted** |
| Backend | FastAPI | Next.js server/API | **Accepted** |
| DB | PostgreSQL | PostgreSQL | **Accepted** |
| Vector DB | Chroma/pgvector variants | pgvector | **Accepted** |
| Retrieval | Vector search | Hybrid lexical + vector + metadata | **Modified** |
| RAG | Yes | Yes | **Accepted** |
| Embeddings | BGE-M3/Gemini variants | POC required | **Open** |
| LLM | Ollama → Gemini/Groq | External provider abstraction | **Modified** |
| Primary LLM | Gemini | Candidate pending POC | **Open** |
| Fallback | Groq | Keep provider fallback | **Accepted** |
| Agents | Potentially | None | **Rejected for V1** |
| Memory | Conversation history | Short-lived session context | **Accepted** |
| User auth | Profile/user system | Anonymous sessions | **Accepted** |
| Admin auth | Not fully settled | Protected admin identity | **Accepted** |
| Cache | Basic | Version-aware | **Modified** |
| Queue | None | CI jobs | **Accepted** |
| Monitoring | Prometheus/Grafana | Platform telemetry | **Simplified** |
| Hosting | Vercel | Vercel only for personal/non-commercial MVP | **Qualified** |
| DR | Free DB backup assumptions | Reproducible corpus + exports; paid backup later | **Modified** |
| Cost | $0 | $0 target with explicit quota risk | **Accepted** |

---

# 6. MUST / SHOULD / NICE / NOT NEEDED YET

## MUST HAVE

- [x] Three-domain scope enforcement.
- [x] Central-law boundary enforcement.
- [x] Authoritative-source hierarchy.
- [x] Versioned legal corpus.
- [x] Effective-date metadata.
- [x] RAG.
- [x] Hybrid retrieval.
- [x] Citation provenance validation.
- [x] Fail-closed answer behavior.
- [x] Secure server-side API keys.
- [x] Rate limiting.
- [x] Anonymous session handling.
- [x] Golden evaluation set.
- [x] Version-controlled migrations.
- [x] Version-controlled ingestion.
- [x] Basic observability.
- [x] Provider abstraction.

## SHOULD HAVE

- [ ] Source freshness monitoring.
- [ ] Query reformulation.
- [ ] Lightweight reranker.
- [ ] Source excerpts.
- [ ] User feedback.
- [ ] Provider health scoring.
- [ ] Corpus review workflow.

## NICE TO HAVE

- [ ] Hindi/Hinglish optimization.
- [ ] Voice.
- [ ] Document upload.
- [ ] Personalized accounts.
- [ ] Automated source-diff detection.
- [ ] Expert review dashboard.
- [ ] Advanced semantic caching.

## NOT NEEDED YET

- [ ] Kubernetes.
- [ ] Microservices.
- [ ] Kafka.
- [ ] Redis cluster.
- [ ] Dedicated vector DB.
- [ ] Agent framework.
- [ ] Fine-tuning.
- [ ] Multi-region.
- [ ] Data warehouse.
- [ ] Service mesh.
- [ ] Complex feature-flag platform.

---

# 7. Open Decisions That Must Be Resolved Before Implementation

These are the decisions where the current architecture should **not** pretend certainty.

## Open Decision A — LLM

Run the benchmark and select the primary model.

## Open Decision B — Embeddings

Compare at least two candidates on the same retrieval benchmark.

## Open Decision C — Retention

Choose an explicit conversation retention period.

## Open Decision D — Commercial status

Clarify whether the deployment is personal/non-commercial testing or a public/commercial product.

This directly affects whether Vercel Hobby remains a permissible deployment option. Vercel's current terms restrict Hobby use to personal/non-commercial use. citeturn157544search3turn157544search0

## Open Decision E — Language

Confirm whether V1 is English-only or must support Hindi/Hinglish.

---

# 8. Required POCs

## POC-01 — Retrieval

### Question

Can the retrieval layer consistently return the legally relevant chunk for LawInc questions?

### Test

100–150 queries covering all three domains.

### Success target

Define:

- Recall@5;
- Recall@10;
- citation support rate.

Do not proceed to large-scale corpus ingestion until results are acceptable.

---

## POC-02 — LLM

### Question

Which candidate produces the best grounded legal explanation under a constrained prompt?

### Compare

- Gemini candidate;
- Groq candidate;
- one additional candidate if available.

### Measure

- groundedness;
- unsupported claims;
- citation correctness;
- refusal correctness;
- structured-output validity;
- latency;
- quota behavior.

---

## POC-03 — End-to-End

### Question

Can one legal question complete within the MVP latency budget?

### Measure

```text
frontend
  +
validation
  +
embedding
  +
retrieval
  +
generation
  +
validation
```

Track p50 and p95.

---

# 9. Architecture Review Triggers

The architecture must be reviewed when any of the following occurs:

### Product

- New legal domain added.
- User accounts introduced.
- Document uploads introduced.
- Paid/commercial use starts.

### Data

- Corpus exceeds current Postgres/search comfort.
- Legal-source frequency increases materially.
- Historical legal versions become a major product feature.

### AI

- Model quality is insufficient.
- Free-tier quotas become restrictive.
- Model provider changes data-handling terms.
- Retrieval failure exceeds threshold.

### Infrastructure

- Serverless timeout becomes common.
- Database connections become a bottleneck.
- Free-tier pausing becomes unacceptable.

### Security

- Sensitive personal data is introduced.
- Public usage grows significantly.
- Formal compliance becomes a requirement.

---

# 10. Architecture Confidence Summary

| Area | Confidence | Why |
|---|---|---|
| Three-domain scope | High | Explicit product decision |
| Modular monolith | High | Directly fits team/timeline |
| PostgreSQL | High | Strong fit for relational + vector metadata |
| pgvector | High for V1 | Small corpus + simplicity |
| Hybrid retrieval | High directionally | Exact legal identifiers matter |
| RAG | High | External legal knowledge changes |
| Versioned corpus | High | Essential for tax/compliance |
| Anonymous sessions | High | No current account requirement |
| Provider abstraction | High | Free-tier instability |
| Gemini primary | Medium | Needs benchmark |
| Groq fallback | Medium | Needs benchmark |
| Embeddings | Medium | Needs retrieval benchmark |
| Vercel | High for personal MVP; Medium long-term | Hobby restrictions |
| $0 architecture | High for testing; Low for public production | Free-tier dependency |
| No agents | High | No current requirement |

---

# 11. Final Re-Evaluated Architecture

The architecture produced by this ADR process is:

```text
                         ┌──────────────────────┐
                         │      User Browser    │
                         └──────────┬───────────┘
                                    │ HTTPS
                                    ▼
                         ┌──────────────────────┐
                         │ Next.js Application  │
                         │  Modular Monolith    │
                         ├──────────────────────┤
                         │ Validation           │
                         │ Rate Limiting        │
                         │ Session              │
                         │ Domain Router        │
                         │ Retrieval            │
                         │ Prompt Builder       │
                         │ Citation Validator   │
                         └───────┬─────┬────────┘
                                 │     │
                      ┌──────────▼─┐ ┌─▼──────────┐
                      │ Supabase   │ │ LLM        │
                      │ PostgreSQL │ │ Provider   │
                      │ + pgvector │ │ abstraction│
                      └──────┬─────┘ └────┬───────┘
                             │              │
                     ┌───────▼───────┐ ┌───▼────────┐
                     │ Legal Corpus  │ │ Primary    │
                     │ + Versions    │ │ candidate  │
                     │ + Metadata    │ └────┬───────┘
                     └───────────────┘      │
                                            ▼
                                      ┌────────────┐
                                      │ Fallback   │
                                      │ provider   │
                                      └────────────┘
```

---

# 12. Source Architecture

For V1, the source strategy should be organized by domain.

## Traffic

Primary sources should include Central Acts/Rules and authoritative transport/government material. India Code currently identifies the Motor Vehicles Act, 1988 as a Central Act and exposes the Central Motor Vehicles Rules alongside the Act. citeturn117330search1turn117330search6

## Income Tax / GST

The source model must include more than a single Act.

It should support:

```text
Income-tax Acts
Rules
Finance legislation
Notifications
Circulars
Forms
Official departmental guidance
Effective-date changes
```

The Income Tax Department currently publishes a dedicated Income Tax Act 2025 section, including FAQs, tax payments, returns, forms, reassessment and TDS material. citeturn117330search14

CBIC's Tax Information Portal explicitly covers Acts, Rules, Notifications, Circulars, Instructions/Guidelines and Orders, with continuously updated content. citeturn698644search0

## Cybercrime

The system should distinguish:

```text
statutory law
court/judicial authority where intentionally included
official cybercrime reporting guidance
official safety guidance
```

The National Cybercrime Reporting Portal is an official Government of India/MHA/I4C service and supports reporting of financial fraud and other cybercrime categories. citeturn117330search0turn117330search7

---

# 13. Final Decision Principle for LawInc

The governing rule is:

> **LawInc should optimize for legally defensible answers, not maximum answer coverage.**

Therefore:

```text
AUTHORITATIVE SOURCE
        +
TEMPORAL VALIDITY
        +
RELEVANT RETRIEVAL
        +
MODEL SYNTHESIS
        +
CITATION VALIDATION
        +
SAFE UNCERTAINTY
        =
LAWInc ANSWER
```

A highly fluent answer without authoritative evidence is a failure.

A narrower answer with strong evidence is preferable.

---

# 14. Review / Approval Record

## Architecture status

**Current status:** Approved as V1 architecture direction, with explicitly open AI-provider and retention decisions.

## Remaining gates before implementation freeze

- [ ] Legal source hierarchy accepted.
- [ ] Source metadata schema accepted.
- [ ] Retrieval POC completed.
- [ ] LLM POC completed.
- [ ] Embedding POC completed.
- [ ] Retention policy selected.
- [ ] Commercial/non-commercial hosting status confirmed.
- [ ] Golden evaluation set created.

## Final confidence

**Overall architecture confidence: Medium-High**

The application architecture is high-confidence for the current scale and constraints. The largest remaining uncertainties are **AI model/embedding quality, legal-source freshness workflow, privacy/retention policy, and the project's eventual commercial status**.

---

# 15. Change Log

| Version | Date | Change |
|---|---|---|
| 1.0 | 2026-08-17 | First formal LawInc ADR register and re-evaluation |
