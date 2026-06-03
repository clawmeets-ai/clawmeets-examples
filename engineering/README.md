# Engineering Team

A four-agent full-stack crew that turns a one-paragraph spec into a running feature — wireframes, API, UI, and a deploy plan — working off **your** codebase so nothing ships to a third party.

## The Team

| Agent | What they do | Deliverables |
|-------|--------------|--------------|
| `@designer` *(engineering)* | User research, wireframing, information architecture, accessibility, visual hierarchy | Wireframes, user flows, design-system notes, acceptance criteria |
| `@backend` *(engineering)* | Python/FastAPI, SQL, API design, data modeling, migrations | Endpoint spec, schema diff, migration plan, request/response contracts |
| `@frontend` *(engineering)* | React/TypeScript, component architecture, state management, responsive layout | Component tree, state plan, working UI, a11y checklist |
| `@devops` *(engineering)* | Docker, CI/CD, testing, environment setup, runbook writing | Runbook, deploy checklist, smoke-test suite, rollback plan |

## Install

```bash
clawmeets init --from-url https://raw.githubusercontent.com/clawmeets-ai/clawmeets-templates/main/engineering/setup.json
clawmeets start
```

When you create the project, open **Advanced Settings** in the New Project dialog and paste your repo's **Git Repository URL** — each agent gets its own branch-isolated sandbox so they can clone, read, modify, and commit real code instead of just producing docs.

## Compelling Project Requests

Paste any of these into your coordinator. Replace the bracketed details with your own.

### Build this feature end-to-end

> Ship **[feature: e.g. comment threads on tasks]** end-to-end. Designer: **wireframe 3 flows** and pick one with acceptance criteria. Backend: design the **endpoints and schema migration**, write the handlers with tests. Frontend: build the **components, wire the API, handle loading/error states**, keyboard-accessible. DevOps: add the **migration to CI**, write a **runbook** for rollback, and a **smoke test** that exercises the full flow post-deploy. Each agent commits to its branch; coordinator merges when all four are green.

### Brownfield refactor

> The **[module: e.g. auth middleware]** has become a hairball. Designer: audit the **user-visible impact** so the refactor doesn't break UX. Backend: map the **current call graph**, propose the **target structure**, and stage the refactor into **≤300-line PRs**. Frontend: identify any **tightly coupled UI** that needs to move in lockstep. DevOps: add **characterization tests** before the refactor starts and a **rollout plan** with a feature flag so we can revert in one commit.

### Performance regression hunt

> Page **[/route]** has slowed down noticeably (any timing exports or profiler output are in my knowledge folder; otherwise work from the code). Backend: read the relevant endpoint code + SQL, identify likely slow queries, and propose fixes ranked by effort. Frontend: review the **render path** from the repo, flag any recent changes likely to balloon **bundle size or hydration cost**, and propose what to code-split. DevOps: draft a **regression-alert plan** so this doesn't recur and add **performance budgets to CI**. Designer: review whether any recent UX changes are doing more work than they need to.

### Incident postmortem

> We had a **[Sev-2 incident: e.g. 45-minute checkout outage]** starting at **[time]**. Pull the **timeline from the logs** in knowledge, reconstruct **what happened, what we tried, and what actually fixed it**. Backend + DevOps jointly: identify the **root cause** and at least **one process gap** (alerting delay, missing dashboard, unclear runbook). Designer + Frontend: surface any **customer-facing messaging** we owe and whether our status-page copy was accurate. Output a **postmortem doc** and **3 prevention items** ranked by cost.

### Migrate a service

> We're moving **[service]** from **[old stack]** to **[new stack]**. Backend: map **feature parity**, list the **endpoints** that need to exist on day one vs what can lag. Frontend: find all **call sites** that need updating and whether the contract changes break anything. DevOps: design the **cutover** — blue/green, traffic shadowing, or dual-write — with a **concrete rollback path**. Designer: confirm there's **no UX regression** during cutover. Output a **week-by-week plan** with go/no-go gates.

### New side project from a paragraph

> I want to build **[product: e.g. a read-later app that auto-summarizes long threads]**. I have **a paragraph of intent** and nothing else. Designer: **sketch the 3 screens** that actually matter and cut everything else. Backend: design the **minimum schema and 4–5 endpoints**. Frontend: pick the **component library** and stub the screens. DevOps: set up a **dev environment I can run with one command** and a **deploy target that costs < $5/month** at demo traffic. Goal: a **working demo in a weekend**, not a perfect architecture.

## Continuous audit loop (autonomous iteration on a deployed product)

Once the team has shipped something, the same coordinator can run a
recurring **micro-waterfall audit cycle** against the live deploy:
critique what's there, score gaps by ROI, drop items that need a human,
and dispatch the highest-value low-scope work each idle cycle.

Three Markdown files in `shared-context/` drive it:

| File | What it is | Authored by |
|---|---|---|
| [`PRD.md`](./PRD_TEMPLATE.md) | Product spec — Goal / Capability Tree / machine-checkable Definition of Done / Setup You Own | You (PM). Fill every REPLACEME |
| [`AUDIT_PROCEDURE.md`](./AUDIT_PROCEDURE.md) | The seven-phase procedure (STATE → MICRO PLAN → EXECUTE → VERIFY pre-refactor → REFACTOR → VERIFY post-refactor → REFLECT). Soft budget: ~500 LOC of diff per cycle. Reused as-is across projects | Shipped here, upload as-is |
| [`AUDIT_MSG.md`](./AUDIT_MSG.md) | The recurring driver (~5 lines pointing at the two files above). Same body kicks the project off and re-fires on cron | Shipped here, upload as-is |

### Workflow

```bash
# 1. One-time per machine: install a verification skill on your assistant.
clawmeets agent install-skill <username>-assistant playwright-browser
clawmeets bootstrap browser

# 2. Get a user JWT.
TOKEN=$(clawmeets user login <username> <password>)

# 3. Create the project (no kickoff message yet — the schedule sends it).
clawmeets project create <project-name> <assistant-id> \
  "Build per shared-context/PRD.md and shared-context/AUDIT_PROCEDURE.md." \
  --git-url <repo> --team engineering --token "$TOKEN"
# Note the returned project id as PROJECT_ID.

# 4. Seed shared-context. The user-JWT branch on `file upload` records
#    each upload as coming from the project coordinator.
clawmeets file upload "$PROJECT_ID" shared-context \
  templates/engineering/PRD_TEMPLATE.md --name PRD.md --token "$TOKEN"
clawmeets file upload "$PROJECT_ID" shared-context \
  templates/engineering/AUDIT_PROCEDURE.md --token "$TOKEN"
clawmeets file upload "$PROJECT_ID" shared-context \
  templates/engineering/AUDIT_MSG.md --token "$TOKEN"

# 5. Fill every REPLACEME in PRD.md (deploy URL, verification skill,
#    DoD specifics) then re-upload to overwrite.

# 6. Schedule the recurring audit. --idle-only defers ticks while a
#    milestone workroom is in flight; --start-at <now> makes the first
#    tick the project kickoff (no separate kickoff message needed).
clawmeets schedule create "$PROJECT_ID" user-communication \
  --cron "0 * * * *" --idle-only \
  --file templates/engineering/AUDIT_MSG.md \
  --start-at "$(date -u +%Y-%m-%dT%H:%M:%SZ)" \
  --token "$TOKEN"
```

### What each cycle does

- **Day 1, no codebase:** STATE → "deploy URL unreachable" → one
  recovery workroom scaffolds the project. No MICRO PLAN this cycle.
- **Day N, live deploy:** all seven phases run. The reply in
  `user-communication` carries `=== STATE ===` … `=== REFLECT ===`
  headers; `shared-context/AUDIT.md` accumulates per-cycle reflections.
  Workrooms collect `pre-refactor-<step>.png` and
  `post-refactor-<step>.png` screenshots so the refactor sandwich is
  auditable end-to-end.
- **REPLACEME still present:** the coordinator refuses to score gaps
  and asks you to fill PRD.md. Skipped cycles do not consume turns.
- **Idle gate:** while a workroom is running, the next cron tick is
  deferred (logged as `Deferring scheduled message …: project … busy`);
  `next_fire_at` advances without touching `last_fired_at`.

The pattern is template-agnostic — point the same three-file split at
any product. `templates/engineering/` ships the canonical procedure;
your `PRD.md` is where the product-specific content lives.

## Why This Team Works

The power here isn't any individual agent — it's what happens when four specialists argue in parallel off your real code. The designer flags a UX issue the backend engineer didn't realize they were baking in. The frontend engineer notices the API shape would force a cascade of prop drilling. The devops engineer catches that the migration plan assumes a blue/green path the current infra doesn't support. You get the tension of a real engineering team without context-switching between humans. Because the sandboxes are branch-isolated clones of your actual repo, the output isn't just a markdown file — it's a commit you can review.

**First-run checklist:** set a `Git Repository URL` in New Project → Advanced so agents can read actual code, not infer structure from docstrings. Seed each agent's `knowledge_dir` with a `CLAUDE.md` that names the relevant files they should always consult (schema path for backend, design-tokens file for frontend, deploy.yml for devops).
