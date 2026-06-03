# Customer Success Team

A two-agent CS crew that splits the lifecycle the way a real org does: the **Account Specialist** owns the relationship side (onboarding, ongoing health, expansion), and the **Customer Support** agent owns the inbox (triage, general inquiries, response drafting, FAQ distillation). Your portfolio, your usage data, your ticket history, and your past expansion plays stay on your machine — nothing leaves for a SaaS tool to train on.

## The Team

| Agent | What they do | Deliverables |
|-------|--------------|--------------|
| `@account_specialist` | Onboarding plans, ongoing health monitoring, QBR prep, expansion theses tied to value delivered | 30/60/90 onboarding plans, kickoff agendas, stakeholder maps, portfolio health scorecards, at-risk diagnoses, save plans, QBR decks, expansion theses, honest renewal calls |
| `@customer_support` | Ticket triage, general customer inquiry handling, response drafting, internal escalation, recurring-question FAQs | Triaged queues, drafted responses in your voice, P1 comms cadences, refund/credit decisions, internal handoff notes, FAQ entries from clustered repeat tickets, after-action / RCA prompts |

## Install

```bash
clawmeets init --from-url https://raw.githubusercontent.com/clawmeets-ai/clawmeets-templates/main/customer_success/setup.json
clawmeets start
```

## Compelling Project Requests

Paste any of these into your coordinator. Replace the bracketed details with your own.

### Onboarding plan for a new account

> Build the onboarding plan for **[account]** (signed deal context + champion + use case in my knowledge folder). `@account_specialist`: write the **30/60/90 plan** — milestones tied to **time-to-first-value**, the success criteria each milestone unlocks, and the owner on the customer side for each step. Add the **stakeholder map** — economic buyer, champion, daily user, blocker, with the warm-intro path for each. Build the **kickoff agenda** — the **5 things the customer needs to hear in the first 30 minutes** and the **3 commitments we need by the end**. Flag the one risk in the first 30 days that, left alone, predicts a stalled rollout.

### Portfolio health scan

> Run a **portfolio health scan** across my book (usage exports + support tickets + last-touch dates in my knowledge folder). `@account_specialist`: score every account on **green/yellow/red** — leading indicators (**login frequency, depth-of-feature use, exec engagement, ticket sentiment**), trailing indicators (**NPS, billing changes**). Rank by **risk × ARR**. For every red, write a one-paragraph **diagnosis** + the single **next action**. Tell me which one account needs me on a call this week and what to say in the first 60 seconds.

### Diagnose an at-risk account

> Account **[account]** is showing **[signal — e.g. 40% drop in WAU, champion left, 3 P1s unresolved]**. Context + usage + support history in my knowledge folder. `@account_specialist`: separate **symptom from cause** across **product fit, adoption gap, champion change, or service failure**. Build the **save plan** with the **exec-escalation path**, the one in-person or video touch that resets the relationship, and the honest read on whether this is a save or a graceful exit. Pre-draft the outreach the new champion (or me) should send today.

### QBR prep with the exec sponsor

> Prep me for the **[quarter]** QBR with **[account + exec]**. `@account_specialist`: pull what's changed on their side (org, strategy, public statements, funding/M&A). Recap **value delivered this quarter against their stated KPIs** — tie every number to a business outcome they care about, not a product metric. Propose the **expansion ask** (BU, persona, value thesis). Pre-draft responses to the **2 awkward questions** they're likely to raise.

### Expansion opportunity scan

> Scan my portfolio for **expansion opportunities** (usage + org-change signals + renewal dates in my knowledge folder). `@account_specialist`: identify the **top 10 accounts** where the **land-and-expand thesis** is ripe — value already delivered in the landed BU, a clear adjacent BU, a credible new champion. Rank by **ARR potential × probability**, with the **value-delivered proof point** and the proposed first-touch for each. Flag any expansion that risks **blowing up the renewal in the landed BU** — that's a hard stop.

### Triage today's ticket queue

> Triage today's open tickets (export in my knowledge folder). `@customer_support`: rank every ticket by **BOTH internal severity AND customer-perceived severity** — flag the deltas, because the second one drives churn. Group **green/yellow/red** with the single next action per ticket (**respond now / await info / escalate to <team>**). Surface the **3 tickets I personally need to look at this morning** and the one I should delegate to product or engineering before lunch.

### Draft response to a customer inquiry

> Customer just wrote in: **[inquiry — paste verbatim, with account context if known]**. `@customer_support`: draft a response that **acknowledges the specific thing they asked**, answers what's actually answerable from product knowledge / past tickets / FAQ in the knowledge folder, and does **NOT overpromise** on anything that requires engineering or product confirmation. Tone: warm but precise. If the inquiry needs escalation, draft the holding response **AND** the internal escalation note. If the account is on a watch list, also flag `@account_specialist`.

### P1 incident comms plan

> P1 incident: **[status + scope + impacted customers]**. `@customer_support`: build the **customer-facing comms cadence** — initial acknowledgment (within 15 min), status updates (cadence tied to severity), resolution note, post-incident follow-up. Pair with the **internal escalation map** (owner / by-when / status interval) and the list of customers who get a personal call vs. a status-page note. For strategic accounts in the impacted set, `@account_specialist` owns the **exec-level relationship recovery** — produce the exec call brief separately.

### Recurring-question FAQ

> Pull last **[period: 90 days]** of resolved tickets from my knowledge folder. `@customer_support`: cluster by **root question** (not by category tag — those lie), surface the **top 10 recurring questions** ranked by **volume × handle-time**, and for each draft the public FAQ entry that would have deflected the ticket. Flag the **2 questions** that are recurring because the product itself is confusing — FAQ won't fix those; product will.

## Why This Team Works

Most CS tools score health on aggregated benchmarks that have never seen your product or your customers, and most "AI support" tools answer from a generic FAQ that doesn't know which of your customers is one cycle from churn. This team reads **your** usage data, **your** ticket history, **your** past renewal saves, and **your** expansion playbook from your local knowledge folders — and splits the work the way a real CS org does: the account_specialist runs the relationship side (leading indicators over trailing, time-to-first-value over feature-tour completion, value-delivered before expansion ask), and the customer_support agent runs the inbox (customer-perceived severity over internal severity, acknowledge fast without overpromising, FAQs from clustered repeats not imagination). Ticket-sentiment trends flow back as a *signal* for portfolio health; the inbox itself stays with support. The renewal forecast you get back is honest, not hopecast — and nothing about your portfolio, your tickets, or your customers leaves your machine.
