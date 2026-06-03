# Retail Team

A three-agent digital workforce for retail, restaurant, and service-business owners. In the time a single consultant returns a draft, this team hands back a **site memo, P&L forecast, and launch campaign — in parallel**.

## The Team

| Agent | What they do | Deliverables |
|-------|--------------|--------------|
| `@market_analyst` | Market sizing, site/trade-area evaluation, competitor and customer intelligence | Site memo, competitor map, demand forecast, go/no-go call |
| `@finance` | 24-month P&L, break-even, capex, pricing, scenario analysis | Financial model, break-even chart, capex budget, funding plan |
| `@marketing` | Positioning, launch campaign, local/digital channels, loyalty | Positioning brief, 90-day launch plan, channel budget, loyalty design |

## Install

```bash
clawmeets init --from-url https://raw.githubusercontent.com/clawmeets-ai/clawmeets-templates/main/retail/setup.json
clawmeets start
```

## Compelling Project Requests

Paste any of these into your coordinator to see the team move. Replace the bracketed details with your own.

### New location go/no-go pack

> Build my **go/no-go pack** for opening a **second [coffee shop] in [East Austin]** in the next 6 months. Budget: **$[350K] all-in**. Two candidate sites: **[1450 E 6th — 1,400 sqft, $6.5K/mo NNN]** and **[2100 Manor Rd — 1,100 sqft, $5.2K/mo NNN]**. All deliverables go to `.bus-files/site-eval/`. `@market_analyst`: per-site memos (`site-A.md` / `site-B.md`) — trade area, competitors, scorecard, single-biggest-risk, go / go-with-changes / no-go. `@finance`: 24-month P&L driven by traffic × conversion × ticket + opening capex with 10–20% contingency → `p-and-l.md`. `@marketing`: positioning + pre-opening + grand opening + post-launch weekly themes + local press / influencer list + loyalty structure → `launch-90day.md`. Coordinator: stitch `INDEX.md` — recommended site, the single most important reason, the single biggest risk, the ONE decision to make this week.

### Launch a new product line

> I run a **[boutique fitness studio]** with **[800 active members]**. I want to launch a **[private-label supplement / apparel / recovery service] line** in the next **8 weeks**. `@market_analyst` sizes the opportunity against my member base and maps competitor offerings, `@finance` models unit economics + pricing + 90-day EBITDA contribution, `@marketing` builds the launch campaign tied to my existing member list. Coordinator returns go/no-go with the breakeven attach rate and the single biggest assumption that has to hold.

### Convert a slow daypart into a new concept

> My **[Italian restaurant]** is **packed Thursday–Sunday** but does **< $800/night Monday–Wednesday**. I want to turn **Tuesday nights into a [natural-wine bar concept]** using the same space and staff. `@market_analyst` validates neighborhood demand and competitor gap, `@finance` builds the Tuesday-only incremental P&L (does it clear marginal cost?), `@marketing` names the concept and designs the launch comms. Coordinator returns run / don't run with breakeven covers per Tuesday and a soft-launch week 1.

### Go from one location to three

> My **[neighborhood bakery]** did **$[1.1M] last year** at one location with **[22%] store-level EBITDA**. I want to open **two more locations in the same metro over 18 months**. `@market_analyst` pre-screens 5–7 candidate neighborhoods (trade-area fit, competitor density, cannibalization risk) and recommends the top 2. `@finance` builds the capital plan, the cannibalization haircut, the funding mix, and the financial guardrails store #2 must hit before store #3. `@marketing` writes the playbook to open store #2 without cannibalizing store #1. Coordinator names the ONE leading indicator at store #2 that decides store #3.

### Launch a DTC online extension

> My **[home goods shop]** has **[4K Instagram followers]** and **~[120 walk-ins/day]**. I want to launch a **DTC online store** in **[90 days]** that ships nationwide. `@market_analyst` picks the 20 SKUs to lead with and profiles the DTC buyer vs. my walk-in, `@finance` models blended margin after shipping + returns and sizes the working capital, `@marketing` builds the 90-day launch campaign to my existing audience. Coordinator lists the 3 KPIs to watch in month 1 and the kill-criteria if they miss.

### Respond to a new competitor

> A **[national chain competitor]** just signed a lease **[two blocks from my pet supply store]**. They'll open in **~[4 months]**. `@market_analyst` models their likely opening playbook, names where I can't compete on cost and where I *can* compete on service / curation / community, and estimates churn risk by segment. `@finance` models the do-nothing impact and the retention investment that pays back. `@marketing` designs the retention campaign to lock in my best customers and the positioning shift that says what I am that they aren't. Coordinator lists the 3 moves to make before they open and the 3 to make in their first 60 days.

## What to Expect

Each agent works with its own proprietary knowledge base on your machine, in parallel. A typical request above produces **3–5 deliverables in ~15 minutes** — strategic-level work that would normally take weeks of meetings with consultants, brokers, and agencies. You stay in the loop via chat and approve or redirect at any point.
