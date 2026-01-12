# Invoice-automation-n8n-ai

End-to-end automation workflow for processing invoice emails, extracting structured data from PDF attachments using AI, validating financial fields, and generating actionable outputs such as spreadsheets and alerts.

This project demonstrates how AI-driven document processing can be safely integrated into production workflows using defensive validation, business rules, and event-driven automation.

---

## Overview

Manual invoice processing is slow, error-prone, and difficult to scale.  
This workflow automates the entire pipeline:

1. Detect incoming invoice emails
2. Extract PDF attachments
3. Parse invoice data using AI
4. Validate and normalize critical fields
5. Log structured results
6. Trigger alerts for high-value invoices or errors

The system is designed with **reliability, auditability, and scalability** in mind.

---

## Workflow Architecture

**Trigger → Extract → AI Parse → Normalize → Persist → Alert**

### High-level Flow

- **Gmail Trigger**
  - Monitors inbox for incoming emails with PDF attachments
  - Supports multiple attachments per email

- **Attachment Splitter**
  - Separates each PDF into an independent processing unit
  - Enables batch processing from a single email

- **PDF Text Extraction**
  - Converts invoice PDFs into raw text for downstream processing

- **AI Invoice Parsing**
  - Uses a GPT-based model with strict rules to extract structured invoice data
  - Returns JSON only (no explanations or free text)

- **Normalization & Validation**
  - Cleans numeric formats (EU / LATAM / US)
  - Verifies required fields
  - Flags inconsistencies or incomplete invoices
  - Preserves raw AI output for audit purposes

- **Persistence**
  - Logs results into Google Sheets for reporting and traceability

- **Alerting**
  - Sends notifications for:
    - High-value invoices (> $5,000)
    - Parsing or validation errors

---


## Validation & Error Handling

The workflow does **not trust AI output blindly**.

Implemented safeguards include:
- JSON boundary detection and parsing checks
- Field presence validation (vendor, invoice number, date, amount)
- Amount sanity checks for multi-page invoices
- Status classification:
  - `OK`
  - `INCOMPLETE`
  - `ERROR`

All raw AI responses are stored for debugging and auditing.

---

## Outputs

- **Google Sheets**
  - Timestamped invoice records
  - Normalized financial fields
  - Status and error messages
  - Raw AI JSON snapshot

- **Alerts**
  - Slack notifications for high-value invoices
  - Email alerts for critical cases

---

## Technologies Used

- **Automation:** n8n
- **AI / LLM:** OpenAI (GPT-based models)
- **Email Integration:** Gmail API
- **Document Parsing:** PDF text extraction
- **Data Storage:** Google Sheets
- **Notifications:** Slack Webhooks, Gmail
- **Logic:** JavaScript (custom code nodes)


