# Lead Intake & Engagement Automation

## Overview

This repository contains a **production-grade n8n automation workflow** that handles **end-to-end lead intake, validation, deduplication, persistence, and notifications** using a **database-first, idempotent architecture**.

The workflow is designed to replace manual operational processes with a **reliable, observable, and scalable automation system** suitable for real-world business use.

---

## System Design Diagram

Before implementation, the complete workflow was **designed visually** to map logic, edge cases, and failure paths.

**Miro Diagram (Design → Implementation Blueprint):**  
👉 https://miro.com/app/board/uXjVGGdxj_U=/?share_link_id=586176035239

The diagram represents:
- Trigger points
- Validation boundaries
- Conditional routing
- Deduplication logic
- Database operations
- Non-blocking notification paths

Each block in the diagram maps directly to an n8n node in the final implementation.

---

## Problem Statement

Manual lead-processing workflows often introduce:
- Delays due to human intervention
- Duplicate records across systems
- Inconsistent validation
- Missed or late notifications
- Poor visibility into failures

This automation was built to **eliminate those issues** by enforcing:
- Deterministic validation
- Database-level deduplication
- Idempotent processing
- Non-blocking side effects
- Clear execution flow and observability

---

## What This Workflow Does

1. Accepts lead submissions via an **n8n Form Trigger**
2. Validates and normalizes incoming data
3. Prevents duplicate processing using **PostgreSQL** as the system of record
4. Creates or updates contacts deterministically
5. Sends notifications via **Telegram** and **Email (SMTP)**
6. Logs workflow execution metadata for auditability
7. Ensures failures in notifications or logging never break the core flow

---

## Architecture Highlights

- **Database-First Design**  
  PostgreSQL acts as the single source of truth.

- **Idempotent Processing**  
  The same lead can be submitted multiple times without creating duplicates.

- **Non-Blocking Notifications**  
  Telegram and Email are treated as side effects and do not affect data integrity.

- **Explicit Validation Boundaries**  
  Invalid data is rejected early in the workflow.

- **Production-Safe Execution**  
  No fragile merges or implicit execution assumptions.

---

## Tech Stack

- n8n (Self-Hosted)
- PostgreSQL
- SMTP (Gmail App Password)
- Telegram Bot API
- Webhook / Form-based triggers

---

## Logical Workflow Structure

Form Trigger
↓
Normalize & Enrich Lead Data
↓
Validate Required Fields
↓
Lookup Existing Contact (PostgreSQL)
↓
IF Contact Exists?
├── Update Contact
└── Create Contact
↓
Telegram Notification
↓
Email Notification
↓
Execution Logging (PostgreSQL)

---

## Error Handling Philosophy

- Validation errors stop execution early
- Database constraints act as final safeguards
- Notification failures do **not** block execution
- Logging failures do **not** block execution
- The workflow is safe to retry

This approach prioritizes **data integrity and reliability** over aggressive retries.

---

## Workflow JSON

The complete **n8n workflow JSON** is included in this repository and can be:
- Imported directly into n8n
- Reviewed node-by-node
- Extended or adapted for other use cases

---

## About the Author

**Kishan Khodbhaya**  
Automation Engineer | Backend-Focused Problem Solver

Portfolio & background:  
👉 https://kishan-khodbhaya.github.io/

This portfolio includes:
- Production-grade automation systems
- Backend and integration work
- Real-world workflows from previous roles

⚠️ Due to NDA restrictions, JSON exports or screenshots from past client or employer systems cannot be shared publicly.  
This repository reflects the **same architectural principles, reliability standards, and execution quality** used in those production environments.

---

## How to Use This Repository

1. Import the workflow JSON into n8n
2. Configure:
   - PostgreSQL credentials
   - Telegram Bot credentials
   - SMTP credentials
3. Review the Miro diagram for architectural context
4. Test using sample form submissions

---

## Final Notes

This workflow is intentionally:
- Practical and production-oriented
- Interview-defensible
- NDA-compliant
- Easy to reason about and extend

It demonstrates **how real automation systems are designed and maintained**, not just how tools are connected.
