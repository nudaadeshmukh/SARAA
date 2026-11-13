<div align="center">

# SARAA

### AI-Assisted Suspicious Activity Report (SAR) Narrative Generation

*From transaction alerts to regulator-ready narratives, with a complete audit trail at every step.*

![Python](https://img.shields.io/badge/Python-3.11-3776AB?logo=python&logoColor=white)
![FastAPI](https://img.shields.io/badge/FastAPI-009688?logo=fastapi&logoColor=white)
![PostgreSQL](https://img.shields.io/badge/PostgreSQL-15-4169E1?logo=postgresql&logoColor=white)
![LangChain](https://img.shields.io/badge/LangChain-RAG-1C3C3C)
![ChromaDB](https://img.shields.io/badge/ChromaDB-Vector%20Store-FF6F00)
![Status](https://img.shields.io/badge/Status-Prototype-orange)

</div>

---

## Table of Contents

- [Overview](#overview)
- [Problem Statement](#problem-statement)
- [Key Features](#key-features)
- [Pipeline Architecture](#pipeline-architecture)
- [Project Structure](#project-structure)
- [Tech Stack](#tech-stack)
- [Getting Started](#getting-started)
- [Usage](#usage)
- [Audit Trail API](#audit-trail-api)
- [Auditability](#auditability)
- [Regulatory Readiness Checks](#regulatory-readiness-checks)
- [Human Review](#human-review)
- [Testing](#testing)

---

## Overview

Financial crime compliance teams spend a large share of their time drafting SAR narratives by hand, stitching together fragmented data from alerts, case systems, KYC records, transaction histories and investigator notes.

**SARAA** structures that work into an auditable, staged pipeline:

> Ingestion → Enrichment → Rule evaluation → Claim generation → Narrative drafting (RAG) → Final compliance validation

It was built for the **Barclays Hack-O-Hire** challenge as a hackathon-grade, production-minded prototype. The emphasis is on transparency, explainability and end-to-end traceability, so every generated narrative can be defended in internal review or regulatory scrutiny.

## Problem Statement

SAR filing is mandatory whenever a bank detects activity that may indicate money laundering, fraud or related financial crime. Drafting the narrative is typically **manual, slow and inconsistent**: analysts must interpret complex transaction patterns, connect them to customer context, and write a clear report that meets regulatory expectations.

SARAA addresses this by:

- Reducing manual narrative drafting effort
- Preserving a complete audit trail for every transformation
- Producing structured claim objects that map evidence to suspicious patterns
- Keeping a human reviewer in control before anything is filed

## Key Features

| Feature | Description |
|---|---|
| **Automated narrative generation** | Drafts SAR narratives from alerts, KYC and transaction data |
| **Structured claim objects** | Case metadata, subject context, suspicious patterns, evidence references and integrity hashes |
| **Configurable rule engine** | YAML-defined fraud / AML rules, with every evaluation logged |
| **RAG-based regulatory context** | Retrieves guidance, SAR templates and historical narratives from ChromaDB |
| **Local LLM** | Narratives generated through Ollama, so no data leaves the machine |
| **Audit trail & lineage** | Immutable, queryable logs across all pipeline stages |
| **Regulatory readiness validation** | 10-point final check before a filing bundle is stored |
| **Human-in-the-loop** | Analysts edit, approve or reject drafts |
| **PostgreSQL persistence** | Stores claims, audit logs and final filing artifacts |

## Pipeline Architecture

```text
 ┌───────────┐   ┌────────────┐   ┌─────────────┐   ┌──────────────┐   ┌─────────────┐   ┌──────────┐
 │ Ingestion │ → │ Enrichment │ → │ Rule Engine │ → │ Claim        │ → │ Narrative   │ → │ OMEGA    │
 │ & Normal- │   │ (KYC, risk │   │ (typologies │   │ Generation   │   │ (RAG + LLM) │   │ Finalize │
 │ ization   │   │  profile)  │   │  & rules)   │   │              │   │             │   │ & Store  │
 └───────────┘   └────────────┘   └─────────────┘   └──────────────┘   └─────────────┘   └──────────┘
        └──────────────────────── Audit trail captured at every stage ─────────────────────────┘
```

| # | Stage | What happens |
|---|---|---|
| 1 | **Ingestion & Normalization** | Alerts, customer KYC, account data and case details are collected and standardized |
| 2 | **KYC Enrichment** | Customer risk profile, account history and contextual signals are attached |
| 3 | **Rule Engine** | Suspicious patterns are detected using predefined rules and typologies |
| 4 | **Claim Generation** | A structured claim object is built (subject, behavior, evidence, detection logic) |
| 5 | **Narrative Generation** | The claim plus retrieved regulatory context is used to draft the SAR narrative |
| 6 | **OMEGA Finalization** | Narrative and audit trail are validated, retained and stored in PostgreSQL |

### Core Outputs

- A structured **claim object**
- A **draft SAR narrative** (exportable as PDF)
- A **regulatory readiness assessment**
- A **final filing bundle**
- A **complete audit trail**

## Project Structure

```text
SARAA/
├── data-ingestion/        # Synthetic data generator + sample alerts, cases, KYC, transactions
├── rule based engine/     # Rule engine, fraud models, claim evidence generator, audit service
├── claim_gen/             # Claim object schema, population, storage and integrity hashing
├── rag-pipeline/          # Embeddings, ChromaDB stores, retrieval + narrative generation (Streamlit)
│   ├── docs/              #   Guidance, SAR templates and historical narratives (chunked)
│   └── chroma_db/         #   Persisted vector stores: guidelines, templates, sars
├── backend/               # Shared DB layer, models and the OMEGA finalization stage
│   ├── db/                #   SQLAlchemy base, session and init_db.py
│   ├── models/            #   AuditLog, SARFinalFiling
│   └── omega/             #   10-point regulatory validation + filing storage
├── audit-trail-system/    # Standalone FastAPI audit service, scripts and tests
├── docker-compose.yml     # PostgreSQL 15 for local development
└── .env.example           # Environment variable template
```

## Tech Stack

| Layer | Technology |
|---|---|
| Language | Python 3.11 |
| API | FastAPI, Uvicorn |
| Data & validation | SQLAlchemy 2.0, Pydantic 2 |
| Database | PostgreSQL 15 (Docker) |
| RAG | LangChain, ChromaDB, Sentence-Transformers (`all-MiniLM-L6-v2`, `e5-base-v2`) |
| LLM | Ollama (currently `gemma3`; swappable, e.g. Llama 3.1) |
| UI / export | Streamlit, ReportLab |
| Test data | Faker |

## Getting Started

### Prerequisites

- Python 3.11+
- Docker and Docker Compose
- [Ollama](https://ollama.com) with a pulled model (e.g. `ollama pull gemma3`)
- Git

### 1. Clone and configure

```bash
git clone https://github.com/nudaadeshmukh/SARAA.git
cd SARAA
cp .env.example .env        # Windows PowerShell: Copy-Item .env.example .env
```

Defaults in `.env` (edit as needed):

| Variable | Default | Purpose |
|---|---|---|
| `POSTGRES_USER` / `POSTGRES_PASSWORD` | `user` / `password` | Database credentials |
| `POSTGRES_DB` | `sar_audit` | Database name |
| `POSTGRES_HOST` / `POSTGRES_PORT` | `localhost` / `5432` | Database location |
| `DATABASE_URL` | `postgresql://user:password@localhost:5432/sar_audit` | SQLAlchemy connection string |
| `DEBUG`, `ENVIRONMENT` | `False`, `development` | App settings |

> Change the default credentials before using this anywhere beyond local development.

### 2. Start PostgreSQL and create tables

```bash
docker compose up -d
python backend/db/init_db.py
```

### 3. Install dependencies

```bash
python -m venv venv
source venv/bin/activate    # Windows: venv\Scripts\activate

pip install -r audit-trail-system/requirements.txt
pip install -r rag-pipeline/requirements.txt
pip install -r data-ingestion/requirements.txt
```

## Usage

### Generate sample data

```bash
cd data-ingestion
python sample_data_generator.py
```

Produces synthetic `customers`, `accounts`, `kyc`, `transactions`, `alerts` and `cases` JSON files in `data-ingestion/sample-data/`, covering money-laundering and fraud scenarios (layering, structuring, mule accounts, account takeover, and more).

### Build the vector stores (optional)

Pre-built ChromaDB stores are included in `rag-pipeline/chroma_db/`. To rebuild them from the chunked documents:

```bash
python rag-pipeline/data-ingestion/embeddings.py
```

> `embeddings.py` currently contains absolute Windows paths (`CHROMA_DB_PATH`, `CHUNKS_FOLDERS`). Update them to your local checkout first.

### Generate a narrative

```bash
cd rag-pipeline
streamlit run retrieval.py
```

The app retrieves relevant guidance, templates and similar historical SARs, then drafts a narrative with the local LLM. Sample input is in `rag-pipeline/queries/sample-input.txt`.

### Finalize and store a filing

The OMEGA stage (`backend/omega/omega_finalization.py`) takes a claim object and its narrative, runs the validation checks, and writes the result to the `sar_final_filings` table.

## Audit Trail API

The standalone audit service in `audit-trail-system/` exposes a FastAPI router:

| Method | Endpoint | Description |
|---|---|---|
| `GET` | `/logs` | List audit logs (filterable) |
| `GET` | `/logs/{audit_id}` | Full detail for one audit record |
| `GET` | `/case/{case_id}/history` | All events for a case |
| `GET` | `/case/{case_id}/sar-trail` | SAR-specific trail for a case |
| `GET` | `/sar/filing/{filing_number}` | Look up by SAR filing number |
| `GET` | `/user/{user_id}/activity` | Activity for a given user |
| `GET` | `/stats` | Aggregate audit statistics |
| `GET` | `/search` | Search audit logs |
| `GET` | `/compliance/report` | Compliance report |

See [`audit-trail-system/README.md`](audit-trail-system/README.md) and [`audit-trail-system/GETTING_STARTED.md`](audit-trail-system/GETTING_STARTED.md) for setup, seeding (`scripts/setup_audit_db.py --seed`) and querying (`scripts/query_audit_trail.py`).

## Auditability

Auditability is a core design goal. Every transformation step is tracked:

- Raw input capture
- Intermediate normalization
- Rule matches and rule execution results
- Evidence mapping
- Narrative generation inputs
- Final approval and storage

Claim objects also carry a generation trace and integrity hashes, so any change after creation is detectable.

## Regulatory Readiness Checks

Before a filing is stored, OMEGA runs a 10-point validation. `regulatory_ready` is `True` only if all checks pass.

| # | Check |
|---|---|
| 1 | Claim object is valid |
| 2 | Narrative is not empty |
| 3 | Narrative meets minimum length |
| 4 | Case ID is present |
| 5 | Alert IDs are present |
| 6 | Subject information is complete |
| 7 | Risk assessment is present |
| 8 | Suspicious patterns are documented |
| 9 | Evidence set is present |
| 10 | Integrity hashes are valid |

## Human Review

SARAA is **not** a fully autonomous filing system. It generates a strong, evidence-backed draft; a human reviewer then edits the narrative, approves or rejects it, adds supporting notes and confirms filing readiness.

## Testing

Tests for the audit trail system live in `audit-trail-system/tests/` (unit and integration):

```bash
cd audit-trail-system
pytest                       # configured via pytest.ini
pytest --cov                 # with coverage
```
