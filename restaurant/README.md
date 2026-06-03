# Restaurant Team

A four-agent crew for restaurant owners and operators. Hand it your restaurant's URL and a target audience hypothesis; get back **an audit, a redesign brief, and a working HTML mockup** — pricing, positioning, identity, and the new site, in parallel.

## The Team

| Agent | What they do | Deliverables |
|-------|--------------|--------------|
| `@market_analyst` | Site crawl + menu/price harvest, trade-area sizing, concept-competitor map, PMF read | `crawl/site-snapshot.md`, `analysis/market.md` |
| `@menu_finance` | Plate cost, prime-cost discipline, menu-engineering matrix, price-move recs | `analysis/menu-economics.md` |
| `@brand` | Positioning, voice + tone, rewritten menu copy, concept naming | `design/current-voice.md`, `design/brand-brief.md` |
| `@designer` | Visual identity (logo, palette, type), menu layout, single-file HTML mockup | `design/current-design-system.md`, `design/redesign-brief.md`, `design/mockup.html` |

`@designer` runs on Gemini; the rest run on Claude. `@market_analyst` and `@designer` both use the `playwright-browser` skill to crawl real restaurant sites (Squarespace, Wix, Toast, Square Online — all JS-rendered). `@designer` ships the redesigned mockup via the `web-artifacts` skill (React + Tailwind + Chart.js via CDN, single HTML file).

## Install

```bash
clawmeets bootstrap browser   # one-time per machine — installs Chromium for playwright-browser
clawmeets init --from-url https://raw.githubusercontent.com/clawmeets-ai/clawmeets-templates/main/restaurant/setup.json
clawmeets start
```

## Compelling Project Requests

Paste any of these into your coordinator. Replace the bracketed details with your own.

### Audit and redesign my restaurant from its URL

> Audit and redesign **[Trattoria Bella, https://trattoria-bella.com]**. Target audience: **[weekday lunch crowd from the nearby office district + weekend dinner couples]**. All deliverables under `.bus-files/restaurant-audit/`. `@market_analyst` crawls the site, harvests menu items + prices, sizes trade area, maps the 5 closest concept competitors, and reads PMF → `crawl/site-snapshot.md` + `analysis/market.md`. `@menu_finance` builds the menu-engineering matrix and recommends per-item price moves → `analysis/menu-economics.md`. `@brand` extracts current voice, sharpens positioning, rewrites top 10 menu descriptions → `design/current-voice.md` + `design/brand-brief.md`. `@designer` extracts the current visual system, ships the redesign brief and a single-file HTML mockup of the new homepage + menu → `design/current-design-system.md` + `design/redesign-brief.md` + `design/mockup.html`. Coordinator stitches `INDEX.md` — the single sharpest improvement opportunity, the top 3 changes in priority order, and the ONE move to make this week.

### Reposition for a new daypart

> My **[Italian restaurant]** is strong Thursday–Sunday but quiet Monday–Wednesday. I want to add a **[weekday lunch / natural-wine bar / chef's counter]** using the same kitchen and staff. `@market_analyst` validates neighborhood demand and competitor gap by day-part. `@menu_finance` builds the incremental P&L and recommends the 8–12 SKU daypart menu with prices. `@brand` names the daypart, writes the positioning that doesn't confuse the existing concept, and drafts launch comms. `@designer` ships the daypart menu card design and a single-page HTML mockup of the new landing page. Coordinator returns run / don't run, breakeven covers per service, and the soft-launch plan for week 1.

### Refresh the menu, not the brand

> I don't want to redesign the brand — I want to fix the menu. Current site: **[URL]**. `@market_analyst` harvests the current menu and identifies where competitors are winning. `@menu_finance` builds the menu-engineering matrix, recommends price moves, picks the 3 hero plates, and names items to cut. `@brand` rewrites menu copy for the top 10 items in the existing voice. `@designer` ships a single-file HTML mockup of just the redesigned menu page (print + web). Coordinator lists the 5 menu changes to ship this month.

### Logo and site refresh from the existing brand

> I like my brand but the logo and site feel dated. Current site: **[URL]**. `@market_analyst` reads who the site currently signals to vs. the customer I actually want. `@brand` keeps the positioning, sharpens the voice for the new site. `@designer` extracts the current visual system, recommends a refined logo direction (not a full rebrand), refreshed palette + type system, and ships a single-file HTML mockup of the new homepage + menu. Coordinator lists the visual changes ranked by impact-to-effort.

### Concept stress-test before opening

> I'm opening **[Sichuan noodle bar]** at **[address / neighborhood]** in **[~5 months]**. No site yet. `@market_analyst` sizes the trade area, profiles the customer segment most likely to convert, maps the 5 closest concept competitors, and gives an honest PMF read for the concept in this neighborhood. `@menu_finance` recommends the 20-SKU launch menu with prices that clear a target prime cost of `[60–62%]`. `@brand` shortlists 5 names + rationale, drafts the one-sentence positioning, and writes voice guidelines. `@designer` ships logo direction, palette + type system, and a single-file HTML mockup of the launch homepage + menu page. Coordinator returns go / go-with-changes / no-go with the single-biggest-assumption that has to hold.

## What to Expect

Each agent works with its own proprietary knowledge base on your machine, in parallel. A typical audit-and-redesign request produces **6–8 deliverables in ~25–35 minutes** — the audit (market read + menu economics + voice extract + design system extract), the redesign brief (positioning + brand + visual identity), and a working HTML mockup of the new homepage and menu pages you can open in a browser. You stay in the loop via chat and approve or redirect at any point.
