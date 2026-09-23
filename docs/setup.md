# Setup — Notion database schema

Create a Notion database with these properties. Names must match exactly — the workflows read and write by property name.

| Property | Type | Used by | Notes |
|---|---|---|---|
| `Website URL` | Title | 1, 1.5, 2, 3 | The lead's primary key |
| `ICP` | Select | 1, 3 | Your target-audience segments; add your own options |
| `Source` | Select | 1 | e.g. Perplexity, Google Search, Manual |
| `Status` | Select | all | Identified → Rejected / Qualified / Qualified (Marketing) / Qualified (Email Found) / Qualified (Snov.io Added) → Email Sent → Replied → Converted, etc. |
| `Has PDFs` | Select | 1.5 | Yes / No / Unknown |
| `Has Scanned PDFs` | Select | 1.5 | Yes / No / Unknown |
| `Has Private Docs` | Select | 1.5 | Yes / No / Unknown |
| `Wordpress` | Select | 1.5 | Yes / No |
| `Website Email` | Text | 1.5 | Scraped from the homepage/contact page |
| `Website Phone` | Text | 1.5 | Scraped from the homepage/contact page |
| `Contact Name` | Text | 2 | Best contact found via Snov.io |
| `Email` | Email | 2 | Best contact's email |
| `Company` | Text | 2 | From Snov.io domain info |
| `Industry` | Select | 2 | From Snov.io domain info |
| `Company Size` | Select | 2 | From Snov.io domain info |
| `Other Contacts` | Text | 2 | Runner-up contacts found |
| `Snovio Response` | Text | 2 | Raw log for debugging |
| `Snovio Checked` | Checkbox | 2 | Whether Agent 2 has run on this lead |
| `2.5 Email` | Text | 2.5 | The generic address used |
| `2.5 Sent` | Select | 2.5 | Yes / Qualified / No |
| `2.5 Date Sent` | Date | 2.5 | |
| `Email Template Sent` | Select | 3 | Which cold template was used |
| `Snov.io Prospect Link` | URL | 3 | |
| `Snov Profile URL` | Text | 3 | |
| `Snovio General List` | Checkbox | 3 | |
| `Date Contacted` | Date | — | |
| `Follow-up Date` | Date | — | |
| `Web Outreach` | Select | — | Sent / No Contact Page Found |
| `Web Outreach Date` | Date | — | |
| `Converted to Paid` | Checkbox | — | Manual, for tracking results |
| `Agent (1.5) Note` / `Agent (2.0) Note` / `Agent (3.0) Note` / `Agent (3.5) Note` | Text | all | Free-text debug notes each agent writes |
| `Notes` | Text | — | Your own manual notes |

Each workflow's Notion nodes are set up with `getAll`/`update` operations against this database — after importing, open each Notion node and point it at your own database.
