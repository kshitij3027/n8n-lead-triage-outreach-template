# n8n Lead Triage + Outreach Template

This workflow helps you **score CRM leads, draft personalized outreach emails with an LLM, and log every action in an audit sheet**. It’s designed to be a plug-and-play template: bring your own Google Sheets, email credentials, and LLM key, and you’re ready to go.

---

## 🚀 Features

* **Lead import from Google Sheets** (`data_crm_leads` tab)
* **Custom scoring & filtering logic** (adjustable thresholds, states, and statuses)
* **AI-generated outreach email** using OpenRouter (or any LLM provider)
* **Email sending via Gmail or SMTP** (disabled by default for safety)
* **Audit logging** of every action in a separate Google Sheet (`data_audit_log` tab)

---

## 🛠️ Prerequisites

* n8n (Cloud or self-hosted)
* Credentials you’ll need to set up after import:

  * Google Sheets OAuth2 (or Service Account)
  * Gmail OAuth2 **or** SMTP server
  * LLM provider (OpenRouter API key, OpenAI, Anthropic, etc.)

---

## 📊 Google Sheets Setup

### 1. Leads Sheet (`data_crm_leads`)

Create a sheet tab with at least these columns:

```
lead_id | first_name | status | score_hint | created_at | lead_source | state | email | phone | language_pref | product_interest
```

### 2. Audit Log Sheet (`data_audit_log`)

Create another sheet tab with these headers:

```
event_id | ts | actor | entity_type | entity_id | action | details
```

---

## ⚙️ Setup Instructions

1. Import [`n8n_lead_triage_template_sanitized.json`](./n8n_lead_triage_template_sanitized.json) into your n8n instance.
2. In the **Read Leads** node → set your Google Sheet URL for the leads sheet.
3. In the **Append Audit** nodes → set your Google Sheet URL for the audit log.
4. Add Google Sheets credentials and link them to those nodes.
5. Configure the **OpenRouter Chat Model** (or replace with your LLM of choice).
6. Configure the **Email: Send** node with Gmail/SMTP.

   * Node is **disabled by default** to prevent accidental sends. Enable only after testing.

---

## ⚖️ Customization

* **Scoring logic** (in the “Lead Triage Agent” *Code* node):

  * `scoreThreshold` (default: 50)
  * `actionableStatus` (default: `['new','contacted']`)
  * `allowedStates` (default: `['CA','WA','OR']`; set `[]` to disable filtering)
* **Outreach prompt** (in the “Outreach Agent” node): tweak tone, style, or JSON structure.
* **Skipped leads**: currently only logged; you can re-route them to Slack, another sheet, or a CRM.

---

## 🔄 Workflow Overview

1. **Manual Trigger** → Read leads from `data_crm_leads`
2. **Lead Triage Agent** scores & flags each lead
3. **Append Audit (scored)** logs results
4. **If selected →** Outreach Agent → Build Email Payload → Merge → Send Email (disabled by default) → Append Audit (email sent)
5. **Else →** Append Audit (skipped)

---

## 📸 Example Screenshot

*(Optional: add a screenshot of your workflow canvas here)*

---

## 📥 Import Directly

You can import this workflow directly via the raw JSON:

```
https://raw.githubusercontent.com/<your-username>/n8n-lead-triage-outreach-template/main/n8n_lead_triage_template_sanitized.json
```

---

## 📜 License

[MIT](./LICENSE) — feel free to fork, adapt, and improve.

---

Would you like me to also generate a **LICENSE file (MIT template)** so your repo looks polished and others know they can safely reuse your template?
