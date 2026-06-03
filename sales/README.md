# Sales Desk Team

A three-agent top-of-funnel pipeline crew. The **SDR** runs lead gen + qualification, **Inside Sales** runs cold email + digital follow-up cadence, and **Field Sales** runs cold-visit pitch + in-person follow-up cadence. Your ICP, your past-winning pitches, your CRM exports, and your reply history stay on your machine — nothing leaves for a SaaS tool to train on.

## The Team

| Agent | What they do | Deliverables |
|-------|--------------|--------------|
| `@sales_dev` | ICP target-list build, fit scoring, buying-signal scans, persona qualification, **contact discovery** (named decision-maker + email/LinkedIn/phone + best channel), disqualification checks, tier splits, handoff packets | Scored target lists, per-account why-now hooks, named contacts with inferred email + LinkedIn + best channel (confidence-flagged), tier splits (digital vs in-person), handoff packets to Inside Sales / Field Sales, cold-dead lead re-qualifications |
| `@inside_sales` | Cold email drafting, A/B variants, multi-step follow-up cadence, trigger-based one-offs, reply triage, digital re-engagement | First-touch cold emails, subject-line + first-line A/B variants, 5-touch follow-up cadences with kill rules, meeting-ask drafts after positive replies, reply triage (interested / not-now / hard-no), 3-touch re-engagement sequences |
| `@field_sales` | Pre-visit briefs, 30-second cold openers, long + short cold-visit pitches, conference on-site motion, post-visit follow-up cadence, exec-sponsor in-person touches | Pre-visit account briefs, 30-second openers, 5-minute + 30-minute cold-visit pitches, pre-event target lists with on-site meeting asks, post-visit follow-up cadences (handwritten note → email recap → value-aided content → meeting ask), re-warm touches for stalled accounts |

## Install

```bash
clawmeets init --from-url https://raw.githubusercontent.com/clawmeets-ai/clawmeets-templates/main/sales/setup.json
clawmeets start
```

## Compelling Project Requests

Paste any of these into your coordinator. Replace the bracketed details with your own.

### End-to-end cold-pipeline build

> Build my **end-to-end cold pipeline** for **[segment — e.g. Series B–D SaaS, 200–2000 headcount, HQ North America]**. `@sales_dev`: build the target-account list from public sources + my CRM exports, score every account on **ICP fit**, scan recent **buying signals** (funding / hiring / launches), pick the persona per account, AND surface the **named decision-maker** for that persona with their **LinkedIn + inferred email + best channel** (confidence-flagged). Rank by **fit × signal** with a one-sentence **why-now** + the named contact per row. Then **tier-split**: digital-touch (SMB / MM) → `@inside_sales`, in-person-touch (strategic / VP+) → `@field_sales`, with handoff packets per account. `@inside_sales`: draft the **first-touch cold email** per persona AND the **5-touch follow-up cadence** (channel mix, intervals, kill rule). `@field_sales`: draft the **pre-visit brief**, the **30-second cold opener**, the **5-minute + 30-minute cold-visit pitch**, and the **post-visit follow-up cadence** (handwritten note → recap → value-aided content → meeting ask). Coordinator: stitch `INDEX.md` — the **target math** (accounts × reply rate × meeting rate), the first batch this week, and the ONE strategic account worth a personally-handcrafted in-person ask.

### Cold email campaign at scale

> Launch a **[duration]** cold email campaign to **[persona — e.g. mid-market fintech CFOs]**. `@sales_dev`: pull the target list, score on ICP fit, surface the **50 highest-signal accounts** with one-sentence why-nows. `@inside_sales`: build the **first-touch cold email** (50–90 words, one ask, no attachments) with **3 A/B variants** at the subject-line + first-line level. Build the **5-touch follow-up cadence** over 14–21 days — channel mix, kill rules (if step 3 has no opens, the list is wrong, not the copy). Pre-draft **trigger-based one-offs** for accounts that hit signals mid-campaign. Set the realistic **reply-rate + meeting-booked target** tied to my pipeline gap.

### Cold-visit motion for a strategic account

> Build the **cold-visit motion** for **[account]** (context + org chart in my knowledge folder). `@sales_dev`: surface the latest buying signals + the persona to target first. `@field_sales`: write the **pre-visit account brief** (economic buyer + champion + blocker mapped, the one why-now hook, warm-intro paths I actually have). Draft the **30-second cold opener** I'd use at reception or in a hallway run-in. Write the **cold visit pitch** in two versions: **5-minute** (got their attention, no chair) and **30-minute** (sat down). Draft the **post-visit follow-up cadence**: same-day handwritten note, +2 day email recap, +1 week value-aided content, +2 week meeting ask, with switch-to-exec-sponsor rule if the daily-user contact stalls. Flag the one moment in the pitch where I should expect **resistance** and pre-draft 2 responses.

### Conference / event cold motion

> I'm attending **[event — name + dates]**. `@sales_dev`: from the attendee list + my pipeline, score for ICP fit and surface the **20 accounts** worth in-person time, tiered by digital vs in-person. `@field_sales`: for the in-person tier, draft the **pre-event outreach** to book on-site meetings (why in-person beats Zoom for this specific deal), draft the **on-site cold pitch hooks** per persona, propose the **dinner shortlist of strongest 10**, and draft the **post-event follow-up cadence** tiered by what happened on-site (booked next-step / interested-but-cold / hard no). `@inside_sales`: for anyone who said "email me after" or wasn't VP+, build the post-event email cadence tailored to the booth conversation hook. Coordinator: tell me my **top 3 accounts to pre-book** before the event opens.

### Reactivate cold-dead leads

> Reactivate the **cold-dead leads** in my CRM (export in my knowledge folder). `@sales_dev`: **re-qualify** each — has anything changed (new funding, new exec, new product launch, new role for the contact)? If the original contact left, surface the **new named decision-maker** + their inferred email + LinkedIn + best channel. Surface the **30 accounts** where the original disqualifier is no longer true and tier them. `@inside_sales`: for the digital tier, draft **re-engagement emails** that DON'T pretend the silence didn't happen — name the gap, name what's changed, re-anchor the why-now. Build the **3-touch re-engagement cadence** with the kill rule. `@field_sales`: for the VP+ tier (or any account where the original contact left and an exec moved in), draft the **personal LinkedIn touch + in-person ask**, and propose the one strategic account worth a flight.

### Reply triage + follow-up split

> I just ran a **[campaign — e.g. 200-account]** cold push and replies are coming in (replies + read receipts in my knowledge folder). `@inside_sales`: **triage every reply** — interested / not-now / hard-no / unsubscribe. For interested, draft the **meeting-ask follow-up** that confirms a calendar slot without making them work to schedule. For not-now, set the **follow-up calendar reminder** + the re-touch copy that names what's changed. For hard-no / unsubscribe, log + retire. `@field_sales`: for any interested reply at **VP+ on a strategic account**, recommend whether the next step should be **in-person** (cold visit / dinner / on-site) instead of a Zoom discovery call, and draft the personal note that suggests it. Coordinator: surface the **3 follow-ups I should personally send today** and the one I should **call instead of email**.

## Why This Team Works

Most "AI SDR" tools train on aggregated buyer data and send the same templated emails your prospects have seen 50 times this quarter. This team works the opposite way: it reads **your** ICP definition, **your** past-winning pitches, **your** CRM exports, and **your** reply history from your local knowledge folders — and writes in your voice. The motion mirrors a real top-of-funnel org: the **SDR scores and tiers**, **Inside Sales runs digital cold + follow-up cadence**, **Field Sales runs in-person cold + follow-up cadence**, and the coordinator enforces the kill rules so dead campaigns don't keep eating calendar. Nothing about your pipeline, your reply history, or your customers leaves your machine.
