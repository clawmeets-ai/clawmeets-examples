# Personal Finance Team

A four-agent personal-CFO crew that holds the monthly budget, the portfolio, the tax strategy, and every recurring bill in one place — so you always have a one-page picture of where you stand. Bank exports, brokerage statements, and tax documents never leave your machine.

> Educational only. Not a substitute for a licensed CPA or CFP — especially before big moves.

## The Team

| Agent | What they do | Deliverables |
|-------|--------------|--------------|
| `@budget_analyst` | Tracks spend vs. plan, flags anomalies and lifestyle creep, watches savings rate | Monthly money-date memo, category trend chart, anomaly list |
| `@investment_advisor` | Allocation, account strategy, rebalancing rhythm (educational, not advice) | Target allocation, rebalance worksheet, contribution plan |
| `@tax_strategist` | Year-end moves, deduction tracking, quarterly estimates, CPA doc prep | Year-end checklist, harvest + Roth-conversion worksheet, CPA packet |
| `@bills_auditor` | Audits recurring charges, hunts price hikes, shops insurance, renegotiates services | Subscription cancel list, insurance quote compare, annual-savings total |

## Install

```bash
clawmeets init --from-url https://raw.githubusercontent.com/clawmeets-ai/clawmeets-templates/main/finance/setup.json
clawmeets start
```

Drop your bank CSV exports, brokerage statements, and last year's tax return into each agent's `knowledge_dir` on the first run — the monthly review starts calibrating to real numbers on the second pass.

## Compelling Project Requests

Drop any of these into your coordinator and watch all four agents fan out in parallel — bank exports, brokerage statements, and tax documents never leave your machine:

1. **Monthly money date** — *"Run my monthly money date off the statements in my knowledge folder. `@budget_analyst`: income and spend by category vs. plan, anomalies, lifestyle creep, savings rate, emergency-fund months. `@bills_auditor`: subscriptions I haven't used in 90 days with cancel-by dates. `@investment_advisor`: whether my contributions are on pace for the year. `@tax_strategist`: any tax-relevant items in the month I should capture now (quarterly estimate nudge, receipt to file). Each agent hands me the **ONE move** to focus on next month; coordinator picks the winner."*

2. **Year-end tax push** — *"Year-end is approaching. Run my tax review against the statements in my knowledge folder: harvest losses in my taxable account (respect wash-sale), calculate Roth-conversion room, list charitable bunching options, flag the 401k / IRA / HSA deadlines I should hit, and prep a clean doc-collection folder so my CPA gets everything in one packet."*

3. **Portfolio reset** — *"Rebalance my 401k and taxable to target allocation (current positions in my knowledge folder), review whether the allocation still fits my age and goals, recommend a cadence for next year's rebalance, remind me to update beneficiaries on every account, and flag which accounts are over- or under-funded based on last year's contribution caps."*

4. **House-purchase stress test** — *"We're considering buying a house at roughly **[target price]** with **[down-payment %]** down in the next **[N] months**. Stress-test our budget at 3 mortgage scenarios, model the down-payment savings plan and what we'd cut to accelerate, calculate the property-tax + interest deduction impact, and audit which subscriptions and bills to kill now to free up cash."*

5. **Find money audit** — *"Audit every recurring charge in the statements I uploaded. `@bills_auditor`: unused-subscription list with cancel-by dates, streaming / software price hikes, insurance premium benchmarks, internet / mobile plans above market, HYSA suggestions for idle cash, projected annual savings total. `@budget_analyst`: cross-check the flagged charges against my actual usage patterns so we don't cut something I'll re-subscribe to in a month. `@tax_strategist`: flag any canceled items that were tax-deductible and whether any replacement should be routed through an HSA, FSA, or business account instead."*

6. **Job change financial reset** — *"I'm starting a new job with a **[$ amount]** change in total comp (details in my knowledge folder). Plan the new paycheck split (401k contribution to max + match, savings rate target, lifestyle-creep budget, HSA contribution), flag the 401k rollover from my old plan, recommend tax-withholding adjustments and whether I need quarterlies, and revisit my emergency-fund target against the new monthly costs."*

## Why This Team Works

Personal-finance SaaS asks you to hand over bank credentials and aggregated transaction history. This team works the opposite way: you hand it a folder of CSV exports and PDFs, it reads them locally, and it produces the same one-page monthly memo a $5K/month advisor would — without anyone ever seeing your account balances.
