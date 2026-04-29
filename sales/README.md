# Sales Desk Team

A four-agent sales crew that works in parallel on your pipeline. Your ICP, your playbook, your winning proposals, and your CRM exports stay on your machine — nothing leaves for a SaaS tool to train on.

## The Team

| Agent | What they do | Deliverables |
|-------|--------------|--------------|
| `@account_researcher` | ICP scoring, buying signals, org mapping, why-now briefs | One-page account briefs, org maps, signal digests |
| `@prospector` | Cold sequences, multi-channel cadence, qualification | Tailored sequences, subject-line variants, calling scripts |
| `@closer` | Discovery + demo prep, objection handling, MAPs, MEDDPICC | Pre-call briefs, close plans, champion enablement packs |
| `@proposal_writer` | Proposals, SOWs, pricing, redlines, exec summaries | Tailored proposals, pricing decks, MSA negotiation notes |

## Install

```bash
clawmeets init --from-url https://raw.githubusercontent.com/clawmeets-ai/clawmeets-templates/main/sales/setup.json
clawmeets start
```

## Compelling Project Requests

Paste any of these into your coordinator. Replace the bracketed details with your own.

### Build the next quarter's pipeline

> I need to add **$[target]** in pipeline by **end of [quarter]**. My ICP is **[e.g. Series B–D SaaS, 200–2000 headcount, HQ North America]** and I've dropped my target-account list into `@account_researcher`'s knowledge folder. Score them for fit, find the right contacts at each of the top accounts, write personalized cold sequences grounded in real signals, and give me a **calling script for the first batch** I'll dial tomorrow morning.

### Conference follow-up at scale

> I just got back from **[conference]** with a stack of business cards (scanned contacts in my knowledge folder) and a vague memory of what we talked about. Research each person's company and role, write tailored follow-ups referencing our likely conversation topic, and flag the ones worth a second meeting this week.

### Close a specific deal

> I have a deal with **[account]** stuck in procurement review (context in my knowledge folder). Build the **multithreaded close plan (MAP)**, draft **exec-sponsor outreach** for our CEO to send their CEO, prep me for the next procurement call with the **3 objections I'll face**, and have a **redline-ready MSA response** ready before the call.

### Renewal at-risk save

> Some of my top accounts have **dropping usage** this quarter (usage + CS notes in my knowledge folder). Diagnose what's slipping, draft outreach **from me** to each CS owner, build a **save plan with exec-escalation paths**, and tell me which one to call first.

### Outbound campaign from scratch

> Launch a **4-week outbound campaign** to **[target segment: e.g. mid-market fintech CFOs]**. Segment by ARR and tech stack using my target-account list in the knowledge folder, write a **6-touch multi-channel sequence** with **3 A/B variants** at the subject-line and first-line level, set a **realistic meeting-booked target**, and tell me the first batch of accounts to load.

### Closed-lost win-back

> Pull everyone in the CRM export in my knowledge folder who went to **closed-lost in the last 18 months**. Score them by **why they churned**, identify the ones where the reason is now stale (new product, new pricing, leadership change on their side), and draft **re-engagement outreach** for the top candidates — each grounded in what's changed since.

### Proposal machine

> I have several deals that need proposals this week (deal briefs + call notes in my knowledge folder). `@account_researcher`: for each deal, pull the **why-now** — buying signals, org changes, and the champion's real KPI — so no two proposals sound the same. `@closer`: for each, decide the **close strategy** (lead-with-ROI / lead-with-risk / land-and-expand) and the one objection each exec sponsor is most likely to raise. `@proposal_writer`: generate **tailored proposals, pricing decks, and one-page exec summaries** using my playbook and past winning proposals, leaning on the strategy + why-now. Flag any scope I should push back on before sending.

### Quarterly deal review

> Run a **quarterly deal review** across my open pipeline (pipeline export in my knowledge folder). For every deal above my threshold, score it on **MEDDPICC**, flag deals that haven't moved in two weeks, recommend which to kill vs. double down on, and build my **honest commit / best-case / pipeline call** for the forecast meeting.

## Why This Team Works

Most "AI SDR" tools train on aggregated buyer data and send the same templated emails your prospects have seen 50 times this quarter. This team works the opposite way: it reads **your** past-winning proposals, **your** ICP definition, **your** CRM exports, and **your** playbook from your local knowledge folders — and writes in your voice. Nothing about your pipeline or your customers leaves your machine.
