# NexusCS — AI-Powered Customer Success Operations Platform

> **Live Demo → [kira-billion.github.io/nexuscs](https://kira-billion.github.io/nexuscs)**

A fully functional, single-file CS Ops platform demonstrating how AI automation, ML classification, and RAG-based support can replace manual L1 operations — and give CS leaders real-time visibility into SLA health, churn risk, and team performance.

---

## What It Does

NexusCS addresses two structural problems found across enterprise support organisations:

1. **Managers flying blind** — no real-time visibility into SLA health, churn risk, or which clients need immediate attention
2. **Agents overwhelmed by L1 tickets** — repetitive queries that a classification engine can resolve or route without human triage

The platform bridges ML model output with CS operational workflow — translating a sentiment score or churn signal into a routing decision, escalation alert, or dashboard metric that a team can act on immediately.

---

## Live Demo

**URL:** [https://kira-billion.github.io/nexuscs](https://kira-billion.github.io/nexuscs)

**Login credentials for demo:**
- Email: `admin@nexuscs.io`
- Password: `nexus2025`

The demo loads with pre-populated sample data (25 tickets, 10 clients, 5 agents, 8 routing rules) so every module is explorable immediately. Upload your own CSV to see it run on real data.

---

## Role-Based Access System

NexusCS ships with four access roles that control module visibility and dashboard layout in real time — no reload required:

| Role | Access |
|------|--------|
| **All Access** | Full platform |
| **VP / Stakeholder** | Dashboard (executive cards), Client Health, SLA, Analytics, Ops Assistant, ML Pipeline |
| **CS Manager** | Everything except the VP-only revenue cards |
| **L1 Agent** | Dashboard, Ticket Queue, AI Triage, Ops Assistant |

Switch roles from the sidebar selector. The dashboard layout adapts automatically — VPs see Revenue at Risk and Automation ROI cards that are hidden for agents.

---

## 12 Modules

### Overview

| Module | What It Does |
|--------|-------------|
| **Dashboard** | Role-adaptive executive view — VP mode surfaces Revenue at Risk, Automation ROI (adjustable ₹/$ per hour rate), KPI row, Critical Tickets feed, Category bar chart, and Sentiment Heatmap by client. All in one screen. |
| **Ticket Queue** | Full ticket table with 6 simultaneous filters: Priority, Status, Client, Category, Agent, and Sentiment Label. Colour-coded sentiment badge per row. Paginated. CSV export. |
| **Client Health** | Composite health score (0–100) per client — dynamically adjusted by real-time average ticket sentiment for that account, not just a static CSV value. Flags churn risk before the client voices it. |
| **Routing Rules** | 8 configurable auto-assignment rules based on client tier, priority, category, and keyword triggers. Toggle on/off, edit inline, add new rules. Mirrors Freshdesk automation and Zendesk trigger logic. |
| **Workflows** | Visual workflow canvas with 3 pre-built automations. Includes a live **Workflow Simulator** that runs each workflow against currently loaded data and shows matched ticket/client counts and per-item outcomes. |
| **AI Triage** | Real-time ticket classifier. Paste any support ticket text → receive category, priority prediction, sentiment label, urgency position (0–100 scale), escalation recommendation, and suggested routing instantly. Urgency keywords (`urgent`, `blocking`, `production`, `entire team`) add +15 to the urgency position. One-click to push the triage result into the Create Ticket modal. |
| **SLA Tracker** | Per-priority SVG ring charts, breach count, compliance %, average resolution time, and at-risk critical ticket count. Exports SLA compliance report as CSV. |
| **Analytics** | Aggregated metrics — CSAT, NPS, MRR, churn risk count, volume by category, time-savings breakdown by automation type, and full agent performance table with capacity bars. |
| **Import Data** | Drag-and-drop or click-to-upload CSV for Tickets, Clients, and Agents separately. Template download for each. Row count confirmed on load. Sentiment scoring runs automatically on every imported ticket. |
| **API Docs** | 4-tab reference: Integration guide, REST endpoints (7 documented), Webhook patterns, and Hiring Guide. |
| **Ops Assistant (AI)** | RAG-grounded conversational assistant. Passes full live data context — tickets with sentiment labels, dynamically adjusted client health scores, MRR, ARR, agent capacity — to Claude API on each query. Multi-turn history. API key stored in browser memory only, never written to file. |
| **ML Pipeline (LIVE)** | Live stats across all loaded tickets — sentiment distribution, average score, worst client by average sentiment, and sentiment-priority alignment rate. Escalation log with a conditional action engine that fires different recommended actions based on score × priority × churn risk combinations. |

---

### Workflow Simulator

The Workflows module includes a live simulation engine — not just a static diagram. Click **▶ Simulate** on any workflow and it runs against your currently loaded data:

- **Critical Escalation** — matches all open Critical tickets, shows per-ticket routing path and outcome
- **SLA Breach Alert** — matches breached and at-risk tickets, shows agent notification chain
- **Onboarding Track** — matches all clients, shows CSM assignment, welcome email trigger, and Day 3/Day 14 health check outcomes

Result panel shows total matched items, steps evaluated, and per-item action trace.

---

### Conditional Action Engine (ML Pipeline)

The ML Pipeline escalation log does not simply surface high-sentiment tickets. It applies a layered decision matrix to determine the correct response:

```
Sentiment > 0.86 AND Priority = Critical  →  "Immediate CSM call + Executive loop-in"
Sentiment > 0.86 AND Priority ≠ Critical  →  "Override priority to Critical + notify CSM"
Sentiment > 0.70 AND Client Churn Risk = High  →  "Schedule EBR within 48 hours"
All other At Risk / Escalate Now          →  "Monitor — review in next daily standup"
```

This cross-signals three independent data sources (sentiment score, ticket priority, client churn risk) into a single recommended action — the same logic a senior CSM applies manually, made systematic.

---

### Smart Notifications Panel

Notifications are generated automatically from live data — not hardcoded:

- Every open Critical ticket fires a notification
- Every High Churn Risk client surfaces an alert with their current health score
- Every agent above 85% capacity triggers a workload warning
- Every SLA breach generates a notification with ticket ID and timestamp

---

## ML & AI Architecture

```
Ticket Created / Imported
         ↓
Sentiment Classifier (keyword-weight engine · 0.0–1.0 score)
         ↓
4-Tier Label Assignment
  Stable       (0.00–0.39)  → Normal queue
  Monitor      (0.40–0.69)  → Flag for review
  At Risk      (0.70–0.85)  → CSM notification
  Escalate Now (0.86–1.00)  → Immediate alert + conditional action
         ↓
Action Trigger (Conditional Engine — see above)
         ↓
Dashboard Update
  Health score adjustment · Sentiment heatmap · SLA feed · ML Pipeline stats
```

**Why keyword-based and not a live API call?**
The classifier runs entirely in-browser with no backend — making it instantly deployable. The architecture maps directly to a live embedding/classification API call; swapping the scoring function is a single-function replacement. The ML Pipeline module documents exactly where that integration point sits.

---

### Health Score Formula

Client health is not a static CSV field. It is recalculated dynamically by cross-signalling the base health score with average ticket sentiment for that account:

```
adjusted = base × 0.7 + (base + sentiment_adj) × 0.3

Sentiment adjustment values (applied to the 30% component):
  Average sentiment ≥ 0.86  →  adj = −20  (net effect: ~−6 pts on final score)
  Average sentiment ≥ 0.70  →  adj = −10  (net effect: ~−3 pts on final score)
  Average sentiment < 0.40  →  adj = +5   (net effect: ~+2 pts on final score)
```

Example: A client with base score 75 and average ticket sentiment ≥ 0.86:
`75 × 0.7 + (75 − 20) × 0.3 = 52.5 + 16.5 = 69` — a 6-point reduction reflecting live distress signals.

The adjustment is intentionally conservative — a single bad ticket week does not crater a client's score, but a sustained pattern of high-distress tickets will visibly depress it before the client escalates formally.

---

### Ops Assistant — RAG Context Construction

The Ops Assistant does not query a generic LLM. Before each API call it assembles a structured context string from live platform state:

- All tickets: ID, priority, status, client name, tier, category, agent, SLA hours, breach status, resolution time, CSAT, sentiment score and label
- All clients: ID, name, tier, base health score, **dynamically adjusted health score and sentiment delta**, churn risk, MRR, ARR, NPS, CSAT, open tickets, CSM, active users vs. total seats, last contact date
- All agents: capacity %, open tickets, closed MTD, avg response time, avg resolution hours, CSAT
- Platform summary: ticket totals, average sentiment, SLA compliance rate

This full context is passed with every query, making the assistant aware of the entire current state of the CS operation.

---

## Tech Stack

| Layer | Technology |
|-------|-----------|
| Frontend | Vanilla HTML · CSS · JavaScript (single file) |
| AI / NLP | Claude API (RAG pattern) · keyword sentiment engine |
| Data | In-browser JSON store · CSV import/export |
| Hosting | GitHub Pages (zero backend, zero cost) |
| Workflow reference | Make.com / Zapier connector patterns documented in API Docs module |
| Integrations | Zendesk · Freshdesk · Salesforce · HubSpot (CSV import + API docs) |

**No backend. No database. No API keys required to run the demo.**
Everything runs in the browser. The Ops Assistant requires an Anthropic API key entered at runtime — stored in browser memory only, cleared on tab close.

---

## How to Load Your Own Data

The fastest way to see NexusCS on real data:

1. Go to **Import Data** in the sidebar
2. Click **⇣ Download Template** for Tickets, Clients, or Agents
3. Fill in the CSV with your own data (or Zendesk/Freshdesk export)
4. Drag-and-drop the file onto the upload zone — or click to browse
5. All modules update immediately. Sentiment scoring runs automatically on every ticket row.

**Minimum viable load:** Tickets CSV only. All dashboard metrics, ML Pipeline, SLA Tracker, and AI Triage will populate. Client Health and Analytics gain additional depth when the Clients and Agents CSVs are loaded.

---

## Enterprise Deployment Path

NexusCS is a prototype that maps directly to enterprise tooling:

```
NexusCS prototype          →    Enterprise equivalent
─────────────────────────────────────────────────────
Keyword sentiment engine   →    OpenAI API / HuggingFace model
In-browser routing rules   →    Freshdesk Automations / Zendesk Triggers
Workflow builder           →    Make.com Scenarios / Zapier Zaps
Workflow Simulator         →    Staging environment regression test
CSV import                 →    Live webhook from Freshdesk / Zendesk
Ops Assistant (RAG)        →    Freshworks Freddy AI / Salesforce Agentforce
ML Pipeline dashboard      →    Power BI / Tableau connected to live data
Conditional action engine  →    PagerDuty escalation policy / JIRA automation
Health score fusion        →    Gainsight / Totango composite scoring
```

---

## How to Run Locally

No installation needed.

```bash
# Option 1 — just open the file
# Download index.html and open it in any browser

# Option 2 — clone and serve
git clone https://github.com/KIRA-Billion/nexuscs.git
cd nexuscs
python3 -m http.server 8080
# Visit http://localhost:8080
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
