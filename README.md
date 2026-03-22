# Prospect Research

An AI-powered prospect research and outreach system. Company intelligence, competitor analysis, strategy generation, email drafting, and sending — all automated through n8n workflows with a Next.js frontend.

---

## System Architecture

### n8n Workflow

<!-- DRAG & DROP your n8n workflow screenshot below this line -->


### System Flow Diagram

<!-- DRAG & DROP your Excalidraw flow diagram screenshot below this line -->


**[Open Interactive Diagram on Excalidraw](https://excalidraw.com/#json=d45JxtqejWDwfiKHFHESC,tJaq6vHoRAHmx6eYsYb2ZQ)**

---

## How It Works

```
Frontend (Next.js) → n8n Webhook → Switch Node → 5 Routes → Airtable → Response → Frontend
```

| Flow | Action | What It Does |
|------|--------|--------------|
| **1 — Company Search** | `research` | Filter Fields → Apify LinkedIn Scraper → Save to Airtable |
| **2 — Competitor Research** | `competitor_research` | Search Records → Company Analysis Agent → Structure Output → Competitor Analysis Agent |
| **3 — Strategy Generation** | `generate_strategy` | 3 chained AI agents: Complete Analysis → Pain Points + Solutions → Strategy Hooks |
| **4 — Email Drafting** | `draft_email` | Get Strategies + Company Data → Email Drafting Agent (Claude) → Save to Email Log |
| **5 — Email Sending** | `send_email` | Email Send Agent (Claude) → Gmail Send Tool → Save message_id + SENT status |

### Data Layer — Airtable (3 Tables)

| Table | Stores |
|-------|--------|
| **Company Brief** | Basic company info from LinkedIn scrape |
| **Company Data** | Full research + competitor analysis |
| **Strategy Data + Email Log** | Strategies, drafted emails, send status, message IDs |

---

## n8n Workflows

The automation layer lives in **n8n** (self-hosted). The workflow handles all 5 action routes through a single webhook entry point.

**Key integrations used in n8n:**
- **Apify** — LinkedIn company scraping
- **Firecrawl** — Website scraping + competitor research
- **OpenAI / Claude** — AI agents for analysis, strategy, email drafting
- **Gmail OAuth2** — Sending emails
- **Airtable** — Data storage (3 tables)
- **Wait + Retry nodes** — Self-healing on token limit errors

> n8n workflows are managed through the n8n editor UI. The webhook URLs are configured in `.env.local`.

---

## What's Built

### Frontend Interface
- Multi-page app — Home, Research (`/scrape`), Database (`/database`)
- Dark / Light theme toggle with neumorphic design system
- Responsive drawer panel for viewing company records

### Company Research
- Add company by name + LinkedIn URL
- Research triggers automatically on add
- Data pulled from Airtable via n8n webhook and stored locally
- Fields: Company Name, Universal Name, Tagline, Description, Employee Count, LinkedIn URL, Website URL, Industries, Specialities, Locations, Phone Number

### Competitor Research
- One-click competitor research trigger per company
- Sends `action: competitor_research` + Interface ID to n8n webhook

### Strategy & Email Pipeline
- One-click strategy generation (3 chained AI agents)
- One-click email draft (Claude-powered, signs as Abhay)
- One-click email send (Gmail OAuth2, tracks message_id)
- Full pipeline: **Research → Analyze → Strategize → Draft → Send**

### Database
- `localStorage` with stable UUID Interface IDs
- Search + status filters (Researched / Drafted / Sent)
- Two-click delete confirmation
- Stats bar with totals per status

### API Routes
- `/api/research` — forwards to n8n research webhook
- `/api/competitor-research` — forwards to n8n competitor webhook
- Dev mode mocks when webhooks are not configured

---

## Stack

| Layer | Tech |
|-------|------|
| **Frontend** | Next.js 16 (App Router), TypeScript, Tailwind CSS |
| **Storage** | localStorage (client), Airtable (server) |
| **Automation** | n8n (5 webhook routes) |
| **AI** | Claude + OpenAI (via n8n AI agents) |
| **Scraping** | Apify (LinkedIn), Firecrawl (websites) |
| **Email** | Gmail OAuth2 |

---

## Setup

```bash
npm install
```

Create `.env.local`:
```env
N8N_RESEARCH_WEBHOOK_URL=your_webhook_url
N8N_COMPETITOR_WEBHOOK_URL=your_webhook_url
```

```bash
npm run dev
```

Runs on `http://localhost:3010`

---

## Project Structure

```
prospect-research/
├── app/                    # Next.js App Router pages
│   ├── page.tsx            # Home
│   ├── scrape/page.tsx     # Research page
│   ├── database/page.tsx   # Database page
│   └── api/                # API routes (webhook bridges)
├── components/             # React components
├── lib/                    # Utilities, types, database layer
├── docs/                   # Architecture diagrams
└── public/                 # Static assets
```

