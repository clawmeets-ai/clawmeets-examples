# Solopreneur Team

A two-agent solopreneur crew that turns half-formed product ideas into shipped features and honest GTM plans. **Your** roadmap, **your** customer notes, **your** metrics, and **your** draft copy never leave your machine.

## The Team

| Agent | What they do | Deliverables |
|-------|--------------|--------------|
| `@pm` *(solopreneur)* | ICP definition, MVP scoping, user stories, feature prioritization, lightweight user research | PRD, prioritized backlog, user-interview script, product-sense critique |
| `@marketing` *(solopreneur)* | Positioning, messaging, growth channels, competitive analysis, launch planning | Positioning doc, launch plan, channel-test matrix, competitive teardown memo |

## Install

```bash
clawmeets init --from-url https://raw.githubusercontent.com/clawmeets-ai/clawmeets-templates/main/solopreneur/setup.json
clawmeets start
```

Drop prior positioning docs, customer-interview transcripts, competitor teardowns, and past launch postmortems into each agent's `knowledge_dir` so the first request doesn't read generic.

## Compelling Project Requests

Paste any of these into your coordinator. Replace the bracketed details with your own.

### Launch this feature

> I want to ship **[feature]** in **[N] weeks**. Write the **PRD** with acceptance criteria, the **prioritized build order** (what's P0 vs cut), **3 customer-interview questions** I should ask this week to de-risk the core assumption, and a **launch plan** — positioning one-liner, 3 distribution channels worth testing, and the metrics we'll look at 2 weeks post-launch to call it a win or a miss.

### Reposition a stale product

> Our product is **[product]** and we've been stuck at **[$ MRR / N users]** for a while (latest metrics in my knowledge folder). `@marketing`: run a **positioning audit** — what we say vs what the market actually hears — compare to competitors in my notes, and draft **2 alternative positioning bets** I could A/B test on the homepage and in ads. `@pm`: for each positioning bet, tell me **what the product has to do differently** for that bet to be honest — features to emphasize, roadmap items to pull in, anything we'd have to stop shipping to not dilute it. List the **metrics I should watch** to decide which wins.

### ICP gut-check

> I've been selling **[product]** to anyone who'll buy. `@pm`: look at my recent paying customers (notes in my knowledge folder), cluster them by **who renews and refers** vs **who churns or complains**, and name the **actual ICP** the data supports (not the one in my deck). `@marketing`: redraft the **headline and first-run onboarding** for that ICP, and tell me which segments I should **stop selling to this quarter** — including the messaging changes that will repel them so the sales team doesn't keep dragging them back in.

### Competitive teardown

> **[Competitor]** just launched **[feature]** and shipped a pricing change. `@marketing`: tear down what they built, what problem they're now solving better than us, where they're ignoring ground we still own, and the **3 ways we should respond** (copy / differentiate / ignore — be honest). `@pm`: translate the response into a **roadmap-level take** — which items move up, which move down, and what we have to stop doing this quarter so the response actually ships. Output a **one-page memo** I can show a design partner to explain where we're going.

### Pre-launch plan

> I'm launching **[product]** in **[N] weeks**. `@pm`: lock the **launch scope** — P0 features that must ship, P1s to cut, and the one demo flow the launch post will actually show. `@marketing`: build the **pre-launch week-by-week** — who to reach out to (warm list in my knowledge folder), the **Product Hunt plan** with hunter outreach, the **drip sequence** to my waitlist, the **launch-day asset list** (tweets, hero demo, screenshots, launch post), and the **after-launch plan for week 2** when attention drops — because that's the week most launches die.

### Channel test plan

> I have **[$ budget / month]** to spend testing growth channels for **[product]**. `@marketing`: design **3 channel tests** running in parallel over 6 weeks (cold outbound / SEO content / paid ads, or whatever makes sense for my ICP), set the **weekly metrics that tell me which is working**, and define **kill criteria** so I don't sink another dollar into the losers. `@pm`: sanity-check each channel against the product — **can the current onboarding handle the traffic this channel sends?** Flag the one activation or instrumentation gap we'd need to close before the channel test is even meaningful. Draft the **first week of assets** for each channel so I can actually start Monday.

## Why This Team Works

Most solopreneurs get advice from generic templates written for an imaginary startup. This team works off **your** customer notes, **your** churn data, and **your** past launch postmortems in the knowledge folder — so the PRD prioritizes what your actual users asked for, and the channel test plan doesn't suggest a strategy your last launch already proved doesn't work. The result is two agents that argue with each other in your voice: the PM wants to scope smaller, the marketer wants to position sharper, and you get the tension of a real co-founder conversation without hiring one.

**First-run checklist:** drop your latest sales calls, churn notes, competitor screenshots, and any positioning doc older than 6 months into the per-agent knowledge dirs so the second run is sharper than the first.
