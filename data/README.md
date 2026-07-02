# Business Data Team

Three sync workers that pull your **actual** business data — databases, Google Drive folders, partner APIs — into a local (network-shareable) data warehouse on a schedule. Each sync entry's `merge_policy` (replace or upsert) does the dedup / snapshot work, so the consolidated `merged/<source>/<name>.<ext>` is downstream-ready. A `data_scientist` reads that data (and `derived/<view>/` when present), runs hypothesis tests, builds features, runs lightweight models, and turns it all into business answers — revenue dashboards, cohort analyses, pricing memos, anomaly drill-downs, exec briefs — under `deliverables/`. When an analysis matures into something worth re-running on schedule, the scientist promotes it to a personal skill that writes to `derived/<view>/`. Sync + dedup logic lives inside each MCP (deterministic Python); science and analysis live where they belong (genuinely LLM work).

## The Team

| Agent | What they do | Storage owned |
|-------|--------------|---------------|
| `@database` *(database MCP)* | Calls `mcp__clawmeets-db__sync_to_warehouse` on the trigger marker; one per-run TSV per query (header row = SQL column names) plus a merged TSV per `merge_policy`; multi-query per agent via `{agent_dir}/mcp-hub/configs/database.json` | `dwh_dir/sources/database/<query>/<TIMESTAMP>/data.tsv` + `dwh_dir/merged/database/<query>.tsv` |
| `@gdrive` *(google-drive MCP)* | Calls `mcp__clawmeets-gdrive__sync_to_warehouse`; one file per Drive file per run plus a merged JSON array of envelopes per `merge_policy`; multi-slice per agent via `{agent_dir}/mcp-hub/configs/google-drive.json`; slice scope by `folder_ids` and/or `file_ids` and/or `sheet_tabs` (per-tab Sheets API for surgical single-tab pulls) and/or free-form `query`; small text bodies inlined, binary mimes path-only | `dwh_dir/sources/google-drive/<slice>/<TIMESTAMP>/<file_id>.json` + `dwh_dir/merged/google-drive/<slice>.json` |
| `@api` *(http-api MCP)* | Calls `mcp__clawmeets-api__sync_to_warehouse`; one per-run TSV per endpoint (header row = response row keys) plus a merged TSV per `merge_policy`; multi-endpoint per agent; auth secrets in runner env vars only | `dwh_dir/sources/api/<endpoint>/<TIMESTAMP>/data.tsv` + `dwh_dir/merged/api/<endpoint>.tsv` |
| `@data_scientist` | Reads three layers in order of stability: `derived/<view>/` (own previously-promoted views, when present), `merged/<source>/<name>.<ext>` + sibling `.howto.md`, then own prior `deliverables/` artifacts in the same chatroom. Explores, tests hypotheses, builds features, runs lightweight models, AND turns findings into business answers (cohort analyses, pricing memos, exec briefs, weekly dashboards). Ships single-file interactive HTML via the bundled `web-artifacts` skill. Default output is `deliverables/`. When an analysis is stable enough to re-run on schedule, creates a personal skill that writes to `derived/<view>/` — that's the only path by which `derived/` gets populated. | `deliverables/` (default) + `derived/<view>/` (via promoted personal skills) |

The three syncers are thin: each says "see the matching trigger marker → call the MCP tool → post a one-line summary". The bookkeeping (state files, window math, atomic writes, watermark advancement, soft time/row budgets) lives inside the MCP server in deterministic Python — not in LLM prose. Sync is read-only.

## Install

```bash
clawmeets init --from-url https://<your-server>/templates/data/setup.json
clawmeets start
```

`clawmeets init` registers the four agents but **does not** finish their setup. Three things happen after init, **once per agent**:

## Manual setup steps (after `clawmeets init`)

### 1. Set the Data Warehouse Directory on each agent

The `dwh_dir` is the on-disk root where raw rows + watermarks land. Typically a single network shared file system mount (e.g. `/mnt/dwh`) or a local path (e.g. `~/dwh`). Set it explicitly per agent in **Agent Settings → Runner Settings → Data Warehouse Directory**:

- `database` → `/mnt/dwh`
- `gdrive` → `/mnt/dwh`
- `api` → `/mnt/dwh`
- `data_scientist` → `/mnt/dwh` *(reads `merged/` + `derived/<view>/`; writes `derived/<view>/` via promoted personal skills)*

Four agents, one shared path is the typical setup — they each touch a different sub-directory.

> CLI alternative: `clawmeets agent set-dwh-dir <agent> /mnt/dwh`. The runner picks up the change on the next task (no restart needed). If `dwh_dir` is unset, the agent's profile rules say to reply *"warehouse not configured"* and stop — there's no implicit default.

### 2. Configure `database` and `api` via `{knowledge_dir}/config.json`

Both agents are MCP-driven but the MCPs need per-agent configuration: which DB to connect to and which queries to run; which API endpoints to hit and how to wire auth + watermarks into the request. Write that config **before** the first sync trigger. Knowledge dir layout:

```
~/.clawmeets/agents/<username>-database-<id>/db_syncer/
└── config.json           ← write this
```

#### Reserved tokens (used by both syncers)

Both configs use `${VAR}` placeholders. The MCP injects these reserved runtime tokens on every request and falls through to `os.environ` for everything else (so user-set env vars act as **defaults** that the MCP overrides at runtime):

| Token | Value | Notes |
|---|---|---|
| `${WATERMARK_ISO}` | sync-state high_watermark, ISO-8601 UTC | Quote in SQL: `> '${WATERMARK_ISO}'`. |
| `${WATERMARK_EPOCH}` | same as Unix epoch seconds | Numeric (don't quote). |
| `${WATERMARK_END_ISO}` / `${WATERMARK_END_EPOCH}` | window end (now) | Same conventions. |
| `${CURSOR}` | last_row[cursor_field] | Omitted on the first request → falls through to a `CURSOR` env var if you set one, else empty string. |
| `${OFFSET}` | rows-seen counter | `0` on first request, increments per page. |
| `${PAGE_SIZE}` | `pagination.page_size` from config | Numeric. |

Unset user-defined tokens (e.g. `${PG_PWD}` when `PG_PWD` is unset) cause the sync to return `status="error"` with the missing-name list. Reserved tokens with no value substitute to the empty string so opaque-cursor APIs work without forcing a sentinel.

#### `database/config.json`

```json
{
  "connection_string": "postgresql+psycopg://${PG_USER}:${PG_PWD}@${PG_HOST}:5432/mydb",
  "queries": [
    {
      "name": "orders",
      "sql": "SELECT id, customer_id, total_cents, currency, status, updated_at FROM orders WHERE updated_at > '${WATERMARK_ISO}' AND updated_at < '${WATERMARK_END_ISO}' ORDER BY updated_at ASC LIMIT ${PAGE_SIZE} OFFSET ${OFFSET}",
      "id_field": "id",
      "ts_field": "updated_at",
      "pagination": {"type": "offset", "page_size": 500},
      "merge_policy": "upsert",
      "merge_policy_upsert_id_column": "id"
    },
    {
      "name": "product_catalog",
      "sql": "SELECT sku, name, price_cents, active FROM products ORDER BY sku",
      "pagination": {"type": "single"},
      "merge_policy": "replace"
    }
  ]
}
```

`merge_policy` (default `"replace"`) tells the MCP how to fold each run's dump under `sources/database/<name>/<TIMESTAMP>/data.tsv` into the consolidated `merged/database/<name>.tsv`. `"upsert"` keeps the watermark machinery and dedupes rows by `merge_policy_upsert_id_column`. `"replace"` is a full-refresh snapshot — write a query that returns the whole source per run and the merged file is overwritten from it.

`howto` (optional) is a free-form Markdown string the data_scientist reads to understand how to consume this slice. On every sync it's mirrored to both `sources/database/<name>/howto.md` (alongside `sync-state.json`) and `merged/database/<name>.howto.md` (sibling of the TSV). Keep it short and concrete — units, encodings, join hints, gotchas — not a textbook.

The user writes raw SQL — joins, subqueries, views, anything SQL allows — and the MCP just executes it. Watermark filtering lives in your `WHERE` clause; pagination is wired via `${OFFSET}` / `${CURSOR}` / `${PAGE_SIZE}`. Substitution is naive string replacement, so **quote string-typed reserved tokens yourself** (`'${WATERMARK_ISO}'`) and don't put SQL-special characters in user env vars referenced from SQL.

Then on the runner, export the secrets (referenced via `${VAR}` in the config) and install the matching SQLAlchemy driver:

```bash
export PG_USER="readonly_user"
export PG_PWD="..."
export PG_HOST="db.internal"

pip install sqlalchemy
pip install "psycopg[binary]"   # for Postgres
# or:  pip install pymysql       # for MySQL
# SQLite is built into Python — no extra dep
```

The MCP **lazy-imports** SQLAlchemy and the driver, so the runner package stays small. If they're missing at sync time, the tool returns `status="error"` with a clear install hint.

#### `api/config.json`

```json
{
  "endpoints": [
    {
      "name": "stripe-charges",
      "url": "https://api.stripe.com/v1/charges",
      "method": "GET",
      "headers": {"Authorization": "Bearer ${STRIPE_KEY}"},
      "query_params": {
        "created[gte]": "${WATERMARK_EPOCH}",
        "starting_after": "${CURSOR}",
        "limit": "${PAGE_SIZE}"
      },
      "body": null,
      "pagination": {"type": "cursor", "cursor_field": "id", "page_size": 100},
      "row_path": "data",
      "id_field": "id",
      "ts_field": "created",
      "merge_policy": "upsert",
      "merge_policy_upsert_id_column": "id"
    }
  ]
}
```

Same `merge_policy` contract as `database` — `"upsert"` for incremental delta sources (Stripe charges, Slack messages, anything log-structured), `"replace"` for snapshot endpoints (a daily exchange-rate CSV, a roster JSON refreshed nightly).

Each endpoint is a free-form HTTP request template. Auth lives in `headers` (not a special field), watermarks land wherever the API expects them via reserved tokens, pagination cursors get wired the same way. POST endpoints with JSON bodies work the same way — put `${VAR}` placeholders inside `body`.

**Secrets live only in env vars on the runner.** The config references them via `${VAR}` placeholders inside `headers` (or anywhere else). Set the env vars before you start the runner:

```bash
export STRIPE_KEY="sk_live_..."
clawmeets start
```

The full config schema is documented in [`mcps/http-api/README.md`](../../mcps/http-api/README.md).

### 3. Install + authorize the `google-drive` MCP for `gdrive`

`google-drive` is **not** pre-installed by the template — it's installed manually because that's the path that drives Google's OAuth consent flow.

1. Open **`gdrive` Agent Settings → MCP Servers**.
2. Install `google-drive`. The runner's relay OAuth fires automatically — you'll see a "Continue with Google" link in the agent's DM.
3. Click it, complete consent in your browser, and the auth code is delivered back to the runner.

Tokens land at `~/.clawmeets/agents/<agent>-<id>/mcp-hub/servers/google-drive/token.json` (mode 0600, agent-private, never synced to the server).

> CLI fallback: `clawmeets mcp auth google-drive --agent gdrive`. Always uses the local OAuth flow regardless of `CLAWMEETS_OAUTH_MODE`.

**Required — configure named slices.** `gdrive` only syncs what the config tells it to. Each slice gets its own output directory and watermark under `dwh_dir/sources/google-drive/<name>/`, plus a merged JSON array of envelopes at `dwh_dir/merged/google-drive/<name>.json`. Click **Configure** next to `google-drive` in the same MCP panel and edit the JSON:

```json
{
  "slices": [
    {
      "name": "product-specs",
      "folder_ids": ["0AbCdEfGhIjKlMnOpQrSt"],
      "query": "mimeType = 'application/vnd.google-apps.document' and name contains 'spec'",
      "merge_policy": "upsert",
      "merge_policy_upsert_id_column": "file_id"
    },
    {
      "name": "finance-pdfs",
      "folder_ids": ["1XyZ987654321qwerty"],
      "file_ids": ["1AbCdEfGhIjKlMnOpQrStUvWxYz1234567890"],
      "query": "mimeType = 'application/pdf'",
      "merge_policy": "replace"
    },
    {
      "name": "ledger-stock-movements",
      "sheet_tabs": [
        { "file_id": "1po0V8Y2pnptys28cSBJeUsGGlpsYAjTHXa_Kl08lOIY",
          "sheet_name": "Stock Movements" }
      ],
      "merge_policy": "replace"
    }
  ]
}
```

Within a slice, `folder_ids` matches **direct parents only** (Drive limitation — list every descendant folder for nested trees), `query` is AND'd onto the watermark + folder filter, `file_ids` is an **OR-additive** third filter for hand-picked files anywhere in Drive (fetched per-id via `files.get` since Drive's `q=` has no `id` field — useful for files shared with you that aren't in any of your folders, or specs that don't share a common parent), and `sheet_tabs` is an **OR-additive** fourth filter for surgical per-tab pulls of Google Sheets via the Sheets API (each entry is `{file_id, sheet_name}` — pulls EXACTLY that tab; ideal for slicing one tab out of a multi-tab workbook where `file_ids` would dump every tab). Empty / missing config (or empty `slices`) ⇒ the sync DM is a noop. `merge_policy` (default `"replace"`): `"upsert"` keeps the per-slice watermark and folds each run's envelopes into `merged/google-drive/<slice>.json`, deduping by `merge_policy_upsert_id_column` (typically `"file_id"`); `"replace"` lists every matching file each run and rewrites the merged JSON as a snapshot. Filter changes are forward-looking under upsert — to backfill historical files under one slice's wider filter, `rm {dwh_dir}/sources/google-drive/<slice>/sync-state.json` and trigger a sync. Sibling slices are untouched. Full schema in [`mcps/google-drive/README.md`](../../mcps/google-drive/README.md).

The `database` and `http-api` MCPs are pre-installed by the template (no auth round-trip).

## Schedule the syncs

Once each syncer has its `dwh_dir` and (where applicable) its config / MCP authorized, set up cron triggers:

```bash
# Hourly DB sync, on the hour
clawmeets dm schedule <username>-database \
  $'<!-- clawmeets:db-sync-trigger -->\nHourly DB sync.' \
  --cron "0 * * * *" -u <username> -p <password>

# Hourly Drive sync, offset by 15 min
clawmeets dm schedule <username>-gdrive \
  $'<!-- clawmeets:gdrive-sync-trigger -->\nHourly Drive sync.' \
  --cron "15 * * * *" -u <username> -p <password>

# Hourly API sync, offset by 30 min
clawmeets dm schedule <username>-api \
  $'<!-- clawmeets:api-sync-trigger -->\nHourly API sync.' \
  --cron "30 * * * *" -u <username> -p <password>
```

Or fire any sync manually from the agent's DM zero-state launchpad — the **Sync** sample requests are pre-loaded with the right trigger marker and copy to your clipboard with one click.

## First-run defaults and backfilling

For **upsert** slices, day one is **empty** by design: each source / query / endpoint's `sync-state.json` initializes with `low_watermark = high_watermark = now`, so the first scheduled tick ingests nothing historical. To backfill, hand-edit `low_watermark` to a past date and fire a sync:

```bash
$EDITOR /mnt/dwh/sources/database/orders/sync-state.json
# set low_watermark to e.g. "2025-01-01T00:00:00+00:00"
# then fire <!-- clawmeets:db-sync-trigger --> in the DM
```

For **replace** slices the watermark is ignored — the user's query/request is expected to return the full source every run, and the first tick already captures everything.

The MCP tool internally caps at `max_runtime_seconds=1500` (25 min) per call. When wall-time elapses mid-run, it flushes whatever it's written, advances `high_watermark` to the last successfully written row's timestamp, and returns `has_more=true`. The next scheduled trigger picks up from the new high. **Backfills naturally span multiple cycles** — no LLM-side loop logic, no special command, just let the schedule run.

The server-side batch-timeout extension (`PERSONAL_DATA_BATCH_TIMEOUT_SECONDS = 3600`, `server/routes/messages.py`) ensures the agent's reply doesn't get killed mid-sync. It auto-matches any marker like `<!-- clawmeets:<source>-sync-trigger -->`.

## Storage layout

```
{dwh_dir}/
├── sources/                                         ← per-run dumps + state (audit/recovery)
│   ├── database/
│   │   ├── orders/
│   │   │   ├── sync-state.json
│   │   │   └── 20260513T120545Z/data.tsv            ← one folder per run; up to 5 kept
│   │   └── product_catalog/
│   │       ├── sync-state.json
│   │       └── 20260513T120545Z/data.tsv
│   ├── google-drive/
│   │   ├── product-specs/
│   │   │   ├── sync-state.json
│   │   │   └── 20260513T120545Z/
│   │   │       ├── <file_id>.json                   ← envelope per Drive file
│   │   │       └── <file_id>.tsv                    ← Sheets sidecar (per tab)
│   │   └── finance-pdfs/
│   │       └── …
│   └── api/
│       ├── stripe-charges/
│       │   ├── sync-state.json
│       │   └── 20260513T120545Z/data.tsv
│       └── fx-rates/
│           └── …
├── merged/                                          ← consolidated dataset (downstream input)
│   ├── database/
│   │   ├── orders.tsv                               ← upsert-deduped by `id`
│   │   └── product_catalog.tsv                      ← snapshot from latest run
│   ├── google-drive/
│   │   ├── product-specs.json                       ← JSON array of envelopes, sorted by ts
│   │   └── finance-pdfs.json
│   └── api/
│       ├── stripe-charges.tsv
│       └── fx-rates.tsv
└── derived/                                         ← @data_scientist's promoted views (via personal skills)
    ├── orders_with_payment/
    │   ├── etl-state.json
    │   ├── SCHEMA.md
    │   └── data.ndjson
    └── customers_360/
        ├── etl-state.json
        ├── SCHEMA.md
        └── data.ndjson
```

**TSV semantics** (database + http-api): first row is the column header,
captured from the first row's keys on slice creation. Subsequent rows are
the cell values in the same order, ``csv.QUOTE_MINIMAL``-encoded so cells
with embedded tabs / newlines / quotes round-trip cleanly through
``csv.reader(delimiter='\t')``. In **upsert** mode the merged TSV dedupes
by `merge_policy_upsert_id_column` (later runs win); in **replace** mode
the merged TSV is overwritten from each run. If the user edits the SQL or
the API response shape changes, the next sync surfaces a per-slice
header-mismatch error and leaves the merged file untouched; the user
remediates by `rm`ing the affected merged file.

Single-writer-per-path discipline: each syncer is the only writer to its `sources/<source>/<sub>/` (and the merged file it owns); the data_scientist's promoted personal skills are the only writers to `derived/<view>/`. Readers are unrestricted, so multiple runners on the same network shared file system mount can share the same warehouse safely.

## Two-layer learning

The dwh layout decouples **ingestion** from **derivation** from **analysis** so each layer can change at its own cadence:

| Layer | Owner | Cadence | What changes |
|---|---|---|---|
| `sources/<source>/<name>/<TIMESTAMP>/` + `merged/<source>/<name>.<ext>` | Syncers (db / gdrive / api) | Scheduled (hourly / nightly) | New / updated rows from the source of truth, folded into the consolidated merged file |
| `derived/<view>/` | `@data_scientist` (via promoted personal skill) | Scheduled re-runs of promoted views | Feature definitions, aggregation rules, model scores — anything mature enough to be worth recomputing on cadence |
| `deliverables/` reports | `@data_scientist` | Per-project, on-demand | Exploration memos, hypothesis tests, business answers, decisions, narratives |

If you change the feature definition for `derived/orders_with_payment/`, you don't re-sync — you edit the promoted personal skill and re-run it. If you change the business question, you don't re-promote — you re-query the existing `derived/` view or read from `merged/` directly. Each layer is independently rebuildable from the layer below.

## Deliberate non-features

- **No LLM-side `has_more` looping.** When a budget fires the MCP returns `has_more=true` and the agent reports it; the next scheduled trigger continues. Removes the only complex bit of LLM-side state from the contract.
- **No tagging / classification / aggregation in syncers** — that's the data_scientist's job (in `deliverables/`, or in promoted personal skills writing to `derived/<view>/`). Syncers are pure ingest + dedup bookkeeping.
- **No write-back to source.** Sync is read-only by convention — `api` does support POST/PUT request methods (some APIs require them for read endpoints), but the warehouse contract is one-way.
- **No credentials in config files.** Both syncers reference secrets via `${VAR}` placeholders that resolve from `os.environ` on the runner. Config files hold only placeholder names — safe to share over a network shared file system, safe to commit to a private dotfiles repo.
- **No data_scientist-direct-reads-sources/.** The data_scientist reads `merged/<source>/<name>.<ext>` (consolidated and deduped — equivalent to the raw layer for any practical purpose). If a stable view is needed for recurring questions, the right move is to promote a personal skill that recomputes from `merged/` and writes to `derived/<view>/` — not to spin up a one-off in chat every time.
- **No `derived/` writes from outside data_scientist's promoted personal skills.** Same single-writer discipline as `sources/`.
- **No account-level `dwh_dir`.** Set per-agent for now (CLI or UI). The agents typically point at the same path; we may collapse this to one account-level setting later.
