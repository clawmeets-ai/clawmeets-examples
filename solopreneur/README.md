# Solopreneur Team

A four-agent solopreneur crew that validates an idea through three iterative loops *before* you write a line of code: PMF, then pitch, then the Amazon-style announcement email. **Your** customer notes, **your** churn data, **your** prior pitches and brand work never leave your machine.

## The Team

| Agent | What they do | Deliverables |
|-------|--------------|--------------|
| `@product` *(solopreneur)* | Defines the wedge, MVP, and roadmap; defends the product hypothesis under market and investor pressure; pivots when the evidence demands it | One-line wedge, MVP scope, pitch artifact, pivot calls |
| `@market` *(solopreneur)* | Voice of the buyer — pressure-tests product hypotheses from the ICP's seat; refuses to grade on a curve | Pressure-test verdicts, buyer-language translations, yes/no PMF calls |
| `@investor` *(solopreneur)* | Critical seed/Series A check-writer — surfaces defensibility, TAM, capital efficiency, exit-path holes; "pass" is a real outcome | Pitch teardowns, defensibility checks, pass/proceed verdicts |
| `@branding` *(solopreneur)* | Mission, vision, tagline, value prop, and the Amazon-style working-backwards announcement email that has to be compelling before you build | Brand pack, announcement email, forward-test verdict |

The user's assistant orchestrates the three loops:

1. **Loop A — PMF** (`@product` ↔ `@market`) — until market signs off or no-go is called.
2. **Loop B — Pitch** (`@product` ↔ `@investor`) — runs once PMF locks; pivots back to A if a critique exposes a PMF hole.
3. **Loop C — Brand & launch narrative** (`@branding` ↔ `@product`) — runs once both lock; pivots back to A or B if the announcement email exposes a wedge or defensibility problem rather than a copy problem.

## Install

```bash
clawmeets init --from-url https://raw.githubusercontent.com/clawmeets-ai/clawmeets-templates/main/solopreneur/setup.json
clawmeets start
```

Drop existing customer notes, churn data, sales-call transcripts, prior pitch decks, competitor screenshots, and any brand work older than 6 months into each agent's `knowledge_dir` so the first request doesn't read generic.

> Template edits are not retroactive — if you already have a solopreneur team installed under the old shape, re-running `clawmeets init --from-url ...` for the same user keeps your existing agents. Wipe and re-init (or hand-register the new agents) to pick up the four-agent loop shape.

## Compelling Project Requests

Paste any of these into your coordinator. Replace the bracketed details with your own.

### Full arc — idea to launch email

> I want to ship **[product idea]**. ICP guess: **[ICP]**. Run the full arc; all deliverables go to `deliverables/arc/`.
>
> **Loop A** — `@product` and `@market` iterate on the wedge until market signs off ("a real buyer would pull this out of your hand") or we call no-go; output `PMF.md` with the wedge that survived, the ICP that survived, and the 3 strongest objections market raised with how product answered each.
>
> **Loop B** — once PMF locks, `@product` and `@investor` iterate on the pitch (problem / why-now / wedge / traction / ask); output `PITCH.md` with a "what I almost rejected and why I didn't" footer from investor; if investor's critique is a PMF hole (not a narrative gap), pivot back to Loop A and note it.
>
> **Loop C** — once both lock, `@branding` interviews `@product` and writes `BRAND.md` (mission, vision, tagline, value prop) and `ANNOUNCEMENT.md` (Amazon-style announcement email as if shipping today: customer name + problem + what they get + why-now + one customer quote that only sounds right if the product actually works); product reviews each draft and either accepts, revises, or names the failure mode (wedge / defensibility / copy); on wedge or defensibility failure, pivot back to the right loop.
>
> Coordinator: stitch `INDEX.md` — the full arc on one page (wedge → pitch → announcement) plus the ONE thing not to screw up at launch.

### PMF sprint

> I have a hypothesis: **[hypothesis]**. Run only Loop A — `@product` defends, `@market` attacks from a buyer's seat (urgency, willingness to pay, status-quo workarounds, what would actually make them switch). Iterate until market signs off or we call no-go after two rounds without movement. Output `VERDICT.md`: the hypothesis that survived (or didn't), the 3 sharpest objections market raised, how product answered each, and the single experiment I should run this week to falsify the remaining risk.

### Pitch from PMF

> PMF is locked — wedge + ICP + survived objections are in my knowledge folder under `pmf-notes.md`. Run only Loop B: `@product` drafts the pitch and iterates with `@investor`. Investor reads as a **[seed / Series A]** check-writer — passes are real outcomes. After 3 revisions or convergence, output `PITCH.md` (the version I'd send) and `CRITIQUES.md` (every objection investor raised, ordered by severity, with the answer or "still open"). If any objection turns out to be a PMF hole, stop and tell me — don't paper over it.

### Working-backwards announcement

> Product is validated by both market and investor (notes in my knowledge folder). Run only Loop C: `@branding` interviews `@product`, then writes `BRAND.md`, `ANNOUNCEMENT.md` (Amazon-style email as if shipping today: customer name + problem + what they get + why-now + one customer quote that only sounds right if the product actually works), and `FORWARD-TEST.md` (would a real customer in this ICP forward this? what's the one line most likely to make them not?). `@product` reviews each draft. On a failure mode that isn't copy (wedge unclear / defensibility soft), product names it and we pivot back to the right loop instead of more revisions.

### Pivot decision

> Investor flagged that my **[wedge]** isn't defensible — every meeting tool will ship this in 6 months (raw critique in my knowledge folder). Re-run Loop A with that constraint embedded: `@product` and `@market` iterate on a sharper wedge that survives the "incumbents will commodity this" critique. When you converge, hand off to `@investor` for one Loop B round to confirm the new wedge holds the defensibility line. Output `PIVOT.md`: what changed, what survived, what we explicitly killed, and the one objection that's still open. If you can't get past the original critique in 3 rounds, tell me — sometimes the right answer is "don't build this".

### One-liner gauntlet

> I have a one-line positioning candidate: **[tagline]**. Run it through all three seats in one short loop: `@market` (would a buyer say this in their own words?), `@investor` (does this signal a defensible wedge or a feature?), `@branding` (does this earn a forward in an announcement email?). Each iteration, `@product` revises the line. Output `FINAL.md` — the version that survived all three plus a 1-line note from each seat on why this version (vs. the prior) is sharper. Cap at 3 rounds; if no version survives, tell me which seat keeps killing it.

## Why This Team Works

Most solopreneur templates assume the idea is right and rush to "make a plan to ship it." This team flips it: three loops have to validate the idea before any code gets written. The market loop kills wedges that real buyers wouldn't pay for. The investor loop kills pitches that paper over defensibility. The branding loop is the final gate — Amazon's working-backwards trick: if you can't write a launch email a customer would forward, the product isn't ready, and the failure mode tells you which earlier loop to re-open. The result is fewer projects, sharper ones, and a real "don't build this" path that costs you a few rounds of conversation instead of six months of code.

**First-run checklist:** drop your latest sales calls, churn notes, customer interview transcripts, past pitch decks, competitor screenshots, and any prior brand work into the per-agent knowledge dirs (`./product`, `./market`, `./investor`, `./branding`) so the loops grade against your real evidence, not invented personas.
