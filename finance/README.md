# Personal Finance Team

A three-agent personal-CFO crew that holds the monthly budget, the portfolio, and the tax strategy in one place — so you always have a one-page picture of where you stand. Bank exports, brokerage statements, and tax documents never leave your machine.

> Educational only. Not a substitute for a licensed CPA or CFP — especially before big moves.

## The Team

| Agent | What they do | Deliverables |
|-------|--------------|--------------|
| `@budget_analyst` | Tracks spend vs. plan, flags anomalies and lifestyle creep, watches savings rate | Monthly money-date memo, category trend chart, anomaly list, single-file HTML dashboards (via the bundled `web-artifacts` skill) |
| `@investment_advisor` | Allocation, account strategy, rebalancing rhythm (educational, not advice) | Target allocation, rebalance worksheet, contribution plan, single-file HTML allocation / drift charts (via the bundled `web-artifacts` skill) |
| `@tax_strategist` | Year-end moves, deduction tracking, quarterly estimates, CPA doc prep | Year-end checklist, harvest + Roth-conversion worksheet, CPA packet |

## Install

```bash
clawmeets init --from-url https://raw.githubusercontent.com/clawmeets-ai/clawmeets-templates/main/finance/setup.json
clawmeets start
```

Drop your bank CSV exports, brokerage statements, and last year's tax return into each agent's `knowledge_dir` on the first run — the monthly review starts calibrating to real numbers on the second pass.

## Compelling Project Requests

Drop any of these into your coordinator and watch all three agents fan out in parallel — bank exports, brokerage statements, and tax documents never leave your machine:

1. **Monthly money date** — *"Run my monthly money date off the statements in my knowledge folder. `@budget_analyst`: income and spend by category vs. plan, anomalies, lifestyle creep, savings rate, emergency-fund months, plus the top recurring charges in the last 90 days that don't look used (no second touch, no engagement signal in the statement). `@investment_advisor`: whether my contributions are on pace for the year. `@tax_strategist`: any tax-relevant items in the month I should capture now (quarterly estimate nudge, receipt to file). Each agent hands me the **ONE move** to focus on next month; coordinator picks the winner."*

2. **Monthly spending + income dashboard** *(killer demo — needs the engineering team installed for `@frontend`)* — *"Build my monthly money picture for **[Month]** from PDF statements in `budget_analyst`'s `knowledge_dir/statements/[Month]/` (mixes credit-card and bank-account PDFs). `@budget_analyst`: read every PDF, extract transactions, dedupe internal transfers, produce strict-shape `.bus-files/spending.json` and a prose `.bus-files/REPORT.md`. `@frontend`: read `spending.json` and ship `.bus-files/dashboard.html` as a single-file interactive artifact (web-artifacts skill — React + Tailwind + Chart.js via CDN) with KPIs, category drill-down, per-card breakdown, top-merchants table, anomalies, category-vs-card heatmap, and a collapsible 'Transfers excluded' panel. Coordinator: stitch and surface the ONE move that matters."*

3. **Year-end tax pack** — *"Build my year-end tax pack. All deliverables go to `.bus-files/tax/`. `@tax_strategist`: from statements + last year's return in my knowledge folder, write `harvest.md` (losses worth harvesting; wash-sale respected), `roth-conversion.md` (room before bumping a bracket), `charitable.md` (bunching + DAF), and `deadlines.md` (401k / IRA / HSA / FSA cutoffs by date). `@investment_advisor`: review the harvest plan for traps (reversed wash sales, ETF substitutes during the 30-day window). `@budget_analyst`: cash flow needed to max remaining 2026 contributions and what to cut to find it. `@tax_strategist`: write `CPA-PACKET.md` — doc checklist with what's already on hand vs. still missing. Coordinator: stitch `INDEX.md` plus the ONE move this week."*

4. **Portfolio reset** — *"Time for a portfolio reset. `@investment_advisor`: rebalance my 401k and taxable to target allocation (current positions in my knowledge folder); confirm the allocation still fits my age and goals; recommend a cadence (threshold or calendar) for next year's rebalance. `@tax_strategist`: flag tax implications of any taxable-account rebalancing (capital gains, harvesting, wash-sale traps). `@budget_analyst`: flag accounts over- or under-funded vs. last year's contribution caps. Coordinator: remind me to update beneficiaries on every account before we close."*

5. **House-purchase stress test** — *"We're considering buying a house at roughly **[target price]** with **[down-payment %]** down in the next **[N] months**. `@budget_analyst`: stress-test monthly cash flow at 3 mortgage scenarios from the statements in my knowledge folder; model the down-payment savings plan and what to cut to accelerate; flag which recurring expenses are the easiest to cut now to free up monthly cash for the down-payment runway. `@investment_advisor`: tell me which accounts to pull the down payment from (taxable vs. HYSA vs. partial Roth) and the order. `@tax_strategist`: property-tax + mortgage-interest deduction impact at each scenario (SALT cap honored)."*

6. **Job change financial reset** — *"I'm starting a new job with a **[$ amount]** change in total comp (details in my knowledge folder). `@budget_analyst`: plan the new paycheck split (savings rate target, lifestyle-creep budget) and revisit my emergency-fund target against the new monthly costs. `@investment_advisor`: max-out plan for 401k + employer match + HSA, plus the 401k rollover from my old plan (direct rollover vs. Roth conversion at rollover). `@tax_strategist`: tax-withholding adjustments (W-4 changes) and whether the new comp triggers quarterly estimates."*

## Why This Team Works

Personal-finance SaaS asks you to hand over bank credentials and aggregated transaction history. This team works the opposite way: you hand it a folder of CSV exports and PDFs, it reads them locally, and it produces the same one-page monthly memo a $5K/month advisor would — without anyone ever seeing your account balances.
