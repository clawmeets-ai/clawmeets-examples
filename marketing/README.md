# Marketing Team

Eleven agents: a **strategist** (ROI, performance, calendar, milestone arcs), a **creative** (big-idea concepting, cross-channel hooks, naming), and nine channel specialists (`@instagram`, `@seo`, `@youtube`, `@tiktok`, `@events`, `@direct_mail`, `@email`, `@pr`, `@blog`). Your assistant stays a thin router — it doesn't carry marketing-specific knowledge; it routes to `@marketing_strategist` for prioritization, `@creative` for ideas, and the channel agents for execution.

## The Team

| Agent | What they own |
|-------|---------------|
| `@marketing_strategist` *(marketing)* | ROI prioritization across channels, performance analysis, campaign calendar, budget allocation, M1/M2/M3 milestone arcs with success metrics |
| `@creative` *(marketing)* | Content ideation, campaign concepts, big-idea generation, cross-channel hooks, naming, taglines |
| `@instagram` *(marketing)* | Feed / reels / stories, captions, hashtags, creator collabs, posting cadence |
| `@seo` *(marketing)* | Keyword research, on-page, technical audits, internal linking, backlink strategy |
| `@youtube` *(marketing)* | Long-form scripts, thumbnails, titles, retention shape, channel + Shorts strategy |
| `@tiktok` *(marketing)* | Hooks, trend selection, sound choice, vertical video structure, creator-first style |
| `@events` *(marketing)* | Conferences, webinars, popups, sponsorships — run-of-show, booth strategy, lead capture |
| `@direct_mail` *(marketing)* | Postcards, catalogs, packaging inserts — list segmentation, print specs (bleed/CMYK), attribution tracking |
| `@email` *(marketing)* | Newsletters, lifecycle/drip campaigns, deliverability, list health, transactional copy |
| `@pr` *(marketing)* | Press releases, media lists, journalist outreach, embargoes, crisis comms |
| `@blog` *(marketing)* | Editorial calendar, long-form posts, thought leadership, repurposing into other channels |

The assistant routes:

- ROI / prioritization / performance / calendar / budget → `@marketing_strategist`
- Ideation / campaign concepts / big ideas / naming → `@creative`
- Channel execution → the channel agent that owns it

All eleven agents assume **product + ideal market segment have already been briefed** before requests — typically via per-agent `knowledge_dir/` references and/or the request itself. Sample requests are written against that assumption.

## Install

```bash
clawmeets init --from-url https://raw.githubusercontent.com/clawmeets-ai/clawmeets-templates/main/marketing/setup.json
clawmeets start
```

Drop product specs, ICP notes, brand voice guidelines, past-campaign recaps, and any prior creative work into each agent's `knowledge_dir` so the first request doesn't read generic.

## Compelling Project Requests

Paste any of these into your coordinator. Replace bracketed details with your own.

### ROI prioritization across channels

> I'm planning **[campaign brief]**. Have `@marketing_strategist` run an ROI fan-out — collect from `@instagram`, `@tiktok`, `@youtube`, `@seo`, `@blog`, `@email`, `@pr`, `@events`, `@direct_mail` each one's best swing on this campaign (expected reach, conversion assumption, cost/effort, CAC). Then `@marketing_strategist` ranks channels top-to-bottom and recommends the top 3–4 with explicit reasoning for what's deprioritized.

### Milestone 1/2/3 strategy

> For **[goal]**, have `@marketing_strategist` define a 3-milestone arc — M1, M2, M3, each with a distinct purpose and success metric. Then for every channel, get back what each milestone produces from their surface and how the sequencing should land (e.g. PR + blog seed before paid amplifies; email lifecycle catches what the funnel already sent). `@marketing_strategist` finalizes the sequenced plan.

### Cohesive go-to-market plan

> Build the GTM for **[launch]**. Start with `@creative` for the big idea — one campaign concept that can be expressed in nine ways. Then send the concept to each channel for their channel-specific expression. `@marketing_strategist` lays out one calendar across all 9 channels, one budget table, and success metrics per milestone. Final output: one narrative, nine expressions, one calendar, one budget.

### Quarterly planning

> It's **[Q1/Q2/Q3/Q4]** planning. Have `@marketing_strategist` ask every channel for (1) what worked last quarter, (2) one experiment they want to run, (3) the budget they'd need — then synthesize a one-page quarterly plan with channel allocations and 2–3 cross-channel themes. `@creative` proposes the campaign theme(s) that tie the experiments together.

### Post-mortem on last campaign

> Run a post-mortem on **[campaign name or attached recap]**. `@marketing_strategist` collects per-channel performance vs. expectation from every channel, identifies what beat and missed forecast, and proposes 2–3 specific changes for next time.

## Content engine (sample pipeline)

A four-phase pipeline that turns a theme into reviewed, ready-to-publish drafts — coordinated through one Google Sheet, with two human sign-off gates. No skills required; each phase is just a copyable sample request on the relevant agent's page.

```
@creative              "Start the content engine"        ──►  appends idea rows to Sheet
@marketing_strategist  "Grade content ideas"             ──►  fills grade + grade_rationale
[YOU]                  set sign_off = yes on chosen rows in the Sheet
@blog                  "Draft signed-off blog ideas"     ──►  fills draft_body inline in the Sheet
@email                 "Draft signed-off email ideas"    ──►  fills draft_body inline in the Sheet
[YOU]                  review the draft cell, set publish_sign_off = yes, publish manually
```

Channels: `blog` and `email`. Drafts live inline in the sheet's `draft_body` column — no separate files, no `file://` links. The pipeline ends at the draft; you publish manually.

### Sheet schema (paste as row 1)

```
id  created_at  channel  idea_title  idea_brief  grade  grade_rationale  sign_off  draft_body  drafted_at  publish_sign_off  notes
```

| Column | Written by |
|---|---|
| `id`, `created_at`, `channel`, `idea_title`, `idea_brief` | `@creative` |
| `grade`, `grade_rationale` | `@marketing_strategist` |
| `sign_off` | **you** (`yes` to advance to draft) |
| `draft_body`, `drafted_at` | `@blog` / `@email` (markdown content inline; email packs subject + pre-header as YAML frontmatter) |
| `publish_sign_off` | **you** (`yes` after reviewing draft) |
| `notes` | anyone |

Google Sheets cells cap at 50,000 characters — covers all but the longest long-form. If a draft hits the cap, the agent flags it in its reply so you can split manually.

### Setup

1. **Create a Google Sheet.** Name it whatever you want. Paste the header row above into row 1.
2. **Note the Sheet's id.** It's the long token in the URL between `/d/` and `/edit`.
3. **Configure the MCP for each of the four content-engine agents** (`@creative`, `@marketing_strategist`, `@blog`, `@email`). In each agent's MCP panel → `google-drive-write` → Configure, paste:
   ```jsonc
   {
     "spreadsheet_id": "<paste your sheet id>",
     "sheet_name": "ideas",
     "row_id_column": "id"
   }
   ```
   On first call the OAuth flow opens; authorize once per agent. Scope is `spreadsheets` (read + write).
4. **Click the "Start the content engine" sample request** from the team page, paste your theme into the bracketed prompt, and send.
5. **Schedule phases 2–4 from each agent's page.** Open `@marketing_strategist` / `@blog` / `@email`, copy their **Content engine** sample request (`Grade content ideas` / `Draft signed-off blog ideas` / `Draft signed-off email ideas`), and schedule it via the DM-scheduling UI on the cadence you want — e.g. `0 9 * * *` for grade, `0 10 * * *` for the producers. Or just send any of them on-demand without a schedule.

## Why This Team Works

A single generalist marketing agent collapses too much heterogeneity into one role — Instagram reels and Google Ads bids and direct-mail print specs and PR pitches barely overlap as crafts. This team splits the work the way real marketing teams do: channel specialists own their surface, a strategist owns ROI / calendar / budget across all of them, a creative owns the big idea that ties them together, and the assistant stays a thin router so it doesn't have to be a marketer. One narrative, eleven expressions.

**First-run checklist:** drop product specs, ICP notes, brand voice guidelines, past-campaign recaps, and existing creative work into the per-agent knowledge dirs (`./marketing_strategist`, `./creative`, `./instagram`, `./seo`, `./youtube`, `./tiktok`, `./events`, `./direct_mail`, `./email`, `./pr`, `./blog`) so requests grade against your real product and real audience, not invented ones.
