# n8n AI Lead Generation Agents

Five n8n workflows that discover, verify, find contacts for, and email leads — automatically, on a schedule. Built for [WebEquipe PDF Search Pro](https://webequipe.com/pdf-search/), but the architecture is product-agnostic: swap the ICP list and product description, and it finds leads for anything.

Full write-up: **[sarwarhossain.com/blog/n8n-ai-lead-generation-agent](https://sarwarhossain.com)**

## The pipeline

```
1  Lead Discovery      → AI agent (Gemini + Perplexity + Google Search) proposes candidate sites
1.5 Site Verification  → plain code confirms each site is real (WordPress + PDFs), no AI
2  Email Finder        → Snov.io domain search + job-title scoring picks the best contact
2.5 Generic Outreach   → verifies and emails role-based addresses (info@, contact@)
3  Prospect Adder       → pushes verified contacts into Snov.io lists for follow-up sequences
```

The core idea: **the AI only proposes leads. Every fact after that is checked with real code**, not model confidence. See the blog post for why that matters.

## What you need

- An [n8n](https://n8n.io) instance (cloud or self-hosted)
- A [Notion](https://notion.so) database (schema below) — this is the pipeline's shared memory
- API credentials for:
  - **Google Gemini** (or swap for any n8n-supported chat model)
  - **Perplexity API**
  - **SerpApi** (Google Search)
  - **Snov.io** (domain search + email verification)
  - **Gmail** (sending outreach)
  - **Slack** (optional — run summaries)

## Setup

1. Create the Notion database — see [`docs/setup.md`](docs/setup.md) for the full property schema.
2. Import each workflow from [`workflows/`](workflows/) into n8n, in order (1 → 1.5 → 2 → 2.5 → 3).
3. In each workflow, reconnect the credentials — imports won't carry your API keys over. Every node needing one is left with a placeholder credential slot.
4. Replace the placeholder values search-and-replace style:
   - `YOUR_NOTION_DATABASE_ID` → your Notion database ID
   - `YOUR_SERPAPI_KEY` → your SerpApi key
   - `YOUR_SNOV_CLIENT_ID` / `YOUR_SNOV_CLIENT_SECRET` → your Snov.io app credentials
   - `REPLACE_WITH_YOUR_CREDENTIAL_ID` → n8n fills this in automatically once you select a credential in each node's UI
5. Edit the system prompt in **Agent 1 → "Build Agent Prompt"** — this is where you describe your own product and ICP list. See [`docs/customize.md`](docs/customize.md).
6. Turn on the schedule triggers once you've test-run each workflow manually.

## Customize for your own product

The whole system is reusable — you're not limited to WordPress/PDF leads. The three places to edit:

- **Agent 1's system prompt** — swap the product description and the ICP (target audience) list
- **Agent 1.5's qualifying check** — right now it checks for WordPress + PDFs; swap for whatever signal proves your product fits
- **Agent 2.5's email templates** — rewrite the subject lines and body copy for your product

See [`docs/customize.md`](docs/customize.md) for a walkthrough.

## A note on the AI

Agent 1 can occasionally return a plausible-sounding site that turns out not to exist or not to match — that's expected from any LLM-driven search agent. That's exactly why Agent 1.5 exists: nothing is trusted until a real HTTP request confirms it. If you fork this, keep that separation — don't let the discovery agent's output go straight to outreach.

## License

MIT — use it, fork it, adapt it for your own product.
