# NexusCS — AI-Powered Customer Success Operations Platform

> **Live Demo → [kira-billion.github.io/nexuscs](https://kira-billion.github.io/nexuscs)**

A fully functional, single-file CS Ops platform built to demonstrate how AI automation, ML classification, and RAG-based support can replace manual L1 operations and give CS leaders real-time visibility into SLA health, churn risk, and team performance.

Built by **Bitan Basu** — AI CX Automation Specialist with 8 years of enterprise CS Operations experience at HP Inc. and Replicon (Deltek).

---

## What It Does

NexusCS solves two problems observed across every support organisation I worked in:

1. **Managers flying blind** — no real-time visibility into SLA health, churn risk, or which clients need immediate attention
2. **Agents overwhelmed by L1 tickets** — repetitive queries that AI can resolve, freeing agents for complex work

The platform bridges ML model output with CS operational workflow — translating a sentiment score or churn signal into a routing decision, escalation alert, or dashboard metric that a team can act on immediately.

---

## Live Demo

**URL:** [https://kira-billion.github.io/nexuscs](https://kira-billion.github.io/nexuscs)

**Login credentials for demo:**
- Email: `admin@nexuscs.io`
- Password: `nexus2025`

The demo loads with pre-populated sample data (25 tickets, 5 clients, 8 routing rules) so you can explore every module immediately.

---

## 12 Modules

| Module | What It Does |
|--------|-------------|
| **Dashboard** | Executive view — SLA compliance, open tickets by priority, churn risk clients, CSAT trend, sentiment alerts. VP-readable in 30 seconds. |
| **Ticket Queue** | Full ticket table with filters for priority, status, tier, category, and sentiment. Colour-coded sentiment badge per ticket. CSV export. |
| **Client Health** | Composite health score (0–100) per client. Factors in open tickets, SLA breaches, CSAT, NPS, and ML sentiment weighting. Flags churn risk before the client voices it. |
| **Routing Rules** | 8 configurable auto-assignment rules based on client tier, priority, category, and keyword triggers. Mirrors Freshdesk automation and Zendesk trigger logic. |
| **Workflows** | Visual workflow builder — SLA breach alert, churn risk notification, onboarding sequence, escalation loop. Shows trigger → condition → multi-step action chain. |
| **AI Triage** | Real-time ticket classifier. Paste any support ticket text → receive category, priority prediction, sentiment score (0–1), escalation recommendation, and suggested first response instantly. |
| **SLA Tracker** | Per-ticket SLA compliance with ring charts by priority tier. Breach risk flagged in real time. Exports SLA compliance report. |
| **Analytics** | Aggregated metrics — volume by category, CSAT distribution, AHT trends, agent workload balance, and automation savings estimate in hours/month. |
| **Import Data** | CSV upload accepting exports from Zendesk, Freshdesk, Salesforce, and HubSpot. Auto-scores imported tickets for sentiment on load. |
| **API Docs** | Integration guide documenting webhook patterns, REST endpoints, and connector logic for Zendesk, Freshdesk, JIRA Service Management, and Salesforce. |
| **Ops Assistant (AI)** | RAG-grounded conversational assistant. Answers natural-language queries across all platform data — tickets, clients, SLA, sentiment. Context-aware suggested prompts. |
| **ML Pipeline (LIVE)** | 5-step visual architecture diagram with live stats — tickets scored, sentiment breakdown, average score, worst-performing client, sentiment-priority alignment rate. Escalation log with recommended actions. |

---

## ML & AI Architecture

```
Ticket Created
      ↓
Sentiment Classifier (JS keyword engine · API-ready)
      ↓
Score Router (0.0–1.0 threshold rules)
  Stable (0.0–0.3)   → Normal queue
  Monitor (0.4–0.6)  → Flag for review
  At Risk (0.7–0.85) → CSM notification
  Escalate Now (0.86+) → Immediate alert + priority override
      ↓
Action Trigger (Slack alert · priority bump · CSM notify · health score penalty)
      ↓
Dashboard Update (Health score · Sentiment heatmap · SLA feed · ML Pipeline stats)
```

**Why keyword-based and not a live API call?**
The classifier runs entirely in-browser with no backend — making it instantly deployable as a demo. The architecture is identical to a live OpenAI API sentiment call; swapping the scoring function for an API call is a one-hour integration task. The ML Pipeline module documents exactly how that connection works.

**Health Score Formula:**
```
Base health score (open tickets + SLA + CSAT + NPS)  ×  70%
Sentiment penalty/bonus                               ×  30%
  avg sentiment > 0.86 → subtract 20 points
  avg sentiment > 0.70 → subtract 10 points
  avg sentiment < 0.40 → add 5 points
```

---

## Tech Stack

| Layer | Technology |
|-------|-----------|
| Frontend | Vanilla HTML · CSS · JavaScript (single file) |
| AI / NLP | OpenAI API architecture (RAG pattern) · keyword sentiment engine |
| Data | In-browser JSON store · CSV import/export |
| Hosting | GitHub Pages (zero backend, zero cost) |
| Workflow reference | Make.com / Zapier connector patterns documented in API Docs module |
| Integrations | Zendesk · Freshdesk · Salesforce · HubSpot (CSV import + API docs) |

**No backend. No database. No API keys required to run the demo.**
Everything runs in the browser. For production deployment, the RAG bot and sentiment engine connect to OpenAI via a single API key configured in the settings panel.

---

## Business Case

| Metric | Value |
|--------|-------|
| Ticket deflection (RAG bot simulation) | **65%** |
| Agent time saved per month (5-agent team) | **~47 hours** |
| Estimated labour cost saved (at ₹6 LPA/agent) | **₹6–12L/month at scale** |
| SLA compliance demonstrated (Replicon, live) | **98%** |
| AHT reduction (HP Inc., Python workflows) | **25%** |

---

## Enterprise Deployment Path

NexusCS is a prototype that maps directly to enterprise tooling:

```
NexusCS prototype          →    Enterprise equivalent
─────────────────────────────────────────────────────
Keyword sentiment engine   →    OpenAI API / HuggingFace model
In-browser routing rules   →    Freshdesk Automations / Zendesk Triggers
Workflow builder           →    Make.com Scenarios / Zapier Zaps
CSV import                 →    Live webhook from Freshdesk / Zendesk
Ops Assistant (RAG)        →    Freshworks Freddy AI / Salesforce Agentforce
ML Pipeline dashboard      →    Power BI / Tableau connected to live data
```

---

## About the Builder

**Bitan Basu** — AI CX Automation Specialist

8 years of enterprise CS Operations experience:
- **HP Inc. (2016–2023):** L2 Team Captain, 12-agent team, Python automation, 25% AHT reduction, 50+ agents mentored
- **Replicon/Deltek (2023–2024):** Fortune 500 accounts, 98% SLA compliance, C-suite escalation management

NexusCS was built in 2025 to demonstrate the bridge between ML/data science output and CS operational workflow — the function most organisations cannot fill from either side of the fence.

- 📧 bitan1basu@gmail.com
- 💼 [LinkedIn](https://linkedin.com/in/bitan-basu)
- 🐙 [GitHub](https://github.com/KIRA-Billion)

---

## How to Run Locally

No installation needed.

```bash
# Option 1 — just open the file
# Download index.html and double-click it in your file manager
# Opens directly in any browser

# Option 2 — clone and serve
git clone https://github.com/KIRA-Billion/nexuscs.git
cd nexuscs
# Open index.html in your browser
# Or run a local server:
python3 -m http.server 8080
# Then visit http://localhost:8080
```

---

## File Structure

```
nexuscs/
├── index.html        ← Entire platform (single file, ~150KB)
├── README.md         ← This file
├── .gitignore        ← Standard ignores
└── LICENSE           ← MIT License
```

---

*Built as a portfolio demonstration of ML-to-workflow bridge architecture for AI CX Automation roles.*
*Designed to connect with enterprise platforms — not replace them.*
