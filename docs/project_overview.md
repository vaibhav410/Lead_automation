# Project Overview — Lead Automation Workflow

## Executive Summary

The **automation leads** n8n workflow automates lead intake for marketing and sales teams. A single POST request to a webhook triggers storage in Google Sheets and an automated Gmail confirmation—eliminating manual data entry and delayed follow-up.

---

## Technical Architecture

### System Context

```
[Website / Form / Postman / CRM]
              │
              │  POST JSON
              ▼
      ┌───────────────┐
      │  n8n Webhook  │
      └───────┬───────┘
              │
              ▼
      ┌───────────────┐
      │ Google Sheets │  →  Lead Database (Sheet1)
      └───────┬───────┘
              │
              ▼
      ┌───────────────┐
      │     Gmail     │  →  Lead inbox
      └───────────────┘
```

### Workflow Metadata

| Field | Value |
|-------|--------|
| Workflow name | `automation leads` |
| Workflow ID | `dFtQLeGlq03OaNw4` |
| Status | Active (`active: true`) |
| Execution order | v1 |
| Node count | 3 |

---

## Node-by-Node Analysis

### 1. Webhook (`b896bff9-2911-4d1b-8e9b-f826fde0f4fa`)

**Purpose:** HTTP entry point for lead submissions.

| Configuration | Detail |
|---------------|--------|
| Method | POST |
| Path | `lead-form` |
| Webhook ID | `8cea0d64-14fd-4bc6-8716-418c2a17a898` |

**Behavior:** n8n exposes a URL that accepts JSON bodies. On each request, the workflow starts and passes the parsed body as `$json` to downstream nodes.

**Pinned test data (development):**

```json
{
  "Name": "Vaibhav",
  "Email": "vkkanojia079@gmail.com",
  "Phone": "9876543210"
}
```

---

### 2. Append row in sheet (`2cf21230-972d-4dfa-b218-35f26b060d8d`)

**Purpose:** Persist lead records in a shared spreadsheet.

| Configuration | Detail |
|---------------|--------|
| Operation | append |
| Document | Lead Database (`1kHwU7qsSHlcy_YqcJxTEO0SBY838kjhrVyj1FFNPh-Q`) |
| Sheet | Sheet1 (`gid=0`) |
| Mapping mode | defineBelow |
| Credential | Google Sheets OAuth2 (`Google Sheets account`) |

**Column schema:**

| Column ID | Type |
|-----------|------|
| name | string |
| email | string |
| phone no  | string (note trailing space in column name) |

**Data flow:** Receives webhook output; appends one row; passes row data (including `email`) to Gmail.

**Reviewer note:** The exported JSON contains literal test values (`Vaibhav`, etc.) in the column mapping. For production, replace with expressions such as `={{ $json.body.Name }}` (exact path depends on webhook body structure). Screenshots confirm the pipeline works; dynamic mapping is recommended before live deployment.

---

### 3. Send a message (`b66ab7e9-8679-4138-8946-7d9b2083e0d6`)

**Purpose:** Send lead confirmation email.

| Configuration | Detail |
|---------------|--------|
| To | `={{ $json.email }}` |
| Subject | Lead Registration Confirmation |
| Type | text |
| Credential | Gmail OAuth2 (`Gmail account`) |

**Message content:**

> Hello, Thank you for your interest. We have successfully received your details and our team will contact you shortly. Best Regards, Earthonoid AI Automation Team

**Data dependency:** Uses `email` from the Google Sheets node output, not directly from the webhook—valid because Sheets returns the appended row fields.

---

## Connection Graph

```
Webhook ──main[0]──► Append row in sheet ──main[0]──► Send a message
```

Linear topology: no branches, merges, or loops. Simple and maintainable.

---

## Data Processing Logic

1. **Ingress** — Webhook receives JSON (field names may be PascalCase: `Name`, `Email`, `Phone`).
2. **Transform (implicit)** — Google Sheets node maps fields to sheet columns (`name`, `email`, `phone no`).
3. **Persistence** — Row appended to Lead Database.
4. **Egress** — Gmail reads `email` from previous node and sends confirmation.

**Gap to address:** Webhook field casing (`Name`) vs sheet columns (`name`) should be aligned via expressions or a Set node to avoid empty cells when payload shape changes.

---

## End-to-End Automation Flow

| Phase | Action | Evidence |
|-------|--------|----------|
| 1 | POST to webhook | Pin data / curl / form |
| 2 | Sheet row created | `google_sheet_updated.png` |
| 3 | Email sent | `gmail_confirmation.png` |
| 4 | Workflow success | `workflow_canvas.png` (green checks, 1 item per node) |

---

## Business Use Case

**Scenario:** A company runs a landing page for product demos. Each signup must be logged for the sales team and acknowledged instantly.

**Without automation:** Staff copy leads from email notifications into spreadsheets and send manual replies—slow and error-prone.

**With automation:** Every submission is stored and confirmed in seconds, 24/7.

---

## Benefits of Automation

- **Speed** — Sub-second processing vs hours of manual work  
- **Accuracy** — No typos from retyping emails or phone numbers  
- **Consistency** — Same confirmation message every time  
- **Visibility** — Sheet becomes single source of truth  
- **Extensibility** — Add Slack, CRM, or scoring nodes without rewriting infrastructure  

---

## Security & Operations Notes

- OAuth credentials are stored in n8n, not in the JSON export (credential IDs only).
- Webhook URL should be treated as sensitive; add auth for public forms.
- Spreadsheet ID is visible in export—acceptable for internship demo; restrict sheet permissions in production.

---

## Evidence Index

| Artifact | Location |
|----------|----------|
| Workflow export | `automation_leads.json` |
| Canvas screenshot | `screenshots/workflow_canvas.png` |
| Sheet screenshot | `screenshots/google_sheet_updated.png` |
| Email screenshot | `screenshots/gmail_confirmation.png` |
| Demo script | `demo-video/demo_script.md` |
| Demo recording | [Google Drive — Screen Recording](https://drive.google.com/file/d/1jTqRmZyaEI6GLbaSTA4uZdTyfU998jhe/view?usp=sharing) |

---

*Document version: 1.0 — Earthonoid internship submission*
