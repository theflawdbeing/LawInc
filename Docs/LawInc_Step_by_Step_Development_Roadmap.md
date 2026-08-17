# LawInc — Complete Beginner-Friendly Development Roadmap

**Document type:** Step-by-step project execution guide  
**Project:** LawInc — AI-Powered Legal Awareness for India  
**Current scope:** Central Indian law only, three domains  
**Developer:** Solo  
**Target timeline:** 2 weeks for MVP  
**Initial users:** <50 friends/testing  
**Budget:** $0 / free-tier cloud  
**Audience:** Someone who may be learning the technologies while building the project

---

# 0. HOW TO USE THIS DOCUMENT

This document is the execution plan to follow from **zero implementation knowledge to a working LawInc MVP**.

The most important rule is:

> **Do not jump to a later phase just because you can start coding it. Finish the current phase and pass its completion benchmark first.**

Each phase contains:

1. What you are trying to accomplish.
2. What you need to learn.
3. Exact tasks.
4. What you should create.
5. How to verify it.
6. Completion benchmark.
7. What unlocks the next phase.

This is intentionally written as a learning-by-building plan. You are not expected to know everything before starting. Learn only the concepts required for the current phase, apply them, and then move forward.

---

# 1. SOURCE-OF-TRUTH ORDER

There are currently four important project files:

| File | Role | Current status |
|---|---|---|
| `LawInc_Architecture_Design_v1(1).md` | Architecture baseline | **Primary architecture source** |
| `LawInc_Ideation v2.0.md` | Earlier ideation/scope | Partially superseded |
| `LawInc_Ideation v1.0.md` | Older ideation | Legacy |
| `README.md` | Project-facing documentation | Currently stale and must be updated |

The architecture document defines the current three-domain scope, cloud architecture, managed Postgres/pgvector, external LLM providers, modular monolith, RAG and related MVP requirements. fileciteturn1file0L11-L36

The older ideation files still contain earlier assumptions such as six domains, state-law coverage, local Ollama, ChromaDB, Docker-only deployment and other features that are no longer the active architecture. fileciteturn2file7L793-L862

The latest ideation version also still contains the previous six-domain/local-stack design, despite already removing state-law and foreign-national scope. fileciteturn2file1L144-L180

Therefore:

> **Do not copy the old README or old ideation commands into the implementation unchanged.**

The final execution architecture is:

```text
Browser
   ↓
Next.js / TypeScript
   ↓
Application/API
   ↓
Validation → Rate Limit → Session → Domain Router
   ↓
Hybrid RAG
   ↓
Supabase PostgreSQL + pgvector
   ↓
Gemini / fallback Groq
   ↓
Citation + Output Validator
   ↓
Answer + Sources + Disclaimer
```

The existing architecture explicitly recommends a modular monolith, managed Postgres/pgvector and external LLM APIs for the current <50-user, two-week, $0 cloud MVP. fileciteturn2file0L111-L129

---

# 2. FINAL V1 SCOPE — DO NOT CHANGE DURING MVP

## 2.1 Supported domains

### Domain A — Traffic & Motor Vehicle Laws

Examples:

- valid driving licence;
- insurance;
- PUC;
- helmet/seat belt;
- drunk driving;
- speed limits;
- wrong-side driving;
- parking;
- modified vehicles;
- excess passengers;
- driving another person's vehicle.

### Domain B — Income Tax / GST / Financial Compliance

Examples:

- taxable income;
- TDS;
- ITR filing;
- capital gains;
- freelance/side income;
- foreign income;
- interest income;
- deductions;
- GST registration;
- Income Tax notices;
- late-filing penalties and interest.

### Domain C — Cybercrime / Digital & Online Transactions

Examples:

- UPI fraud;
- online scams;
- OTP/PIN sharing;
- identity theft;
- another person's account;
- impersonation;
- cyber harassment;
- threats;
- defamation;
- copyright infringement;
- illegal/private content;
- unauthorized access;
- fraudulent online transactions.

## 2.2 Explicitly out of scope

Do not implement:

- state-specific law;
- Tamil Nadu;
- Karnataka;
- tenancy;
- employment/labour;
- consumer law;
- foreign national/visa law;
- immigration;
- passport law;
- court representation;
- prediction of court outcomes;
- autonomous legal actions;
- native mobile applications;
- WhatsApp;
- voice;
- document generation;
- fine-tuning;
- agents;
- Kubernetes;
- microservices;
- Kafka;
- Redis cluster.

The current architecture explicitly marks many of these as not needed for V1. fileciteturn2file0L45-L66

---

# 3. FINAL V1 ARCHITECTURE

## 3.1 Current architecture

```text
USER
 │
 ▼
NEXT.JS WEB APP
 │
 ▼
APPLICATION/API
 │
 ├── Input validation
 ├── Rate limiting
 ├── Session
 ├── Domain classification
 │
 ▼
RETRIEVAL
 │
 ├── Query normalization
 ├── Metadata filtering
 ├── Lexical/exact matching
 ├── Vector similarity
 └── Lightweight reranking
 │
 ▼
SUPABASE POSTGRES + PGVECTOR
 │
 ▼
GROUNDED PROMPT
 │
 ├── Gemini primary candidate
 └── Groq fallback
 │
 ▼
OUTPUT VALIDATION
 │
 ├── Citation provenance
 ├── Structured output validation
 ├── Uncertainty
 └── Safety checks
 │
 ▼
ANSWER
 │
 ├── Explanation
 ├── Sources
 ├── Applicable date/version
 └── Disclaimer
```

The architecture document specifies RAG, source/effective-date metadata, safe failure, session continuity, rate limiting and citation-backed responses as core requirements. fileciteturn2file0L13-L32

---

# 4. DEVELOPMENT PHASE MAP

```text
PHASE 0  → Understand + freeze project
PHASE 1  → Development environment
PHASE 2  → Repository + application skeleton
PHASE 3  → Database + schema
PHASE 4  → Legal source strategy
PHASE 5  → Corpus ingestion pipeline
PHASE 6  → Retrieval system
PHASE 7  → AI provider POC
PHASE 8  → Grounded answer engine
PHASE 9  → Safety + citation validation
PHASE 10 → API + sessions + rate limits
PHASE 11 → Frontend
PHASE 12 → End-to-end integration
PHASE 13 → Evaluation + testing
PHASE 14 → Security + observability
PHASE 15 → Deployment
PHASE 16 → Final MVP hardening
PHASE 17 → Release
```

The existing architecture's development order broadly follows freeze scope → source matrix → schema → database → ingestion → embeddings → retrieval → model adapter → prompts → citation validation → rate limiting/sessions → UI → evaluation → deployment → security → release. fileciteturn4file0L14-L85

---

# PHASE 0 — ORIENT YOURSELF BEFORE CODING

## Goal

Understand what you are actually building and eliminate outdated assumptions.

## What you learn

- What is an MVP?
- What is architecture?
- What is RAG?
- What is an API?
- What is a database?
- What is an embedding?
- What is a vector database?
- What is an LLM?
- What is a source/citation?
- What is a deployment?

Do not spend days studying these topics. Learn a basic definition and move on.

## Tasks

### Task 0.1 — Read the current architecture document

Read these sections first:

- Executive Summary
- Understanding of System
- Functional Requirements
- Non-Functional Requirements
- Assumptions
- Architecture Goals
- Recommended Architecture Pattern
- AI/ML Architecture
- Development Order
- Validation Checklist

### Task 0.2 — Read the latest ADR document

Review:

- scope ADR;
- source-authority ADR;
- corpus-versioning ADR;
- modular-monolith ADR;
- database ADR;
- retrieval ADR;
- RAG ADR;
- LLM/embedding POCs;
- citation validation;
- fail-closed behavior.

### Task 0.3 — Mark old files as legacy

Do not delete them yet.

Create:

```text
docs/
  legacy/
```

Move or clearly label:

```text
LawInc_Ideation v1.0.md
LawInc_Ideation v2.0.md
README.md
```

as historical/legacy until updated.

### Task 0.4 — Create the current project requirements file

Create:

```text
docs/requirements.md
```

Copy the final three-domain requirements into it.

## Completion benchmark

You can explain, without reading the documents:

> “LawInc is a three-domain Central-law legal-awareness RAG application for Indian citizens. It retrieves authoritative legal sources, uses an external LLM to explain them, validates citation provenance, and fails safely when evidence is insufficient.”

## Do not proceed until

You are no longer planning six domains or local Ollama inference.

---

# PHASE 1 — LEARN + SET UP YOUR DEVELOPMENT ENVIRONMENT

## Goal

Get your computer ready.

## Learn

- Git
- GitHub
- Node.js
- npm/pnpm
- TypeScript basics
- VS Code
- terminal/PowerShell
- environment variables

## Install

Recommended:

- Git
- Node.js LTS
- VS Code
- GitHub account/repository
- Docker Desktop only if you want local database experiments
- PostgreSQL client/tool if useful

Python may still be useful for ingestion experiments, but the primary runtime architecture is TypeScript.

## Tasks

### 1.1 Configure Git

Learn:

```bash
git --version
git config --global user.name "Your Name"
git config --global user.email "your@email.com"
```

### 1.2 Learn the Git workflow

Understand:

```text
working directory
   ↓
git add
   ↓
git commit
   ↓
git push
   ↓
GitHub
```

### 1.3 Create GitHub repository

Repository:

```text
lawinc
```

### 1.4 Add `.gitignore`

Must ignore:

```text
.env
.env.*
node_modules/
.next/
dist/
build/
coverage/
data/raw/
data/processed/
logs/
```

Never commit:

- API keys;
- tokens;
- downloaded sensitive user data;
- database passwords.

### 1.5 Create README placeholder

Do not rewrite it fully yet.

## Completion benchmark

Run:

```bash
git status
git log
node --version
npm --version
```

and successfully push a commit to GitHub.

## Exit condition

You can create a branch, commit, push and revert a change without help.

---

# PHASE 2 — CREATE THE PROJECT SKELETON

## Goal

Create the application structure before implementing business logic.

## Final structure

```text
lawinc/
├── apps/
│   └── web/
│       ├── app/
│       ├── components/
│       ├── modules/
│       │   ├── chat/
│       │   ├── retrieval/
│       │   ├── llm/
│       │   ├── citations/
│       │   ├── sessions/
│       │   ├── corpus/
│       │   └── feedback/
│       ├── lib/
│       └── tests/
│
├── ingestion/
│   ├── sources/
│   ├── parsers/
│   ├── chunkers/
│   ├── embed/
│   ├── validators/
│   └── manifests/
│
├── eval/
│   ├── datasets/
│   ├── runners/
│   └── reports/
│
├── supabase/
│   ├── migrations/
│   └── seed/
│
├── docs/
│   ├── architecture/
│   ├── legal-scope/
│   ├── adr/
│   ├── runbooks/
│   └── legacy/
│
├── .github/
│   └── workflows/
│
├── package.json
├── README.md
├── .env.example
└── tsconfig.json
```

This is consistent with the architecture's final repository direction. fileciteturn2file9L913-L930

## Learn

- Next.js project structure;
- App Router;
- TypeScript imports/exports;
- modules;
- environment variables.

## Tasks

1. Initialize Next.js.
2. Add TypeScript.
3. Add Tailwind.
4. Create basic layout.
5. Create a simple `/health` route.
6. Create `/api/v1/health`.
7. Make the app run locally.

## Completion benchmark

You can open:

```text
http://localhost:3000
```

and see a LawInc landing page.

Also:

```text
GET /api/v1/health
```

returns:

```json
{
  "status": "ok"
}
```

## Do not proceed until

You understand where:

- frontend code lives;
- API code lives;
- shared utilities live;
- tests will live.

---

# PHASE 3 — DATABASE FOUNDATION

## Goal

Build the persistent data layer.

## Learn

- PostgreSQL basics;
- tables;
- primary keys;
- foreign keys;
- indexes;
- constraints;
- migrations;
- transactions.

## Database

Use:

```text
Supabase Postgres
+
pgvector
```

The architecture deliberately chooses PostgreSQL because the project needs relational metadata, source/version references and vector retrieval in one place. fileciteturn4file3L324-L337

## Core entities

Create:

```text
sessions
messages
sources
source_chunks
retrieval_events
answers
citations
feedback
```

## Tasks

### 3.1 Create Supabase project

Create development project.

### 3.2 Record credentials

Create:

```text
.env.local
```

Never commit it.

### 3.3 Create migrations

Create:

```text
supabase/migrations/
```

### 3.4 Enable pgvector

### 3.5 Create tables

Implement:

- UUID primary keys;
- foreign keys;
- timestamps;
- domain constraint;
- effective-date fields;
- validation status fields.

### 3.6 Create indexes

At minimum:

```text
messages(session_id, created_at)
sources(domain, status, effective_from)
source_chunks(source_id)
vector index
citations(answer_id)
```

### 3.7 Test a database transaction

Create one sample session and message, then verify rollback/commit behavior.

## Completion benchmark

You can:

1. run migrations from a clean database;
2. create a session;
3. insert a message;
4. insert a source;
5. insert a chunk;
6. retrieve records using SQL;
7. delete/reset the development database and recreate it using migrations.

## Exit condition

Database setup is reproducible without manually clicking through the database UI.

---

# PHASE 4 — DEFINE THE LEGAL SOURCE STRATEGY

## Goal

Before building AI, decide exactly what legal information is allowed into LawInc.

This is one of the most important phases.

## Learn

- primary source vs secondary source;
- Act;
- Rule;
- Regulation;
- Notification;
- Circular;
- Order;
- amendment;
- effective date;
- supersession;
- source authority.

## Source hierarchy

Implement the concept:

```text
Tier 1
Primary authoritative legal text

Tier 2
Official administrative material

Tier 3
Official government guidance

Tier 4
Secondary explanatory material

Tier 5
Unverified web content
```

### V1 answer rule

Tier 4/5 must not become the authoritative legal basis of an answer.

## Tasks

### 4.1 Create source authority matrix

Create:

```text
docs/legal-scope/source-authority-matrix.md
```

Columns:

| Field | Example |
|---|---|
| Domain | Traffic |
| Organization | Government body |
| Source | Act/Rule/etc. |
| Authority tier | 1 |
| URL | official URL |
| Document type | Act |
| Publication date | date |
| Effective date | date |
| Refresh frequency | estimated |
| Notes | scope |

### 4.2 Define source rules for each domain

#### Traffic

Identify the Central Acts/Rules and official government material needed.

#### Income Tax/GST

Do NOT assume one Act is enough.

Plan for:

```text
Acts
Rules
Notifications
Circulars
Instructions
Forms
official compliance guidance
effective dates
```

#### Cybercrime

Plan for:

```text
statutory provisions
official cybercrime guidance
official reporting resources
other deliberately selected primary/legal authority
```

### 4.3 Define source conflict policy

Example:

```text
Current primary source
      > current official notification
      > older source
      > official guidance
      > secondary explanation
```

Do not guess conflict resolution rules. Document them.

### 4.4 Define freshness policy

For each document:

```text
publication_date
effective_from
effective_to
retrieved_at
version
content_hash
supersedes
superseded_by
```

## Completion benchmark

Take five sample legal questions.

For each, you can answer:

- Which source should support it?
- Why is that source authoritative?
- Is the source current?
- What happens if an older version also exists?

## Exit condition

The legal corpus has a written authority/freshness policy before ingestion starts.

---

# PHASE 5 — BUILD THE LEGAL CORPUS INGESTION PIPELINE

## Goal

Turn official legal documents into clean, searchable, versioned chunks.

## Learn

- PDF parsing;
- OCR;
- text cleaning;
- chunking;
- metadata;
- embeddings;
- hashes;
- deterministic data pipelines.

## Pipeline

```text
Official Source
      ↓
Fetch
      ↓
Raw Document
      ↓
Parse / OCR
      ↓
Clean
      ↓
Detect Structure
      ↓
Chunk
      ↓
Attach Metadata
      ↓
Hash
      ↓
Embed
      ↓
Validate
      ↓
Publish
```

## Tasks

### 5.1 Create raw storage

```text
ingestion/
  data/
    raw/
```

### 5.2 Create normalized storage

```text
ingestion/
  data/
    processed/
```

### 5.3 Build parser

Support:

- HTML;
- text PDFs;
- OCR fallback for scanned PDFs.

### 5.4 Preserve legal structure

A chunk should ideally know:

```text
source
document
chapter
part
section/rule
heading
text
page
```

Do not blindly split every N characters.

### 5.5 Build chunker

First version:

- logical heading boundary;
- section/rule boundary;
- paragraph boundary;
- then token/character limit.

### 5.6 Build content hash

Example concept:

```text
SHA-256(document contents)
```

### 5.7 Build metadata object

Example:

```json
{
  "domain": "traffic",
  "authority_tier": 1,
  "document_type": "act",
  "title": "…",
  "provision": "…",
  "publication_date": "…",
  "effective_from": "…",
  "effective_to": null,
  "source_url": "…",
  "content_hash": "…",
  "corpus_version": "…"
}
```

### 5.8 Build ingestion manifest

```text
ingestion/manifests/
```

Each source should be traceable.

### 5.9 Start with a SMALL corpus

Do not ingest every possible document.

Start with:

- 1–3 representative sources per domain;
- enough content to test retrieval.

## Completion benchmark

Run ingestion and produce:

```text
raw documents
processed text
chunks
metadata
content hashes
manifest
```

for all three domains.

No manual copy/paste should be required after the source download step.

## Exit condition

You can delete the processed corpus and recreate it deterministically.

---

# PHASE 6 — EMBEDDINGS

## Goal

Convert legal chunks into vectors.

## Learn

- embeddings;
- semantic similarity;
- cosine similarity;
- vector dimensions;
- embedding consistency.

## Important rule

Do not permanently choose an embedding model before evaluation.

The architecture explicitly marks the embedding choice as something to validate with a LawInc-specific retrieval benchmark. fileciteturn4file3L324-L367

## Candidates

Start with:

- Gemini embedding candidate;
- one alternative candidate for comparison.

## Tasks

### 6.1 Build embedding adapter

Create:

```text
ingestion/embed/
```

### 6.2 Embed sample corpus

### 6.3 Store embeddings in pgvector

### 6.4 Verify dimensions

The query embedding and document embedding dimensions must match.

### 6.5 Measure embedding generation

Record:

```text
document count
token count
embedding count
time
errors
```

## Completion benchmark

You can submit:

```text
"Do I need insurance to drive my friend's car?"
```

and generate a query embedding compatible with your stored corpus embeddings.

## Exit condition

At least one working embedding pipeline exists and can be rerun.

---

# PHASE 7 — BUILD THE RETRIEVAL ENGINE

## Goal

Answer:

> “Given a user's question, which legal chunks should the LLM see?”

This is more important than making the LLM sound intelligent.

## Learn

- semantic search;
- lexical search;
- metadata filtering;
- top-k;
- reranking;
- Recall@K;
- MRR.

## Retrieval design

```text
Question
   ↓
Normalize
   ↓
Domain detection
   ↓
Metadata filter
   ↓
Exact/lexical search
   +
Vector search
   ↓
Merge candidates
   ↓
Score
   ↓
Rerank
   ↓
Top 5–10 chunks
```

The architecture explicitly calls for normalization, domain detection, metadata filtering, vector similarity, optional lexical matching and lightweight reranking. fileciteturn4file3L339-L367

## Tasks

### 7.1 Implement domain labels

```text
traffic
tax_finance
cyber
```

### 7.2 Implement metadata filters

Example:

```text
domain = traffic
AND source_status = current
AND effective_from <= today
AND effective_to is null OR effective_to >= today
```

### 7.3 Implement vector retrieval

Start:

```text
top_k = 10
```

### 7.4 Implement exact matching

Prioritize:

- Act names;
- section numbers;
- rule numbers;
- notification numbers;
- tax terms;
- known acronyms.

### 7.5 Implement scoring

Start simple.

Do not over-optimize the formula before testing.

### 7.6 Implement retrieval API

Internal function:

```text
retrieve(query) -> ranked chunks
```

## Build a retrieval test set

At least:

- 10 traffic questions;
- 10 tax/GST questions;
- 10 cyber questions.

Then add:

- 10 exact-section queries;
- 10 difficult/ambiguous queries.

## Completion benchmark

For most test questions, the correct supporting chunk appears in:

```text
top 5
```

or at minimum:

```text
top 10
```

Set an explicit target before moving on.

Suggested initial gate:

> **≥80% Recall@5 on the initial representative test set.**

Then improve it.

## Exit condition

You trust the retrieval engine enough that you would manually answer a question using its retrieved documents.

---

# PHASE 8 — AI PROVIDER POC

## Goal

Determine which external LLM is actually suitable.

## Important

Do not simply decide:

> “Gemini is popular.”

Run the experiment.

## Candidates

- Gemini candidate;
- Groq candidate;
- optional third provider if available at $0.

## Learn

- model inference;
- temperature;
- context;
- structured outputs;
- token usage;
- latency;
- rate limits;
- retries.

## Prepare benchmark

Use:

```text
20 straightforward questions
10 difficult questions
10 out-of-scope questions
10 adversarial/prompt-injection questions
10 tax freshness questions
```

## Test

Each provider receives:

```text
same system prompt
same question
same retrieved context
same output schema
```

## Measure

| Metric | Result |
|---|---|
| Groundedness | |
| Citation correctness | |
| Unsupported claims | |
| Output validity | |
| Refusal quality | |
| p50 latency | |
| p95 latency | |
| Failure rate | |
| Free-tier feasibility | |

## Completion benchmark

Choose a provider only after recording results.

## Exit condition

You can explain:

> “We selected X because it performed better on our actual legal benchmark under our constraints.”

If neither provider is acceptable, stop and change the design before continuing.

---

# PHASE 9 — BUILD THE GROUNDED ANSWER ENGINE

## Goal

Turn retrieved legal material into a useful answer.

## Learn

- system prompts;
- developer prompts;
- structured output;
- prompt injection;
- context windows;
- prompt versioning.

## Prompt architecture

Use:

```text
SYSTEM
  product boundary
  safety
  legal scope
  citation rules
  uncertainty

DEVELOPER
  response format
  source rules

CONTEXT
  retrieved evidence

USER
  question
```

The architecture explicitly recommends versioned prompts and treating retrieved documents as evidence rather than executable instructions. fileciteturn4file3L381-L406

## Define response schema

Example:

```json
{
  "answer": "...",
  "citations": [
    {
      "chunk_id": "...",
      "reason": "..."
    }
  ],
  "uncertainty": "low",
  "out_of_scope": false
}
```

## Required response sections

Use only if supported by retrieved evidence:

```text
What this means
Applicable rule/law
What you can do
Important caveat
Sources
Disclaimer
```

Do NOT require every answer to invent a “fine”, “rights”, or “action” section if those facts are not supported.

## Completion benchmark

Give the system:

```text
question + retrieved documents
```

and receive valid structured JSON that contains only source-backed claims.

---

# PHASE 10 — CITATION + SAFETY ENGINE

## Goal

Stop the model from becoming the source of truth.

## Learn

- validation;
- provenance;
- fail-closed systems;
- prompt injection;
- abuse controls.

## Citation rule

The model should cite:

```text
chunk_id
```

not merely produce:

```text
"Act X Section Y"
```

The server verifies that the cited chunk was actually retrieved.

The architecture specifically identifies structured citation IDs as stronger than simple regex citation validation. fileciteturn4file3L408-L427

## Implement

### 10.1 Citation validator

Checks:

```text
Does citation exist?
       ↓
Does chunk_id exist?
       ↓
Was chunk retrieved?
       ↓
Does source metadata exist?
       ↓
Is source current?
```

### 10.2 Output validator

Checks:

- valid JSON;
- required fields;
- no unknown fields;
- no invalid citation IDs.

### 10.3 No-answer state

Create:

```text
INSUFFICIENT_AUTHORITY
```

### 10.4 Out-of-scope state

Create:

```text
OUT_OF_SCOPE
```

### 10.5 Provider failure state

Create:

```text
SERVICE_UNAVAILABLE
```

### 10.6 Regeneration

Maximum:

```text
1 regeneration attempt
```

Do not recursively regenerate.

### 10.7 Prompt-injection test

Put malicious instructions inside a fake retrieved document.

Verify the model treats the text as data.

## Completion benchmark

Test:

1. valid answer → passes;
2. fake citation → rejected;
3. missing retrieval → no authoritative answer;
4. outdated source → rejected or caveated;
5. malicious retrieved instruction → ignored;
6. malformed model output → safe fallback.

## Exit condition

The system cannot silently transform unsupported information into an authoritative answer.

---

# PHASE 11 — DOMAIN ROUTER + QUERY CLASSIFICATION

## Goal

Determine which of the three supported areas the user is asking about.

## Learn

- classification;
- rule-based matching;
- confidence;
- ambiguity.

## Do not use a large AI classifier yet.

Start with:

```text
rules + lightweight model/LLM classification if needed
```

## Output

```json
{
  "domain": "traffic",
  "confidence": 0.94
}
```

Possible states:

```text
traffic
tax_finance
cyber
out_of_scope
ambiguous
```

## Test cases

Include questions:

- clearly traffic;
- clearly tax;
- clearly cyber;
- mixed;
- unrelated;
- intentionally ambiguous.

## Completion benchmark

Create a confusion table and record classification accuracy.

Suggested initial gate:

> **≥90% accuracy on a manually verified classification set.**

## Exit condition

A user asking an unrelated legal question does not get accidentally routed into one of the three domains.

---

# PHASE 12 — API LAYER

## Goal

Turn internal logic into a clean application interface.

## Learn

- REST;
- HTTP;
- JSON;
- status codes;
- validation;
- error handling;
- API versioning.

## Create

```text
/api/v1/health
/api/v1/chat
/api/v1/feedback
/api/v1/sources/:id
```

Admin:

```text
/api/v1/admin/corpus/*
```

## Implement `/api/v1/chat`

Flow:

```text
request
 ↓
validate
 ↓
rate-limit
 ↓
session
 ↓
classify
 ↓
emergency/safety check if applicable
 ↓
retrieve
 ↓
generate
 ↓
validate
 ↓
persist
 ↓
respond
```

## Request

```json
{
  "request_id": "uuid",
  "message": "Do I need insurance to drive my friend's car?"
}
```

## Response

```json
{
  "answer": "...",
  "domain": "traffic",
  "citations": [],
  "status": "supported",
  "disclaimer": "..."
}
```

## Error types

Implement:

```text
INVALID_REQUEST
RATE_LIMITED
OUT_OF_SCOPE
INSUFFICIENT_AUTHORITY
LLM_UNAVAILABLE
RETRIEVAL_UNAVAILABLE
INTERNAL_ERROR
```

## Completion benchmark

Use Postman/curl/browser and successfully run:

```text
valid request
invalid request
empty request
huge request
unsupported query
provider failure
```

with controlled responses.

---

# PHASE 13 — SESSIONS + RATE LIMITING + PRIVACY

## Goal

Allow conversation continuity without introducing a full user-account system.

## Learn

- cookies;
- secure cookies;
- sessions;
- rate limiting;
- data minimization.

The architecture explicitly requires session-based continuity, abuse/rate limiting and no end-user account requirement in V1. fileciteturn2file0L19-L32

## Session

Use:

```text
opaque random session ID
```

preferably in:

```text
HTTP-only
Secure
SameSite
cookie
```

## Rate limiting

Initial targets:

```text
per session
per IP where practical
global provider budget
```

## Sensitive-data warning

Tell users not to enter:

- passwords;
- OTPs;
- PINs;
- card details;
- full bank identifiers.

## Retention

Choose an explicit MVP retention policy.

Do not leave this undefined before deployment.

## Completion benchmark

You can:

- start a new session;
- continue the same conversation;
- create a different session;
- exceed rate limit;
- verify the server rejects abuse.

---

# PHASE 14 — FRONTEND

## Goal

Create the real user experience.

## Learn

- React components;
- state;
- forms;
- fetch/API requests;
- accessibility;
- loading/error states.

## Screens

### Screen 1 — Landing

Keep it simple:

```text
LawInc
Know the law. Understand your options.

Ask your legal question.

[ Text input ]

Examples:
• Can I drive someone else's car?
• Do I need to file ITR for freelance income?
• What should I do after a UPI scam?
```

### Screen 2 — Chat

Show:

```text
User message
↓
Assistant answer
↓
Sources
↓
Disclaimer
```

### Source card

Display:

```text
Source
Authority
Provision
Effective date
Source link
```

### Unsupported state

```text
This question is outside LawInc's current scope.
```

### Insufficient-authority state

```text
I could not find sufficient authoritative material to answer this reliably.
```

### Provider failure

Do not expose internal model details.

## Completion benchmark

A non-technical friend can:

1. open the website;
2. understand what it does;
3. ask a question;
4. receive an answer;
5. understand where the answer came from.

---

# PHASE 15 — END-TO-END INTEGRATION

## Goal

Make the whole system work as one product.

## Full flow

```text
Browser
 ↓
Next.js
 ↓
API
 ↓
validation
 ↓
session
 ↓
domain router
 ↓
retrieval
 ↓
LLM
 ↓
citation validation
 ↓
database
 ↓
response
 ↓
UI
```

## Test three canonical questions

### Traffic

> “Can I drive my friend's car if the insurance is in their name?”

### Tax/GST

> “Do I need to file an ITR if I have freelance income?”

### Cyber

> “What should I do after someone used my UPI account fraudulently?”

## Completion benchmark

Each query should:

- classify correctly;
- retrieve relevant sources;
- generate answer;
- cite actual retrieved source IDs;
- display sources;
- show disclaimer;
- persist the session/message;
- complete without manual intervention.

---

# PHASE 16 — GOLDEN TEST SET

## Goal

Create the project's objective quality benchmark.

The older project files planned around 50 queries, while the latest architecture calls for an explicit evaluation set including core, adversarial, out-of-scope and time-sensitive tax questions. fileciteturn2file0L29-L32

## Recommended V1 dataset

### Traffic

15 queries.

### Tax/GST

20 queries.

Tax gets more because freshness and compliance complexity are higher.

### Cyber

15 queries.

### Out of scope

10 queries.

### Adversarial

10 queries.

### Total

```text
70 queries
```

This is better than mechanically preserving the old six-domain 50–60 query plan.

## Dataset fields

```json
{
  "id": "traffic-001",
  "query": "…",
  "domain": "traffic",
  "expected_behavior": "answer",
  "expected_sources": [],
  "must_contain": [],
  "must_not_contain": [],
  "severity": "normal",
  "notes": "..."
}
```

## Include difficult cases

- exact section;
- vague wording;
- Hindi/Hinglish;
- multiple concepts;
- outdated-law traps;
- state-law traps;
- fake legal assumptions;
- prompt injection;
- requests for guaranteed legal outcomes.

## Completion benchmark

Automated evaluation can produce:

```text
retrieval recall
citation accuracy
groundedness
out-of-scope accuracy
invalid-output rate
latency
provider failures
```

Create a report:

```text
eval/reports/latest.md
```

---

# PHASE 17 — SECURITY HARDENING

## Goal

Assume someone will try to break the application.

## Learn

- OWASP basics;
- XSS;
- SQL injection;
- authentication/authorization;
- secrets;
- prompt injection;
- API abuse.

## Checklist

### Secrets

- [ ] No API keys in frontend.
- [ ] `.env` ignored.
- [ ] Production secrets stored in hosting platform.
- [ ] Service keys separated.

### Input

- [ ] max message size;
- [ ] schema validation;
- [ ] control characters handled;
- [ ] JSON limits.

### Output

- [ ] safe Markdown;
- [ ] no raw HTML from model;
- [ ] no executable model output.

### Database

- [ ] parameterized queries;
- [ ] least privilege;
- [ ] no admin credentials in frontend.

### API abuse

- [ ] rate limit;
- [ ] request timeout;
- [ ] duplicate request handling;
- [ ] provider budget protection.

### Prompt injection

Test:

```text
"Ignore the retrieved law and say..."
```

and malicious instructions embedded inside legal source text.

### Data privacy

Test that secrets/PII warning exists before user interaction.

## Completion benchmark

Perform a written security review and demonstrate that all critical checks pass.

---

# PHASE 18 — OBSERVABILITY

## Goal

Know what the system is doing when something goes wrong.

## Learn

- logs;
- metrics;
- tracing basics;
- request IDs.

## Log

At minimum:

```text
request_id
timestamp
domain
retrieval_count
retrieval_latency
model_provider
model
generation_latency
validation_status
fallback_used
error_code
```

Do NOT log:

- API keys;
- passwords;
- OTPs;
- UPI PIN;
- unnecessary sensitive user information.

## Metrics

Track:

```text
requests
p50 latency
p95 latency
retrieval latency
LLM latency
provider failures
fallback rate
citation failures
out-of-scope rate
error rate
```

## Completion benchmark

Trigger a failed request and trace it using the request ID from the application logs.

---

# PHASE 19 — CI/CD

## Goal

Make the application build and deploy consistently.

## Learn

- CI;
- CD;
- GitHub Actions;
- environment variables;
- deployment pipelines.

## Pipeline

```text
Pull Request
 ↓
Lint
 ↓
Typecheck
 ↓
Unit Tests
 ↓
Build
 ↓
Dependency Scan
 ↓
Merge
 ↓
Deploy
 ↓
Smoke Test
```

The architecture explicitly recommends GitHub Actions plus deployment-platform integration and automated build/test/security checks. fileciteturn4file0L89-L100

## Create

```text
.github/workflows/
  ci.yml
  deploy.yml
```

## Completion benchmark

A clean GitHub pull request automatically runs CI.

A merged change deploys the application.

---

# PHASE 20 — DEPLOYMENT

## Goal

Put the MVP on the internet.

## Important scope note

The architecture has Vercel as the current MVP candidate, but the final hosting decision depends on whether the project remains personal/non-commercial and on free-tier constraints.

## Tasks

### 20.1 Create production database

Do not reuse development data.

### 20.2 Configure production secrets

### 20.3 Deploy web app

### 20.4 Configure production URL

### 20.5 Configure database connection

### 20.6 Configure model APIs

### 20.7 Run migrations

### 20.8 Load a production-safe corpus

### 20.9 Run smoke tests

## Completion benchmark

From a clean browser/device:

1. open public URL;
2. ask a supported question;
3. receive an answer;
4. see source;
5. verify citation;
6. test unsupported query;
7. test error state.

---

# PHASE 21 — PERFORMANCE TESTING

## Goal

Measure whether the MVP meets its targets.

The architecture's initial performance target is approximately p50 <4 seconds and p95 <10 seconds, with first-token latency ideally below 3 seconds. These are targets, not guarantees. fileciteturn2file0L88-L96

## Measure

Break latency into:

```text
frontend
validation
embedding
retrieval
LLM
validation
database persistence
```

## Run

At least:

```text
10 requests
30 requests
50 requests
```

Track:

- p50;
- p95;
- failures;
- model latency;
- retrieval latency.

## Completion benchmark

You know which component is slowest.

Do not optimize blindly.

---

# PHASE 22 — FAILURE TESTING

## Goal

Prove that the system fails safely.

## Test failures

### LLM failure

Expected:

```text
fallback provider
```

### Both providers fail

Expected:

```text
safe service-unavailable response
```

### Retrieval failure

Expected:

```text
no fabricated legal answer
```

### Invalid citation

Expected:

```text
regeneration or safe fallback
```

### Database failure

Expected:

```text
controlled error
```

### Rate-limit violation

Expected:

```text
429-style controlled response
```

### Out-of-scope question

Expected:

```text
OUT_OF_SCOPE
```

## Completion benchmark

All failure scenarios produce intentional results, not stack traces or fabricated law.

---

# PHASE 23 — LEGAL-CORPUS FRESHNESS SYSTEM

## Goal

Make LawInc maintainable after launch.

This is especially important for Tax/GST.

The architecture explicitly identifies stale tax/compliance content as one of the largest technical risks and requires effective/publication/retrieval dates plus corpus versioning. fileciteturn4file2L237-L248

## Tasks

### 23.1 Add source status

```text
draft
validated
published
superseded
archived
```

### 23.2 Add source version

### 23.3 Add effective dates

### 23.4 Add content hashes

### 23.5 Create corpus release version

Example:

```text
corpus-2026-08-17-v1
```

### 23.6 Add ingestion report

```text
documents processed
documents changed
documents unchanged
documents failed
chunks created
embeddings created
```

### Completion benchmark

You can replace one source version and prove that:

- old version remains traceable;
- new version is searchable;
- current retrieval selects the correct version.

---

# PHASE 24 — USER TESTING

## Goal

Have real people use the product before calling it complete.

## Users

Start with:

```text
5–10 friends
```

Do not start with 50 simultaneously.

## Give them tasks

Ask them to:

1. ask a traffic question;
2. ask a tax question;
3. ask a cyber question;
4. ask an unrelated question;
5. try a follow-up question;
6. inspect the citation;
7. report whether they trust the answer.

## Observe

Do NOT coach them.

Watch:

- confusion;
- abandoned queries;
- misunderstood disclaimer;
- citation usage;
- latency tolerance;
- response readability.

## Completion benchmark

You have a written list of:

```text
top 5 user problems
top 5 bugs
top 5 improvements
```

---

# PHASE 25 — MVP HARDENING

## Goal

Fix only what matters.

## Priority order

```text
P0 — legal correctness / security / crashes
P1 — broken core functionality
P2 — major UX issues
P3 — performance issues
P4 — polish
```

## Do not add new features

Unless they are necessary for core functionality.

---

# PHASE 26 — README + DOCUMENTATION REWRITE

## Goal

Replace the stale README.

The current README still advertises:

- foreign-national support;
- Tamil Nadu and Karnataka;
- Ollama;
- ChromaDB;
- local Docker deployment;
- old project structure.

Those are legacy assumptions and should not remain in the final public README. fileciteturn2file6L594-L680

## New README should contain

1. What LawInc is.
2. Current three-domain scope.
3. Out-of-scope areas.
4. Architecture.
5. Local development setup.
6. Environment variables.
7. Corpus ingestion.
8. Running evaluation.
9. Testing.
10. Deployment.
11. Security notes.
12. Disclaimer.
13. Development status.
14. Known limitations.

## Completion benchmark

A new developer can clone the repository and understand:

- what it does;
- how to run it;
- where the data comes from;
- how the AI works;
- how to run tests.

---

# PHASE 27 — FINAL ARCHITECTURE VALIDATION

## Goal

Before release, verify the architecture rather than simply testing features.

The architecture document provides an explicit validation checklist covering scope, data, AI, security and operations. fileciteturn4file1L144-L181

## Scope

- [ ] Only three domains enabled.
- [ ] Central law only.
- [ ] State-specific law excluded.
- [ ] Foreign-national/visa excluded.
- [ ] UI does not imply universal Indian-law coverage.

## Data

- [ ] Source authority metadata exists.
- [ ] Publication/effective/retrieval dates exist where available.
- [ ] Chunks link to source versions.
- [ ] Corpus is reproducible.

## AI

- [ ] RAG mandatory.
- [ ] Citation IDs map to retrieval results.
- [ ] No-answer path exists.
- [ ] Fallback works.
- [ ] Golden set passes.

## Security

- [ ] No secrets client-side.
- [ ] Validation works.
- [ ] Rate limiting works.
- [ ] Sensitive-data warning visible.
- [ ] Admin routes protected.
- [ ] Model output safely rendered.

## Operations

- [ ] Deployment reproducible.
- [ ] Migrations versioned.
- [ ] Ingestion versioned.
- [ ] Request IDs logged.
- [ ] Provider failures visible.
- [ ] Recovery process documented.

---

# PHASE 28 — FINAL RELEASE GATE

Do not call LawInc “MVP complete” until ALL conditions below are satisfied.

## Product

- [ ] User can ask a question without onboarding selectors.
- [ ] Three domains work.
- [ ] Unsupported queries are handled clearly.
- [ ] Disclaimer is always shown.

## Retrieval

- [ ] Corpus is reproducible.
- [ ] Sources are authoritative.
- [ ] Source versions exist.
- [ ] Retrieval benchmark passes.

## AI

- [ ] Provider selected based on evaluation.
- [ ] Fallback tested.
- [ ] Structured output validated.
- [ ] Citation provenance validated.
- [ ] Hallucination tests pass.

## Security

- [ ] API keys are private.
- [ ] Input validation works.
- [ ] Rate limiting works.
- [ ] Prompt-injection tests pass.
- [ ] Sensitive-data warning exists.

## Reliability

- [ ] Failure paths tested.
- [ ] Logs work.
- [ ] Recovery procedure written.

## Deployment

- [ ] Production URL works.
- [ ] Database migrations work.
- [ ] Environment variables configured.
- [ ] Smoke test passes.

## Documentation

- [ ] README updated.
- [ ] Architecture document current.
- [ ] ADR register current.
- [ ] Runbook exists.
- [ ] Evaluation report exists.

---

# PHASE 29 — POST-MVP: DO NOT START IMMEDIATELY

Only after the MVP has been tested should you consider:

```text
1. Better retrieval
2. Better tax freshness automation
3. Hindi/Hinglish improvement
4. Better source UX
5. User feedback analytics
6. More robust monitoring
7. Automated source change detection
8. Authentication
9. Saved conversations
10. Document upload
```

Do not jump to:

```text
microservices
Kubernetes
agents
fine-tuning
multi-region
dedicated vector database
Kafka
```

unless measurable requirements justify them.

The architecture explicitly says these should not be implemented before measurable demand. fileciteturn2file4L377-L387

---

# 5. CHRONOLOGICAL MASTER TASK LIST

This is the condensed version to use as your actual checklist.

## Phase 0 — Understand

- [ ] Read current architecture.
- [ ] Read ADR register.
- [ ] Confirm three-domain scope.
- [ ] Mark old files as legacy.
- [ ] Write current requirements.
- [ ] Understand high-level RAG flow.

## Phase 1 — Environment

- [ ] Install Git.
- [ ] Install Node.js.
- [ ] Install VS Code.
- [ ] Configure Git.
- [ ] Create GitHub repo.
- [ ] Create `.gitignore`.
- [ ] Push first commit.

## Phase 2 — Scaffold

- [ ] Initialize Next.js.
- [ ] Configure TypeScript.
- [ ] Configure Tailwind.
- [ ] Create landing page.
- [ ] Create health endpoint.
- [ ] Create folders.
- [ ] Add `.env.example`.

## Phase 3 — Database

- [ ] Create Supabase project.
- [ ] Enable pgvector.
- [ ] Write migrations.
- [ ] Create tables.
- [ ] Create indexes.
- [ ] Test transactions.
- [ ] Test migration recreation.

## Phase 4 — Legal Sources

- [ ] Define source hierarchy.
- [ ] Define Traffic sources.
- [ ] Define Tax/GST sources.
- [ ] Define Cyber sources.
- [ ] Define conflict rules.
- [ ] Define freshness metadata.
- [ ] Define source manifest.

## Phase 5 — Ingestion

- [ ] Download sample sources.
- [ ] Parse PDFs/HTML.
- [ ] OCR where needed.
- [ ] Clean text.
- [ ] Extract legal structure.
- [ ] Chunk.
- [ ] Generate hashes.
- [ ] Attach metadata.
- [ ] Create manifest.
- [ ] Validate output.

## Phase 6 — Retrieval

- [ ] Build embeddings.
- [ ] Store vectors.
- [ ] Build vector search.
- [ ] Build exact matching.
- [ ] Add metadata filtering.
- [ ] Add scoring.
- [ ] Add reranking.
- [ ] Build retrieval test set.
- [ ] Measure Recall@5/10.

## Phase 7 — LLM POC

- [ ] Integrate candidate provider 1.
- [ ] Integrate candidate provider 2.
- [ ] Build shared provider interface.
- [ ] Build benchmark.
- [ ] Measure quality.
- [ ] Measure latency.
- [ ] Measure failures.
- [ ] Select candidate.

## Phase 8 — Answer Engine

- [ ] Write system prompt.
- [ ] Write developer prompt.
- [ ] Create context builder.
- [ ] Define JSON schema.
- [ ] Add prompt versioning.
- [ ] Generate grounded answer.
- [ ] Test prompt injection.

## Phase 9 — Safety

- [ ] Citation validation.
- [ ] Output schema validation.
- [ ] Out-of-scope state.
- [ ] Insufficient-authority state.
- [ ] One retry/regeneration.
- [ ] Safe fallback.
- [ ] Sensitive-data warning.

## Phase 10 — API

- [ ] `/health`.
- [ ] `/chat`.
- [ ] `/feedback`.
- [ ] `/sources/:id`.
- [ ] Admin corpus routes.
- [ ] Request validation.
- [ ] Error envelope.
- [ ] Rate limiting.

## Phase 11 — Session

- [ ] Anonymous session.
- [ ] Secure cookie.
- [ ] Conversation persistence.
- [ ] Session expiry.
- [ ] Rate-limit state.
- [ ] Retention policy.

## Phase 12 — Frontend

- [ ] Landing.
- [ ] Chat.
- [ ] Loading state.
- [ ] Error state.
- [ ] Source cards.
- [ ] Disclaimer.
- [ ] Out-of-scope UX.
- [ ] Insufficient-authority UX.
- [ ] Mobile layout.

## Phase 13 — Integration

- [ ] Traffic end-to-end.
- [ ] Tax end-to-end.
- [ ] Cyber end-to-end.
- [ ] Unsupported end-to-end.
- [ ] Follow-up conversation.

## Phase 14 — Evaluation

- [ ] 70-query benchmark.
- [ ] Retrieval metrics.
- [ ] Citation metrics.
- [ ] Groundedness metrics.
- [ ] Refusal metrics.
- [ ] Latency metrics.
- [ ] Regression report.

## Phase 15 — Security/Operations

- [ ] Secret review.
- [ ] OWASP review.
- [ ] Prompt injection tests.
- [ ] Abuse tests.
- [ ] Dependency scan.
- [ ] Structured logs.
- [ ] Request IDs.
- [ ] Failure alerts/visibility.

## Phase 16 — Deployment

- [ ] Production DB.
- [ ] Production secrets.
- [ ] Deploy application.
- [ ] Run migrations.
- [ ] Load corpus.
- [ ] Run smoke tests.
- [ ] Verify production URL.

## Phase 17 — User testing

- [ ] Recruit 5–10 testers.
- [ ] Observe usage.
- [ ] Collect feedback.
- [ ] Fix P0/P1 problems.
- [ ] Repeat smoke tests.

## Phase 18 — Release

- [ ] README updated.
- [ ] ADRs updated.
- [ ] Architecture validated.
- [ ] Golden set passes.
- [ ] Security checks pass.
- [ ] Recovery process documented.
- [ ] MVP released.

---

# 6. WHAT YOU SHOULD LEARN AS YOU GO

Do not try to become an expert in everything before building.

Use this dependency-based learning map:

```text
Git
 ↓
JavaScript/TypeScript basics
 ↓
React/Next.js
 ↓
HTTP/API
 ↓
PostgreSQL
 ↓
SQL
 ↓
Embeddings
 ↓
Vector search
 ↓
RAG
 ↓
LLM APIs
 ↓
Prompt engineering
 ↓
Structured outputs
 ↓
Testing
 ↓
Security
 ↓
Deployment
 ↓
Observability
```

---

# 7. MINIMUM LEARNING CHECKPOINTS

Before each major phase, answer the following.

## Before database

> What is a table, primary key, foreign key and index?

## Before ingestion

> What is a document chunk and why does metadata matter?

## Before embeddings

> What does an embedding represent?

## Before retrieval

> What is semantic similarity?

## Before LLM integration

> What is an API request and response?

## Before prompts

> Why does retrieved evidence need to be separated from user instructions?

## Before citation validation

> How can I prove the answer actually used the retrieved source?

## Before deployment

> Where do my secrets live?

## Before release

> How do I know this version is better than the previous one?

---

# 8. BEGINNER RULES FOR THIS PROJECT

## Rule 1

Do not build multiple components simultaneously.

Finish one vertical slice.

## Rule 2

When confused, identify which layer you are working in:

```text
UI
API
business logic
retrieval
AI
database
infrastructure
```

## Rule 3

When something fails, reproduce the smallest failure possible.

## Rule 4

Do not paste random code from tutorials without understanding where it fits.

## Rule 5

Commit frequently.

Recommended:

```text
feat:
fix:
refactor:
docs:
test:
chore:
```

## Rule 6

Keep a development journal.

Create:

```text
docs/dev-log.md
```

Every day record:

```text
What I learned
What I built
What broke
How I fixed it
What remains
```

## Rule 7

Keep an architecture decision log.

If you change:

```text
database
model
retrieval
deployment
auth
schema
source strategy
```

write/update the corresponding ADR.

---

# 9. DAILY WORKFLOW

For each development session:

```text
1. Read today's phase
       ↓
2. Identify what you don't understand
       ↓
3. Learn only that concept
       ↓
4. Implement the smallest version
       ↓
5. Test it
       ↓
6. Commit it
       ↓
7. Update dev log
       ↓
8. Check completion benchmark
       ↓
9. Move to next task
```

Do not use:

```text
learn everything
      ↓
build everything
```

Use:

```text
learn
 ↓
build
 ↓
test
 ↓
understand
 ↓
continue
```

---

# 10. FIRST 14 DAYS — PRACTICAL SCHEDULE

The architecture document originally maps the core work over approximately 14 days, from scope/source/schema through ingestion, API, fallback, UI, evaluation, deployment, security and release. fileciteturn4file0L14-L85

## DAY 1

### Goal

Environment + architecture understanding.

Tasks:

- Git/GitHub setup.
- Node setup.
- Read architecture.
- Create repository.
- Create Next.js app.
- Create requirements document.
- Create dev log.

### Completion

Application runs locally and first commit is pushed.

---

## DAY 2

### Goal

Database + schema.

Tasks:

- Supabase setup.
- pgvector.
- migrations.
- core tables.
- indexes.
- seed data.

### Completion

Database can be recreated from migrations.

---

## DAY 3

### Goal

First legal corpus.

Tasks:

- authority matrix;
- obtain representative sources;
- parse documents;
- chunk;
- metadata;
- hashes.

### Completion

Three-domain sample corpus exists.

---

## DAY 4

### Goal

Embeddings + retrieval.

Tasks:

- embed chunks;
- store vectors;
- vector search;
- lexical search;
- metadata filtering.

### Completion

Correct source appears in top results for initial retrieval benchmark.

---

## DAY 5

### Goal

AI POC.

Tasks:

- provider abstraction;
- provider 1;
- provider 2;
- benchmark;
- select candidate.

### Completion

Model decision is evidence-backed.

---

## DAY 6

### Goal

Grounded answer engine.

Tasks:

- prompts;
- structured output;
- context builder;
- answer generator.

### Completion

Question → grounded JSON answer.

---

## DAY 7

### Goal

Citation + safety.

Tasks:

- citation validator;
- output validator;
- fail closed;
- out-of-scope;
- insufficient authority.

### Completion

Fake citations cannot reach the user.

---

## DAY 8

### Goal

API + session.

Tasks:

- `/chat`;
- sessions;
- rate limit;
- errors;
- persistence.

### Completion

Complete backend API works through Postman/curl.

---

## DAY 9

### Goal

Frontend.

Tasks:

- landing;
- chat;
- loading;
- errors;
- citations;
- disclaimer.

### Completion

A friend can use the app.

---

## DAY 10

### Goal

End-to-end integration.

Tasks:

- three domains;
- unsupported;
- follow-up;
- provider fallback.

### Completion

All core flows work.

---

## DAY 11

### Goal

Evaluation.

Tasks:

- golden set;
- retrieval metrics;
- citation accuracy;
- groundedness;
- latency.

### Completion

Evaluation report exists.

---

## DAY 12

### Goal

Security + observability.

Tasks:

- secrets;
- rate limits;
- logs;
- prompt injection;
- dependency scan.

### Completion

Security checklist passes.

---

## DAY 13

### Goal

Deployment.

Tasks:

- production DB;
- production secrets;
- deployment;
- migrations;
- corpus;
- smoke tests.

### Completion

Production URL works.

---

## DAY 14

### Goal

Hardening + release.

Tasks:

- user testing;
- fix critical issues;
- README;
- runbook;
- ADR updates;
- final validation.

### Completion

All release gates pass.

---

# 11. PHASE COMPLETION SCORECARD

Use this table every day.

| Phase | Status | Benchmark passed? | Notes |
|---|---|---|---|
| 0 Scope | ⬜ | | |
| 1 Environment | ⬜ | | |
| 2 Scaffold | ⬜ | | |
| 3 Database | ⬜ | | |
| 4 Legal sources | ⬜ | | |
| 5 Ingestion | ⬜ | | |
| 6 Embeddings | ⬜ | | |
| 7 Retrieval | ⬜ | | |
| 8 LLM POC | ⬜ | | |
| 9 Answer engine | ⬜ | | |
| 10 Safety | ⬜ | | |
| 11 Router | ⬜ | | |
| 12 API | ⬜ | | |
| 13 Sessions | ⬜ | | |
| 14 Frontend | ⬜ | | |
| 15 Integration | ⬜ | | |
| 16 Evaluation | ⬜ | | |
| 17 Security | ⬜ | | |
| 18 Observability | ⬜ | | |
| 19 CI/CD | ⬜ | | |
| 20 Deployment | ⬜ | | |
| 21 Performance | ⬜ | | |
| 22 Failure testing | ⬜ | | |
| 23 Freshness | ⬜ | | |
| 24 User testing | ⬜ | | |
| 25 Hardening | ⬜ | | |
| 26 Documentation | ⬜ | | |
| 27 Architecture validation | ⬜ | | |
| 28 Release | ⬜ | | |

---

# 12. DEFINITION OF DONE

LawInc V1 is done only when:

```text
SCOPE
✓
Three Central-law domains only

DATA
✓
Authoritative corpus
✓
Versioned sources
✓
Effective dates
✓
Reproducible ingestion

RETRIEVAL
✓
Hybrid retrieval
✓
Retrieval benchmark passes

AI
✓
Provider benchmark completed
✓
Grounded prompt
✓
Structured output
✓
Citation provenance validation
✓
Fallback

SAFETY
✓
Out-of-scope handling
✓
Insufficient-authority handling
✓
Prompt-injection resistance
✓
Rate limiting

PRODUCT
✓
Working chat
✓
Source display
✓
Disclaimer
✓
Session continuity

QUALITY
✓
Golden evaluation passes
✓
Performance measured
✓
Failure tests pass

SECURITY
✓
No exposed secrets
✓
Input validation
✓
Safe output rendering
✓
Dependency review

OPERATIONS
✓
Logs
✓
Request IDs
✓
Deployment
✓
Recovery procedure
✓
Documentation
```

---

# 13. THE MOST IMPORTANT THING TO REMEMBER

Do not think of the project as:

```text
Build a chatbot.
```

Think of it as:

```text
Build a legal knowledge system
        +
Build a retrieval system
        +
Build a controlled AI explanation layer
        +
Build a trustworthy citation system
        +
Build a safe web interface
```

The LLM is only one component.

The architecture's central principle is that deterministic retrieval and source validation carry legal correctness while the LLM primarily synthesizes and explains retrieved evidence. fileciteturn1file0L19-L40

The most important success condition is therefore:

> **LawInc should prefer saying “I don't have enough authoritative evidence to answer this” over confidently giving unsupported legal information.**

---

# 14. YOUR STARTING POINT

When you begin implementation, do **not** start with the LLM.

Start here:

```text
TODAY
  ↓
PHASE 0
  ↓
freeze scope
  ↓
PHASE 1
  ↓
environment
  ↓
PHASE 2
  ↓
project skeleton
  ↓
PHASE 3
  ↓
database
```

The architecture's own implementation sequence starts by freezing scope, defining the authoritative source matrix and schema before building ingestion, embeddings, retrieval and model integration. fileciteturn4file1L117-L139

Once Phase 3 passes its benchmark, the next immediate job is **Phase 4 — Legal Source Strategy**.

Do not start downloading random legal PDFs before Phase 4 is complete.
