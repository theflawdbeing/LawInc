<div align="center">

# ⚖️ LawInc

### AI-Powered Legal Awareness for India

**Know Your Rights. Zero Cost. Fully Private. Open Source.**

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)
[![Python](https://img.shields.io/badge/Python-3.11+-blue.svg)](https://python.org)
[![Next.js](https://img.shields.io/badge/Next.js-14-black)](https://nextjs.org)
[![Ollama](https://img.shields.io/badge/Ollama-Llama%203.1-ff6f00)](https://ollama.com)
[![Docker](https://img.shields.io/badge/Docker-Ready-2496ED)](https://docker.com)
[![Status](https://img.shields.io/badge/Status-In%20Development-brightgreen)]()

</div>

---

## 🎯 What is LawInc?

LawInc is a **100% open-source, self-hosted legal awareness assistant** designed for India. It helps citizens and foreign nationals understand their constitutional rights, navigate everyday legal situations, and take correct immediate action — all while running entirely on your local machine with **zero API costs** and **complete data privacy**.

Whether it's a traffic violation in Bangalore, a police encounter in Chennai, or visa questions for foreign nationals — LawInc provides **citation-backed guidance** referencing exact Acts, Sections, and Years from Indian law.

> ⚠️ **Disclaimer:** LawInc provides general legal information, not professional legal advice. Laws change and vary by jurisdiction. Always consult a licensed advocate for your specific situation.

---

## ✨ Key Features

- 🔒 **100% Offline & Private** — Self-hosted LLM (Llama 3.1) runs locally. No data leaves your machine.
- 📜 **Citation-Enforced Responses** — Every legal claim MUST cite the Act, Section, and Year. No hallucinations.
- 🚨 **Emergency Bypass** — Detects high-risk situations ("police beating right now") and immediately provides emergency contacts instead of AI responses.
- 🗺️ **Jurisdiction-Aware** — Covers **Central Laws + Tamil Nadu + Karnataka** with state-specific rules.
- 🌐 **Multilingual** — English and Hindi/Hinglish support out of the box.
- 🌍 **Foreign National Module** — Special guidance on visa rules, FRRO registration, consular rights, and the Foreigners Act.
- 🐳 **One-Command Deploy** — Docker Compose spins up the entire stack instantly.
- 💰 **Zero Budget** — Built entirely with free and open-source software. No OpenAI, no Claude, no paid APIs.

---

## 🏗️ Architecture

```
┌─────────────────────────────────────────────────────────────┐
│                    NEXT.JS FRONTEND                          │
│              (shadcn/ui + Tailwind CSS)                      │
│         Landing Page → Chat Interface → Profile              │
└──────────────────────────┬──────────────────────────────────┘
                           │ HTTP/WebSocket
┌──────────────────────────▼──────────────────────────────────┐
│                    FASTAPI BACKEND                           │
│  • Intent Classification    • Severity Scoring               │
│  • Emergency Detection      • Citation Validation            │
│  • RAG Pipeline             • Response Formatting            │
└──────────────────────────┬──────────────────────────────────┘
                           │
        ┌──────────────────┼──────────────────┐
        ▼                  ▼                  ▼
┌──────────────┐   ┌──────────────┐   ┌──────────────┐
│   OLLAMA     │   │   CHROMADB   │   │  POSTGRESQL  │
│  Llama 3.1   │   │  (Vector DB) │   │  + pgvector  │
│   8B Local   │   │              │   │              │
└──────────────┘   └──────────────┘   └──────────────┘
        │                  │                  │
        └──────────────────┼──────────────────┘
                           │
┌──────────────────────────▼──────────────────────────────────┐
│              LEGAL KNOWLEDGE BASE                            │
│  Tier 1: Constitution + Fundamental Rights                  │
│  Tier 2: IPC, CrPC, MV Act, Foreigners Act (Central)        │
│  Tier 3: Tamil Nadu State Rules                             │
│  Tier 4: Karnataka State Rules                              │
│  Tier 5: Sample complaints, FIR formats, RTI templates      │
└─────────────────────────────────────────────────────────────┘
```

---

## 🛠️ Tech Stack

| Layer | Technology | Purpose |
|-------|-----------|---------|
| **LLM** | [Ollama](https://ollama.com) + Llama 3.1 8B | Local inference, zero API costs |
| **Embeddings** | [sentence-transformers](https://sbert.net) BGE-M3 | Multilingual legal document vectors |
| **RAG** | [LangChain](https://python.langchain.com) Community | Retrieval-augmented generation |
| **Vector DB** | [ChromaDB](https://trychroma.com) | Local document retrieval |
| **Backend** | [FastAPI](https://fastapi.tiangolo.com) | High-performance async Python API |
| **Frontend** | [Next.js 14](https://nextjs.org) | React web interface |
| **Database** | [PostgreSQL 15](https://postgresql.org) + [pgvector](https://github.com/pgvector/pgvector) | Persistence & vector search |
| **OCR** | [Tesseract](https://github.com/tesseract-ocr/tesseract) | Scanned PDF parsing |
| **UI** | [shadcn/ui](https://ui.shadcn.com) + Tailwind CSS | Accessible, beautiful components |
| **DevOps** | [Docker](https://docker.com) + Docker Compose | One-command local deployment |
| **Monitoring** | [Prometheus](https://prometheus.io) + [Grafana](https://grafana.com) | Self-hosted observability |

---

## 🚀 Quick Start

### Prerequisites

- [Docker](https://docs.docker.com/get-docker/) & Docker Compose
- [Ollama](https://ollama.com/download) installed locally
- 8GB+ RAM (16GB recommended for smooth LLM inference)

### 1. Clone the Repository

```bash
git clone https://github.com/yourusername/lawinc.git
cd lawinc
```

### 2. Pull the LLM Model

```bash
ollama pull llama3.1:8b
```

### 3. Start the Stack

```bash
docker-compose up --build
```

This will spin up:
- 🖥️ **Frontend** at `http://localhost:3000`
- ⚙️ **Backend API** at `http://localhost:8000`
- 🗄️ **PostgreSQL** at `localhost:5432`
- 📊 **Grafana** at `http://localhost:3001`

### 4. Ingest Legal Documents (First Run)

```bash
# Download and process Central + TN + Karnataka acts
docker-compose exec backend python scripts/ingest_legal_docs.py
```

### 5. Start Chatting

Open `http://localhost:3000`, select your profile and jurisdiction, and start asking legal questions.

---

## 📁 Project Structure

```
lawinc/
├── backend/
│   ├── app/
│   │   ├── main.py                 # FastAPI entry point
│   │   ├── routers/
│   │   │   ├── chat.py             # Chat endpoints
│   │   │   └── health.py           # Health checks
│   │   ├── services/
│   │   │   ├── rag_engine.py       # ChromaDB + LangChain RAG
│   │   │   ├── citation_validator.py # Post-process citation check
│   │   │   ├── severity_detector.py  # Emergency keyword scan
│   │   │   └── llm_client.py       # Ollama integration
│   │   ├── models/
│   │   │   └── schema.py           # Pydantic models
│   │   └── core/
│   │       ├── config.py           # Environment config
│   │       └── constants.py        # Emergency keywords, jurisdictions
│   ├── scripts/
│   │   ├── ingest_legal_docs.py    # PDF → text → ChromaDB pipeline
│   │   └── seed_test_data.py       # Golden test set loader
│   ├── data/
│   │   ├── raw/                    # Downloaded PDFs
│   │   └── processed/              # Cleaned text chunks
│   ├── Dockerfile
│   └── requirements.txt
├── frontend/
│   ├── app/
│   │   ├── page.tsx                # Landing page
│   │   ├── chat/
│   │   │   └── page.tsx            # Chat interface
│   │   └── layout.tsx
│   ├── components/
│   │   ├── chat/
│   │   │   ├── MessageBubble.tsx
│   │   │   └── ChatInput.tsx
│   │   └── ui/                     # shadcn/ui components
│   ├── lib/
│   │   └── api.ts                  # Backend API client
│   ├── Dockerfile
│   └── package.json
├── docker-compose.yml
├── prometheus.yml
├── Makefile
└── README.md
```

---

## 🧪 Evaluation & Safety

### Citation Enforcement

Every AI response is post-processed to ensure:
1. At least one citation exists (Act, Section, Year)
2. The citation matches retrieved documents from ChromaDB
3. If validation fails, a fallback message is returned instead of hallucinated law

### Emergency Bypass

High-risk keywords immediately trigger a static emergency response:
- 🚨 **Police Control Room:** 100
- 👩 **Women Helpline:** 181
- 👶 **Childline:** 1098
- ⚖️ **National Legal Aid:** 15100
- 🚑 **Ambulance:** 108

### Golden Test Set

A curated benchmark of 50 legal queries with verified answers is included in `backend/data/golden_test_set.json`. Run evaluation with:

```bash
docker-compose exec backend python scripts/evaluate.py
```

---

## 🗺️ Supported Jurisdictions

| Jurisdiction | Coverage |
|-------------|----------|
| 🇮🇳 **Central Laws** | Constitution, IPC, CrPC, Motor Vehicles Act, Foreigners Act, Consumer Protection Act |
| 🏛️ **Tamil Nadu** | TN Motor Vehicles Rules, TN Rent Control Act, TN Police regulations |
| 🏛️ **Karnataka** | Karnataka Motor Vehicles Rules, Karnataka Rent Act, Karnataka Police Act |

> For other states, LawInc will direct you to your State Legal Services Authority.

---

## 🌐 Supported Languages

| Language | Status |
|----------|--------|
| English | ✅ Full support |
| Hindi / Hinglish | ✅ V1 support |
| Tamil | 🚧 V2 planned |
| Kannada | 🚧 V2 planned |

---

## 📸 Screenshots

*(Coming soon — add screenshots of the chat interface, emergency bypass, and citation display)*

---

## 🛣️ Roadmap

- [x] Core RAG pipeline with local LLM
- [x] Citation enforcement & validation
- [x] Emergency bypass system
- [x] Central + TN + Karnataka laws
- [x] English + Hindi support
- [ ] WhatsApp Business API integration (V2)
- [ ] Tamil + Kannada language support (V2)
- [ ] Voice input via Bhashini (V2)
- [ ] Auto-document generator (FIR, RTI drafts) (V2)
- [ ] Admin analytics dashboard (V2)
- [ ] Fine-tuned legal LLM on Indian corpus (V2)

---

## 🤝 Contributing

We welcome contributions! Please see [CONTRIBUTING.md](./CONTRIBUTING.md) for guidelines.

Areas where help is needed:
- 🏛️ Expanding state-specific legal databases
- 🌐 Adding more Indian languages
- 🧪 Expanding the golden test set
- 📱 Mobile app (React Native / Flutter)

---

## ⚠️ Legal Disclaimer

LawInc is an **educational legal awareness tool**. It is **not** a substitute for professional legal advice, representation, or counsel. The information provided is general in nature and may not apply to your specific situation. Laws change frequently and vary by jurisdiction. Always consult a licensed advocate or legal professional for advice tailored to your circumstances.

The creators of LawInc assume no liability for any actions taken based on information provided by this tool.

---

## 📄 License

This project is licensed under the [MIT License](./LICENSE).

---

## 🙏 Acknowledgements

- [India Code](https://indiacode.nic.in) for bare acts and legal texts
- [PRS Legislative Research](https://prsindia.org) for policy summaries
- [Ollama](https://ollama.com) for making local LLMs accessible
- [shadcn/ui](https://ui.shadcn.com) for beautiful, accessible components
- The open-source community for making zero-budget innovation possible

---

<div align="center">

**Built with ❤️ for every Indian who deserves to know their rights.**

[⭐ Star this repo](https://github.com/yourusername/lawinc) if you find it useful!

</div>
