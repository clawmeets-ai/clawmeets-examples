# PRD — <product name>

> Fill every REPLACEME below before scheduling the audit loop.
> The coordinator reads this file as the project's source of truth.

## Goal
REPLACEME — One-sentence objective. Measurable success criteria.

## Capability Tree
Capabilities, NOT tasks. Hierarchical bullets, no sequencing.
- REPLACEME (e.g. Authentication → login, session handling)
- REPLACEME (e.g. Cards → create, edit, delete, drag)

## Constraints
REPLACEME — Tech stack, scale, API shape, deployment target.

## Definition of Done
MACHINE-CHECKABLE per capability — phrased so the audit can replay it.
- REPLACEME (e.g. Cards.create: POST /cards returns 201; UI shows the new card without reload)

## Open Questions
- REPLACEME (or "none yet")

## Setup You Own
The audit loop assumes these are done before it runs.

### Public deploy URL
REPLACEME — paste the URL the audit loop should hit.
Examples: ngrok premium reserved subdomain, `*.trycloudflare.com`,
`*.loca.lt` (free localtunnel shows a friend-check interstitial that
blocks unattended automation; paid hosts skip it).

### Verification skill
REPLACEME — name of the skill installed on your assistant for verifying
the deployed UI. Recommended: `playwright-browser`.
Install with:
    clawmeets agent install-skill {username}-assistant playwright-browser
    clawmeets bootstrap browser   # one-time per machine

### Auth / secrets
REPLACEME — if the deployed app requires login, drop test-account
credentials into `personal-skill-hub/_playwright/storage/<host>.json`.
