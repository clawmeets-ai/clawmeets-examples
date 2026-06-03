# Audit procedure (micro-waterfall)

This file is the canonical procedure every cycle follows. Every user
message in `user-communication` triggers exactly one cycle. Day 1 with
no deploy and day N with a live deploy run the **same** procedure — the
STATE phase branches on what's actually shipped.

Read this file together with `shared-context/PRD.md`. PRD = WHAT to
build. This file = HOW to make incremental progress.

## Pre-flight (before any phase)

If `PRD.md` is missing or any REPLACEME remains in PRD.md, reply asking
the user to fill it and skip this cycle. Do not score gaps.

## Phase 1 — STATE

For each `## Capability Tree` node and `## Definition of Done` item in
PRD.md, declare pass / fail / unknown. Cite evidence: deployed-URL
response, screenshots, file contents, most recent worker output.

If the deploy URL is empty, missing, or unreachable, the only reported
gap is "no working deployment". Emit one recovery workroom with the
relevant DevOps worker and stop here — do not proceed to MICRO PLAN.
This is the day-1 path: no codebase yet → recovery workroom = scaffold
the project.

## Phase 2 — MICRO PLAN

Pick a small batch of work tied to failed `## Definition of Done` items.

**Soft budget:** the total expected diff across the cycle stays under
~500 LOC. This is a guideline, not a hard cap — a 600-LOC cycle that
ships one coherent feature beats two artificially-split 300-LOC cycles
that each pay the seven-phase overhead. Stop adding to the batch when
the next item would clearly blow past the ballpark; that item belongs
in a follow-up cycle.

Each candidate gap should be:
  - directly tied to a failed `## Definition of Done` item,
  - achievable by one worker in this cycle (multi-step but coherent),
  - reviewable in one verification pass against the DoD.

Score every candidate before picking:
  { title, roi: 1-5, scope: S|M|L, autonomy: self|human, evidence }

Drop scope=L (architectural rewrites — split into smaller DoD items
first, or escalate to the user). Drop autonomy=human and surface to the
user instead. Examples that should always be dropped:
  - "Migrate from SQLite to Postgres" → L (architectural)
  - "Pick a final color palette" → human (taste)
  - "Wire OAuth for sign-in" → human (credentials)

Take the highest-ROI items from what remains. Prefer 1–3 workrooms (one
worker each) over a single mega-batch — verification stays cleaner.

## Phase 3 — EXECUTE

Use `create_room` to spin up exactly one workroom for the picked step.
@mention only the workers needed.

## Phase 4 — VERIFY (pre-refactor)

After workers report done, re-walk the affected `## Definition of Done`
items using the verification skill named in PRD.md (`## Setup You Own →
Verification skill`). Capture screenshots into the workroom's
`screenshots/` subdir as `pre-refactor-<step>.png`. Any failure → stop
and replan; do NOT refactor.

## Phase 5 — REFACTOR

Inspect the diff just produced. Workers patch minimally and ignore code
smell — that is your job. Delegate one cleanup step (dedupe, extract,
rename, or add a missing test). Skip ONLY if the diff is genuinely clean
— and explicitly say why.

## Phase 6 — VERIFY (post-refactor)

Re-walk the SAME `## Definition of Done` items with the same skill.
Capture `post-refactor-<step>.png`. Refactor must preserve behavior, not
just compile. Any regression → revert the refactor and log the smell to
`shared-context/AUDIT.md` for next cycle.

## Phase 7 — REFLECT

- Did this cycle move us forward? (yes/no — say why)
- Any design mistake emerging that the PM should know about?
- Continue / fix / pivot — pick one.

Append one line to `shared-context/AUDIT.md` under today's heading.
Idempotency: if today already has `[cycle N]`, increment to `N+1`.

Reply in `user-communication` with:
- a one-paragraph summary,
- a link to `AUDIT.md`,
- a bulleted list of any `autonomy=human` items the PM must act on
  (e.g. "Choose primary brand color", "Provide test Google account").
