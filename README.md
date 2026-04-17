# n8n Automation Portfolio

Production-grade automation workflows built with **n8n**, **Claude AI**, and modern business APIs.  
Self-hosted on VPS (Ubuntu · Docker · Nginx · Cloudflare SSL).

---

## Workflows

### 1. CRM & AI Proposal Automation
**File:** `crm-proposal-automation-FINAL.json`

Fully automated lead handling pipeline — from form submission to personalised proposal in under 23 seconds, with zero manual effort.

**What it does:**
1. Receives lead data via Webhook (Google Form → Apps Script → n8n)
2. Normalises and logs the lead to Google Sheets (LeadsRegistry)
3. Creates or updates a contact in HubSpot CRM (upsert logic, no duplicates)
4. Creates a linked deal in HubSpot with budget and stage
5. Saves CRM IDs back to Google Sheets
6. Sends lead data to **Claude API** → generates a personalised proposal email
7. Copies a Google Docs proposal template and fills all placeholders automatically
8. Shares the document with the lead's email (Google Drive API)
9. Saves proposal URL to Google Sheets
10. Sends the email + proposal link via Gmail
11. Updates `email_status = sent` in Google Sheets

**Stack:**
`n8n` · `Claude API (claude-sonnet-4-5)` · `HubSpot API` · `Google Sheets API` · `Google Docs API` · `Google Drive API` · `Gmail API` · `Webhook`

**Result:** Lead captured → CRM updated → personalised proposal sent in **23 seconds**, fully automated.

---

### 2. AI News Digest → Telegram
**File:** `ai-digest-telegram-FINAL.json`

Daily automated AI news digest — aggregates, summarises, and delivers top AI news to a Telegram channel every morning.

**What it does:**
1. Triggers daily at 9:00 AM (cron schedule)
2. Fetches top AI news articles from the last 24h via **Tavily Search API**
3. Sends articles to **Claude AI** via LangChain LLM Chain
4. Claude formats a structured digest: summary, top-5 news with emoji tags, source links, daily trend
5. Delivers the formatted digest to a Telegram channel

**Stack:**
`n8n` · `Claude AI (Anthropic)` · `Tavily Search API` · `Telegram Bot API` · `LangChain LLM Chain` · `Cron Scheduler`

**Result:** 365 days/year, zero manual effort, always-fresh AI news digest delivered automatically.

---

### 3. Service Job Management — ClickUp Sync
**File:** `service-job-sync-FINAL.json`

Automated bi-directional sync between a Google Sheets job registry and ClickUp. New jobs are picked up every 15 minutes and pushed to ClickUp as tasks — with name, priority, due date, and description mapped automatically.

**What it does:**
1. Schedule trigger fires every 15 minutes
2. Reads all jobs from Google Sheets (JobsRegistry)
3. Filters only new jobs where `pm_task_id` is empty (idempotent — no duplicates)
4. Creates a task in ClickUp via API with name, description, due date (Unix ms), and priority
5. Writes ClickUp task ID and sync timestamp back to Google Sheets

**Stack:**
`n8n` · `Google Sheets API` · `ClickUp API` · `JavaScript (Code node)` · `Schedule Trigger`

**Result:** Zero manual task creation. Jobs appear in ClickUp automatically within 15 minutes of being added to the sheet.

---

### 4. Service Job Escalation Alerts → Telegram
**File:** `service-job-escalation-FINAL.json`

Automated escalation workflow — scans for overdue jobs every 15 minutes and fires a Telegram alert with full job details when a job has been "In Progress" for over 24 hours.

**What it does:**
1. Schedule trigger fires every 15 minutes
2. Reads all jobs from Google Sheets (JobsRegistry)
3. JavaScript filter: keeps only jobs where `status = "In Progress"` AND age > 24 hours
4. Sends a formatted Telegram alert per overdue job — job ID, client, type, address, assignee, due date, notes

**Stack:**
`n8n` · `Google Sheets API` · `Telegram Bot API` · `JavaScript (Code node)` · `Schedule Trigger`

**Result:** Overdue jobs never go unnoticed. Alerts fire automatically with full context — no manual monitoring required.

---

## Infrastructure

All workflows run on a self-hosted n8n instance:

```
VPS (Hetzner Ubuntu 24.04)
└── Docker Compose
    └── n8n container
        ├── Nginx reverse proxy
        ├── Cloudflare SSL (Let's Encrypt)
        └── OAuth2 credentials (Google Workspace, HubSpot)
```

**Domain:** `n8n.guseynov.online`

---

## How to Import

1. Open your n8n instance
2. Go to **Workflows** → **Import from file**
3. Select the `.json` file
4. Add your own credentials (API keys, OAuth tokens)
5. Activate the workflow

> **Note:** All sensitive values (API tokens, chat IDs, Sheet IDs) have been removed from the exported JSON files. You will need to reconnect your own credentials after import.

---

## More Projects

Coming soon:
- `ai-sales-outreach.json` — Lead scoring + personalised outreach pipeline
- `contract-esignature.json` — Contract generation + approval workflow
- `video-call-summary.json` — Transcription + AI summary + clip ideas

---

## About

Built by **Parvin Huseynov** — AI Engineer & Automation Architect  
[linkedin.com/in/parvin-huseynov](https://linkedin.com/in/parvin-huseynov)
