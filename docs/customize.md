# Customizing this for your own product

This system ships as a **template**, not a working example tied to any one
product — the AI prompt and target-audience list are placeholders you're
meant to replace. Three places to edit:

## 1. Agent 1 — the system prompt

Open `1-lead-discovery.json`, find the **"Build Agent Prompt"** code node.
At the top, edit:

```js
const YOUR_PRODUCT_NAME = 'Your Product Name';
const YOUR_PRODUCT_DESCRIPTION = `...`;
```

Describe what your product actually does and who it's for — this is what
the AI agent uses to judge whether a lead is a fit. Further down, the
`REQUIRED JSON SCHEMA PER LEAD` block has two placeholder fields,
`qualifying_signal_1` / `qualifying_signal_2` — rename these to whatever
your product's real qualifying signals are (and keep them in sync with
step 3 below).

**Keep the `URL RULE` exactly as written.** It's the rule that stops the
agent from hallucinating a URL that doesn't exist — the single most
important line in the whole prompt.

## 2. Agent 1 — the target-audience list (ICPs)

Same workflow, find the **"Build ICP Context + Skip List"** code node.
Below the `EDIT EVERYTHING BELOW THIS LINE` marker:

- Replace `exampleIcp1Queries` / `exampleIcp2Queries` with your own Google
  search-operator strings — add as many query variants per niche as you
  want; the workflow rotates through them day by day
- Replace the `icps` array with your own target audiences: `name`, `who`,
  `pain`, and `qualifying_signals` are used directly in the AI prompt;
  `perplexity_prompt` and `google_operator` drive the two search tools
- Add or remove ICPs freely — `icpIndex = dayNumber % icps.length` adapts
  automatically to however many you define

## 3. Agent 1.5 — the qualifying check

Open `1.5-site-verification.json`, find **"Check Signals"**. This is
where "qualified" is actually decided, with a real HTTP request — not
the AI. Replace the current check with whatever objectively proves your
product fits (specific page/text existing, a tech-stack fingerprint,
etc.), and make sure the field names you write back to Notion match the
`qualifying_signal_1` / `qualifying_signal_2` naming you chose in step 1.

## 4. Agent 2.5 — the outreach email

Open `2.5-generic-outreach.json`, find **"Send Generic Outreach Email"**.
Replace the subject-line array and the email body with your own copy —
keep it short and specific to the exact problem your product solves for
that ICP; generic outreach copy is why reply rates stay low.

## A general note

Every "Qualified" status this system produces is a claim you're about to
put in an email. Before wiring in your own product, walk through Agent
1.5's logic and make sure it's checking something that's actually true
when it says yes — that's the whole reason this pipeline is split into
stages instead of one AI agent doing everything end to end.
