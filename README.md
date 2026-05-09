# SARAA

SARAA is an AI-powered Suspicious Activity Report narrative generation system built for banking compliance workflows. It converts transaction alerts, customer KYC, account activity, and rule-engine outputs into regulator-ready SAR narratives with full auditability, traceability, and human review support.

---

# Overview

Financial crime compliance teams spend significant time drafting SAR narratives manually, often stitching together fragmented data from alerts, case systems, KYC records, transaction histories, and investigative notes.

SARAA reduces that effort by structuring the pipeline into auditable stages:

- Ingestion
- Enrichment
- Rule evaluation
- Claim generation
- Narrative drafting
- Final compliance validation

The system was designed as a hackathon-grade but production-minded solution for the Barclays Hack-O-Hire challenge. It emphasizes transparency, explainability, and end-to-end traceability so that every generated narrative can be defended during internal review or regulatory scrutiny.

---

# Problem Statement

SAR filing is mandatory whenever banks detect suspicious activity that may indicate money laundering, fraud, or related financial crime.

However, drafting the narrative is usually:

- Manual
- Slow
- Inconsistent

Analysts must interpret complex transaction patterns, connect them to customer context, and write a clear report that meets regulatory expectations.

SARAA addresses this by:

- Reducing manual narrative drafting effort
- Preserving complete audit trails for every transformation
- Producing structured claim objects that map evidence to suspicious patterns
- Supporting human review and approval before filing

---

# Key Features

- Automated SAR narrative generation from alerts and transaction data
- Claim object creation with rich metadata, suspicious pattern summaries, and evidence references
- Full audit trail and lineage tracking across pipeline stages
- Rule-based and model-assisted detection logic
- RAG-based regulatory context retrieval for narrative support
- Final regulatory readiness validation before output
- Human-in-the-loop review and approval workflow
- PostgreSQL-backed persistence for claims, audit logs, and final filing artifacts

---

# Pipeline Architecture

SARAA is organized as a modular pipeline:

## 1. Ingestion and Normalization

Transaction alerts, customer KYC, account data, and case details are collected and standardized.

## 2. KYC Enrichment

Customer risk profile, account history, and related contextual signals are attached.

## 3. Rule Engine

Suspicious patterns are detected using predefined rules and typologies.

## 4. Claim Generation

A structured claim object is created containing:

- Subject context
- Suspicious behavior
- Evidence references
- Detection logic

## 5. Narrative Generation

The claim object is used to draft a SAR narrative in a regulatory format.

## 6. OMEGA Finalization

The final narrative and audit trail are checked for readiness, retained, and stored in PostgreSQL.

---

# Core Outputs

SARAA produces:

- A structured **claim object**
- A **draft SAR narrative**
- A **regulatory readiness assessment**
- A **final filing bundle**
- A **complete audit trail** for traceability and review

---

# Tech Stack

- Python
- FastAPI
- SQLAlchemy
- Pydantic
- PostgreSQL
- LangChain
- Llama 3.1

---

# How It Works

## 1. Input Collection

SARAA receives:

- Transaction alerts
- Customer KYC data
- Account and counterparty information
- Rule engine outputs
- Fraud or risk model scores
- Regulatory context references

---

## 2. Claim Generation

The claim generation stage converts the incoming data into a structured object containing:

- Case metadata
- Subject and account details
- Suspicious pattern descriptions
- Evidence set references
- Detection logic
- Risk assessment
- Regulatory hooks
- Generation trace
- Integrity hashes

---

## 3. Narrative Drafting

The structured claim is used to generate a SAR narrative that explains:

- What happened
- Why the activity is suspicious
- Which patterns or typologies match
- Which evidence supports the conclusion

---

## 4. Finalization

The narrative and audit trail are validated for:

- Regulatory completeness
- Evidence traceability
- Human approval readiness
- Retention and storage requirements

---

# Auditability

A core design goal of SARAA is complete auditability.

Every transformation step is tracked, including:

- Raw input capture
- Intermediate normalization
- Rule matches
- Evidence mapping
- Narrative generation inputs
- Final approval and storage

This ensures the system is defensible and transparent during compliance review.

---

# Regulatory Readiness

Before a SAR narrative is finalized, SARAA performs a readiness check that verifies:

- Narrative completeness
- Customer identification
- Transaction chronology
- Evidence resolvability
- Jurisdiction-specific formatting
- Human approval presence
- No leakage of sensitive data

---

# Human Review

SARAA is not a fully autonomous filing system.

It is designed to support analysts by generating a strong draft, after which a human reviewer can:

- Edit the narrative
- Approve or reject the draft
- Add supporting notes
- Confirm filing readiness

---

# Why SARAA Matters

SARAA helps compliance teams move faster without sacrificing defensibility.

It improves:

- Consistency
- Auditability
- Evidence traceability
- Operational efficiency

Instead of working from fragmented source systems, reviewers receive a structured evidence-backed narrative workflow.

For a hackathon project, SARAA demonstrates a strong combination of:

- AI
- Data engineering
- Compliance logic
- System architecture
- Explainability-first design

---

# Future Enhancements

Possible next steps include:

- Real-time alert triage
- Better typology classification
- Network graph visualization of counterparties
- Analyst feedback learning loop
- Multi-jurisdiction filing templates
- Automated redaction and privacy controls
