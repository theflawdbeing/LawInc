# LawInc — Ideation & Pre-Development Master Plan

> **AI-Powered Legal Awareness for India**
> 100% Open Source • Zero Budget • Self-Hosted • Central Law Only
> Status: Pre-Development (Green Light ✅)

---

## 1. Problem & Scope Definition

### Core Value Proposition
> *"LawInc helps Indian citizens understand their legal rights and take correct immediate action by providing Constitution-aware, citation-backed legal guidance via a web interface — built entirely with open-source software."*

### Target Users
**Indian citizens only**, encountering everyday legal situations across six domains: fundamental rights, consumer disputes, employment/labour issues, traffic violations, cybercrime, and police harassment.

*(No nationality or profile-type selection at onboarding — the product assumes an Indian citizen user by default.)*

### V1 Feature Scope (MVP — 4 Weeks)

| Feature | Status |
|---------|--------|
| Web chat interface (anonymous, no auth) | V1 |
| 6 legal domains: Constitutional, Consumer, Employment & Labour, Traffic, Cybercrime, Police Harassment | V1 |
| Jurisdiction: Central Laws only (India-wide) | V1 |
| Citation-enforced RAG (Act + Section + Year mandatory) | V1 |
| Self-hosted LLM (Ollama + Llama 3.1 8B) | V1 |
| Local embeddings (sentence-transformers BGE-M3) | V1 |
| Local vector DB (ChromaDB) | V1 |
| Severity detection + emergency bypass | V1 |
| English + Hindi/Hinglish support | V1 |
| Docker Compose deployment | V1 |

### V2 Features (Post-MVP)
- WhatsApp Business API integration
- Additional Indian language support (Tamil, Kannada, etc.)
- Voice input via Bhashini
- Auto-document generator (FIR drafts, RTI templates, consumer complaint drafts)
- Admin analytics dashboard
- Fine-tuned local model on Indian legal corpus
- State-specific law layers (if expanded beyond Central-only)

### Out of Scope (Never)
- Actual legal representation in court
- Giving binding "legal advice" (always informational)
- Storing sensitive PII long-term
- Predicting case outcomes
- Challenging existing judgments
- Foreign nationals' visa, immigration, or Foreigners Act guidance
- Tenancy / landlord-tenant law (state subject — excluded)
- Any state-specific laws or jurisdiction selection (Central law only)

### Success Criteria
> A user can describe a legal problem in plain Hindi/English via web chat, receive a citation-backed response referencing the correct Central law with Act/Section/Year, see immediate actionable steps, and get emergency contacts if severity is high — with **zero external API costs** and **100% offline-capable inference**.

---

## 2. Architecture & Tech Stack (100% FOSS)

### Complete Stack

| Layer | Technology | License | Purpose |
|-------|-----------|---------|---------|
| LLM Inference | Ollama + Llama 3.1 8B (Q4_K_M) | Apache 2.0 / MIT | Local inference, zero API costs |
| Embeddings | sentence-transformers (BGE-M3) | Apache 2.0 | Multilingual text vectorization |
| RAG Framework | LangChain Community (Python) | MIT | Retrieval-augmented generation |
| Vector Database | ChromaDB (persistent local) | Apache 2.0 | Document retrieval |
| Backend API | FastAPI | MIT | Async Python API |
| Frontend | Next.js 14 (App Router) | MIT | React web interface |
| Database | PostgreSQL 15 + pgvector | PostgreSQL License | Persistence + vector support |
| OCR | Tesseract + pytesseract | Apache 2.0 | Scanned PDF parsing |
| Containerization | Docker + Docker Compose | Apache 2.0 | One-command deployment |
| Monitoring | Prometheus + Grafana | Apache 2.0 | Metrics & observability |
| UI Components | shadcn/ui + Tailwind CSS + Radix UI | MIT | Accessible component library |

### Data Sources (Free & Legal)

| Source | Format | Notes |
|--------|--------|-------|
| India Code (indiacode.nic.in) | PDF / HTML | Single canonical source for all Central Acts |
| PRS Legislative Research | HTML | Plain-language summaries for context/validation |
| Supreme Court judgments (landmark only, e.g. D.K. Basu) | PDF / HTML | For Police Harassment domain — procedural guidelines beyond statute |

*(TN Gazette, Karnataka Gazette, and all state-level sources are no longer required.)*

### Database Schema

#### Table 1: Users (Anonymous Sessions)
```sql
CREATE TABLE users (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    session_id UUID UNIQUE NOT NULL,
    language_preference VARCHAR(20) DEFAULT 'en',
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);
```
*(`profile_type` and `jurisdiction` columns removed — no longer needed since the product is Indian-citizen-only and Central-law-only.)*

#### Table 2: Conversations
```sql
CREATE TABLE conversations (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    user_id UUID REFERENCES users(id),
    scenario_type VARCHAR(30) CHECK (scenario_type IN ('constitutional', 'consumer', 'employment', 'traffic', 'cybercrime', 'police')),
    severity_score INTEGER CHECK (severity_score BETWEEN 1 AND 10),
    user_query TEXT NOT NULL,
    ai_response TEXT,
    citations JSONB,
    citation_verified BOOLEAN DEFAULT FALSE,
    escalation_triggered BOOLEAN DEFAULT FALSE,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);
```

#### Table 3: Citations Registry
```sql
CREATE TABLE citations_registry (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    act_name VARCHAR(200) NOT NULL,
    section VARCHAR(50) NOT NULL,
    year VARCHAR(10) NOT NULL,
    content TEXT NOT NULL,
    source_url TEXT,
    embedding_id VARCHAR(100),
    last_verified DATE DEFAULT CURRENT_DATE
);
```
*(`jurisdiction` column removed — every entry is implicitly Central law.)*

---

## 3. UX/UI & Workflow Mapping

### User Journey (Happy Path)

1. **Landing**: User visits LawInc web app
2. **Query**: Types legal problem directly in English or Hindi/Hinglish — no onboarding selectors needed
3. **Severity Scan**: System checks for emergency keywords (`abhi`, `emergency`, `marne wala`, `dying`, `beating`, `rape`, `accident`)
   - If severity = 10 → **Emergency Bypass**: Return static emergency card only. No LLM call.
   - If severity < 10 → Proceed to RAG
4. **Retrieval**: ChromaDB retrieves relevant chunks from the Central legal corpus
5. **Generation**: Local LLM generates response using strict citation prompt
6. **Validation**: Post-processor extracts citations via regex and verifies against retrieved chunks
7. **Response Delivery**: Formatted response with 📜 Citation, 💰 Fine/Penalty, 🛡️ Rights, 📋 Action Steps, ⚠️ Disclaimer
8. **Logging**: Conversation stored locally for analysis

### Wireframes

```
┌─────────────────────────────┐
│         LANDING             │
│                             │
│      [⚖️ LawInc]            │
│   "Know Your Rights"        │
│                             │
│   Type your legal question  │
│   in English or Hindi...    │
│                             │
│     [🚀 Start Chat]         │
│                             │
│ Free • Offline • Private    │
└─────────────────────────────┘

┌─────────────────────────────┐
│         CHAT UI             │
│                             │
│  User: Police ne 5000       │
│        maanga red light     │
│        jump karne pe        │
│                             │
│  Bot:                       │
│  📜 Citation: Motor Vehicles│
│       Act, 1988, Sec 177    │
│  💰 Fine: ₹1,000 (first     │
│       offence)              │
│  🛡️ Rights: Demand e-challan│
│       or written receipt    │
│  📋 Steps: 1. Ask for       │
│       e-challan...          │
│  ⚠️ Disclaimer: Not legal   │
│       advice...             │
│                             │
│  [📄 Download Summary]      │
└─────────────────────────────┘
```

---

## 4. Risk & Constraint Management

### The Dragon (Primary Technical Risk)

> **Running a 7B parameter LLM locally with strict citation enforcement while maintaining <5s response time on consumer hardware.**

**Spike Solution (Week 1 Priority):**
- Install Ollama, pull `llama3.1:8b`
- Build throwaway `citation_validator.py`
- Test structured prompt forcing with retrieved ChromaDB context
- Success metric: **90%+ citation accuracy** on 50 golden test queries

### Data Scope Constraint

| Domain | Core Acts / Sources |
|--------|---------------------|
| Fundamental Rights / Constitutional | Constitution of India (Art. 14, 19, 21, 22, 32) |
| Consumer Rights | Consumer Protection Act, 2019 |
| Employment & Labour | Code on Wages 2019, Industrial Disputes Act 1947, POSH Act 2013 |
| Traffic Rules | Motor Vehicles Act, 1988 (+ amendments) |
| Cybercrime | IT Act 2000, relevant IPC/BNS sections (66C, 66D, 67) |
| Police Harassment | CrPC/BNSS (arrest safeguards, Sec 41/41A), IPC/BNS (custodial offences), Constitution Art. 21/22, D.K. Basu guidelines (SC) |

**Explicitly excluded:** Tenancy law, foreign nationals'/visa/immigration law, and any state-specific statutes. All content is Central law, applicable uniformly across India.

**Fallback for out-of-scope queries:** *"I currently cover Fundamental Rights, Consumer, Employment & Labour, Traffic, Cybercrime, and Police Harassment under Central Indian law. For other matters, please contact your State Legal Services Authority (dlsa.gov.in) or a qualified lawyer."*

### Emergency Bypass System

```python
EMERGENCY_KEYWORDS = [
    "abhi", "right now", "emergency", "marne wala", "dying",
    "beating", "rape", "accident", "murder", "fire", "help me",
    "police beating", "someone dying"
]

if any(kw in query.lower() for kw in EMERGENCY_KEYWORDS):
    return EMERGENCY_CARD  # Static response, NO LLM call
```

### Quality Standards

| Standard | Implementation |
|----------|---------------|
| Responsive | Mobile-first Tailwind CSS |
| Error Handling | Graceful degradation if LLM offline |
| Citation Lock | Post-process regex extraction + verification |
| Emergency Bypass | Keyword-based immediate escalation |
| Disclaimer | Every response ends with legal disclaimer |
| Privacy | Anonymous sessions, no PII storage |

### Timeboxing

| Milestone | Duration | Deliverable |
|-----------|----------|-------------|
| M1: The Dragon | Week 1 | Ollama + Llama 3.1 running. Citation spike ≥90% accuracy. |
| M2: Data Ingestion | Week 1-2 | All 6 domains' Central Acts in ChromaDB. Tesseract OCR for scans. |
| M3: Core Chat | Week 2-3 | FastAPI + Next.js. 6 domains live. Severity + emergency bypass live. |
| M4: Domain Depth | Week 3 | Police Harassment + Cybercrime domains fully tested (newer, less "textbook" than Traffic/Consumer). |
| M5: Polish | Week 4 | Hindi/Hinglish. Docker Compose. README + demo video. |

---

## 5. Green Light Check

| Check | Status |
|-------|--------|
| Can I build V1 in 2–4 weeks using chosen stack? | ✅ YES |
| Am I writing custom code for something a managed service solves? | ✅ NO — self-hosting by design |
| Is every V1 feature tied to the Core Value Proposition? | ✅ YES |
| Can this run entirely offline? | ✅ YES |
| Is the scope limited enough for zero budget? | ✅ YES — Central law only × 6 domains |
| Are emergency bypass and citation lock defined? | ✅ YES |
| Are legal disclaimers and ToS drafted? | ✅ YES — disclaimer on every response |
| Is the golden test set planned? | ✅ YES — ~50-60 query benchmark (≈8-10 per domain) |

---

## 6. Golden Test Set (Planned)

```json
[
  {
    "query": "What is the fine for jumping a red light?",
    "language": "en",
    "domain": "traffic",
    "expected_citations": [
      {"act": "Motor Vehicles Act", "section": "177", "year": "1988"}
    ],
    "expected_answer_contains": ["₹1000", "first offence"],
    "severity": 2
  },
  {
    "query": "Police ne ghar mein ghuske pitai ki abhi ho raha hai",
    "language": "hi",
    "domain": "police",
    "expected_response_type": "emergency_bypass",
    "severity": 10
  }
]
```

*Target: ~8-10 golden queries per domain × 6 domains = ~50-60 total test queries.*

---

## 7. Immediate Next Steps

```bash
# Day 1: Setup local LLM
curl -fsSL https://ollama.com/install.sh | sh
ollama pull llama3.1:8b
ollama run llama3.1:8b

# Day 2: Project scaffold
mkdir law-inc && cd law-inc
# docker-compose.yml (PostgreSQL + optional Grafana)
# backend/ (FastAPI + ChromaDB + LangChain)
# frontend/ (Next.js + shadcn/ui)

# Day 3: Download first Central Acts (one per domain to start)
# - Constitution of India (key articles: 14, 19, 21, 22, 32)
# - Consumer Protection Act, 2019 (key sections)
# - Motor Vehicles Act, 1988 (Sections 177-202)
# - IT Act, 2000 (Sections 66C, 66D, 67)
# - CrPC/BNSS (Sections 41, 41A - arrest safeguards)
# Convert to text using pdftotext or Tesseract OCR
```

---

*Document Version: 2.0*
*Last Updated: 2026-08-17*
*Project Status: Ready for Development* 🚀
