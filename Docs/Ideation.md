# LawInc — Ideation & Pre-Development Master Plan

> **AI-Powered Legal Awareness for India**  
> 100% Open Source • Zero Budget • Self-Hosted • Central + TN + Karnataka  
> Status: Pre-Development (Green Light ✅)

---

## 1. Problem & Scope Definition

### Core Value Proposition
> *"LawInc helps Indian citizens and foreign nationals in India understand their legal rights and take correct immediate action by providing Constitution-aware, citation-backed legal guidance via a web interface — built entirely with open-source software."*

### Target Users
1. **Indian Citizens** who encounter everyday legal situations (traffic violations, police encounters, tenant disputes, consumer grievances)
2. **Foreign Nationals** residing in or visiting India who need to understand visa rules, registration requirements, and their rights under Indian law
3. **NRIs / OCI Holders** dealing with property or legal matters in Central India, Tamil Nadu, or Karnataka

### V1 Feature Scope (MVP — 4 Weeks)

| Feature | Status |
|---------|--------|
| Web chat interface (anonymous, no auth) | V1 |
| 5 legal scenarios: Traffic, Police Harassment, Tenant-Landlord, Consumer Complaint, Foreigner Visa | V1 |
| Jurisdiction: Central Laws + Tamil Nadu + Karnataka only | V1 |
| Citation-enforced RAG (Act + Section + Year mandatory) | V1 |
| Self-hosted LLM (Ollama + Llama 3.1 8B) | V1 |
| Local embeddings (sentence-transformers BGE-M3) | V1 |
| Local vector DB (ChromaDB) | V1 |
| Severity detection + emergency bypass | V1 |
| English + Hindi/Hinglish support | V1 |
| Docker Compose deployment | V1 |

### V2 Features (Post-MVP)
- WhatsApp Business API integration
- Tamil + Kannada language support
- Voice input via Bhashini
- Auto-document generator (FIR drafts, RTI templates)
- Admin analytics dashboard
- Fine-tuned local model on Indian legal corpus

### Out of Scope (Never)
- Actual legal representation in court
- Giving binding "legal advice" (always informational)
- Storing sensitive PII long-term
- Predicting case outcomes
- Challenging existing judgments
- Coverage beyond Central + TN + Karnataka

### Success Criteria
> A user can describe a legal problem in plain Hindi/English via web chat, receive a citation-backed response referencing correct Central/TN/Karnataka laws with Act/Section/Year, see immediate actionable steps, and get emergency contacts if severity is high — with **zero external API costs** and **100% offline-capable inference**.

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

| Jurisdiction | Source | Format |
|-------------|--------|--------|
| Central Laws | India Code (indiacode.nic.in) | PDF / HTML |
| Tamil Nadu | TN Government Gazette Portal | PDF |
| Karnataka | Karnataka Gazette / Official State Portal | PDF |
| Summaries | PRS Legislative Research | HTML |

### Database Schema

#### Table 1: Users (Anonymous Sessions)
```sql
CREATE TABLE users (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    session_id UUID UNIQUE NOT NULL,
    profile_type VARCHAR(20) CHECK (profile_type IN ('indian_citizen', 'foreign_national', 'nri')),
    language_preference VARCHAR(20) DEFAULT 'en',
    jurisdiction VARCHAR(20) DEFAULT 'central',
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);
```

#### Table 2: Conversations
```sql
CREATE TABLE conversations (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    user_id UUID REFERENCES users(id),
    scenario_type VARCHAR(30) CHECK (scenario_type IN ('traffic', 'police', 'tenant', 'consumer', 'foreigner', 'constitutional')),
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
    jurisdiction VARCHAR(20) CHECK (jurisdiction IN ('central', 'tamil_nadu', 'karnataka')),
    act_name VARCHAR(200) NOT NULL,
    section VARCHAR(50) NOT NULL,
    year VARCHAR(10) NOT NULL,
    content TEXT NOT NULL,
    source_url TEXT,
    embedding_id VARCHAR(100),
    last_verified DATE DEFAULT CURRENT_DATE
);
```

---

## 3. UX/UI & Workflow Mapping

### User Journey (Happy Path)

1. **Landing**: User visits LawInc web app
2. **Onboarding**: Selects profile type (Citizen / Foreign National / NRI) and jurisdiction (Central / Tamil Nadu / Karnataka)
3. **Query**: Types legal problem in English or Hindi/Hinglish
4. **Severity Scan**: System checks for emergency keywords (`abhi`, `emergency`, `marne wala`, `dying`, `beating`, `rape`, `accident`)
   - If severity = 10 → **Emergency Bypass**: Return static emergency card only. No LLM call.
   - If severity < 10 → Proceed to RAG
5. **Retrieval**: ChromaDB retrieves relevant chunks from selected jurisdiction's legal corpus
6. **Generation**: Local LLM generates response using strict citation prompt
7. **Validation**: Post-processor extracts citations via regex and verifies against retrieved chunks
8. **Response Delivery**: Formatted response with 📜 Citation, 💰 Fine, 🛡️ Rights, 📋 Action Steps, ⚠️ Disclaimer
9. **Logging**: Conversation stored locally for analysis

### Wireframes

```
┌─────────────────────────────┐
│         LANDING             │
│                             │
│      [⚖️ LawInc]            │
│   "Know Your Rights"        │
│                             │
│   I am:                     │
│  [🇮🇳 Citizen] [🌍 Foreign]  │
│                             │
│   Jurisdiction:             │
│  [All India] [TN] [KA]      │
│                             │
│     [🚀 Start Chat]         │
│                             │
│ Free • Offline • Private    │
└─────────────────────────────┘

┌─────────────────────────────┐
│         CHAT UI             │
│                             │
│  [Jurisdiction: Karnataka]  │
│                             │
│  User: Police ne 5000       │
│        maanga red light     │
│        jump karne pe        │
│                             │
│  Bot:                       │
│  📜 Citation: Motor Vehicles│
│       Act, 1988, Sec 177    │
│  💰 Fine: ₹1,000 (KA rules) │
│  🛡️ Rights: Demand receipt  │
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

| Jurisdiction | Coverage |
|-------------|----------|
| Central | Constitution, IPC, CrPC, MV Act, Foreigners Act, Consumer Protection Act |
| Tamil Nadu | TN Motor Vehicles Rules, TN Rent Control Act, TN Police regulations |
| Karnataka | Karnataka Motor Vehicles Rules, Karnataka Rent Act, Karnataka Police Act |

**Fallback for other states:** *"I currently only cover Central laws, Tamil Nadu, and Karnataka. For other states, please contact your State Legal Services Authority (dlsa.gov.in)."*

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
| M2: Data Ingestion | Week 1-2 | Central + TN + KA acts in ChromaDB. Tesseract OCR for scans. |
| M3: Core Chat | Week 2-3 | FastAPI + Next.js. 5 scenarios. Severity + emergency live. |
| M4: Foreign National | Week 3 | Foreigners Act + visa + consular rights for TN/KA. |
| M5: Polish | Week 4 | Hindi/Hinglish. Docker Compose. README + demo video. |

---

## 5. Green Light Check

| Check | Status |
|-------|--------|
| Can I build V1 in 2–4 weeks using chosen stack? | ✅ YES |
| Am I writing custom code for something a managed service solves? | ✅ NO — self-hosting by design |
| Is every V1 feature tied to the Core Value Proposition? | ✅ YES |
| Can this run entirely offline? | ✅ YES |
| Is the scope limited enough for zero budget? | ✅ YES — 3 jurisdictions × 5 scenarios |
| Are emergency bypass and citation lock defined? | ✅ YES |
| Are legal disclaimers and ToS drafted? | ✅ YES — disclaimer on every response |
| Is the golden test set planned? | ✅ YES — 50-query benchmark |

---

## 6. Golden Test Set (Planned)

```json
[
  {
    "query": "What is the fine for jumping a red light in Karnataka?",
    "language": "en",
    "user_type": "indian_citizen",
    "jurisdiction": "karnataka",
    "expected_citations": [
      {"act": "Motor Vehicles Act", "section": "177", "year": "1988", "jurisdiction": "central"}
    ],
    "expected_answer_contains": ["₹1000", "first offence"],
    "severity": 2
  },
  {
    "query": "Police ne ghar mein ghuske pitai ki abhi ho raha hai",
    "language": "hi",
    "user_type": "indian_citizen",
    "jurisdiction": "central",
    "expected_response_type": "emergency_bypass",
    "severity": 10
  }
]
```

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

# Day 3: Download first 3 Central Acts
# - Constitution of India (key articles: 14, 19, 21, 22, 32)
# - IPC (Sections: 279, 304, 379, 420, 506)
# - Motor Vehicles Act, 1988 (Sections 177-202)
# Convert to text using pdftotext or Tesseract OCR
```

---

*Document Version: 1.0*  
*Last Updated: 2026-08-17*  
*Project Status: Ready for Development* 🚀
