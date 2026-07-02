# Personal Data Warehouse Team

Six workers — five sync agents that pull your real data into a local
(network-shareable) data warehouse on a schedule **and** answer
interactive DMs (search inbox, compose mail, look up next meeting, list
albums, inspect inbox rows), plus a derivation agent that scouts the
warehouse and proposes ETL rules for downstream cross-template
consumption. Three of the syncers ride **standard, provider-agnostic
protocols** (IMAP+SMTP for mail, CalDAV for calendar, macOS Photos for
photos); `@website_monitor` crawls user-chosen sites via WebFetch with
LLM judgment against a free-text `content_of_interest`; `@gdrive_inbox`
tails Google Sheets that external trigger services (IFTTT, Zapier,
Google Apps Script) append rows to — the right shape for ingesting
push-driven event streams like Ring doorbell motion or
parcel-delivery notifications without standing up a webhook server.
Pairs with the `etl` skill to derive cleaned views, without your data
ever leaving your network.

## The Team

| Agent | Protocols | What they do | Storage owned |
|-------|-----------|--------------|---------------|
| `@mailbox` | IMAP + SMTP (`mailbox` MCP) | Sync new mail on the trigger marker; interactive search / get / send | `dwh_dir/{sources,merged}/mailbox/<slice>/` (one slice per IMAP folder) |
| `@calendar` | CalDAV (`calendar` MCP) | Sync new + updated events on the trigger marker; interactive list / get / create / update / delete | `dwh_dir/{sources,merged}/calendar/<slice>/` (one slice per CalDAV calendar) |
| `@photo` | macOS Photos (`osxphotos` MCP) | Sync newly-added photo metadata on the trigger marker; interactive list albums / list photos / export | `dwh_dir/sources/osxphotos/` + `dwh_dir/merged/osxphotos.json` (single dataset) |
| `@website_monitor` | WebFetch (`website-monitor` skill) | Crawl per-rule entry URLs on the trigger marker; extract items matching a free-text `content_of_interest` description | `dwh_dir/{sources,merged}/website/<rule>/` (one rule per watched site theme) |
| `@gdrive_inbox` | Google Sheets push (`google-drive` MCP) | Sync IFTTT/Zapier-fed Sheet tabs on the trigger marker; interactive sample / inspect rows; one slice per tab | `dwh_dir/{sources,merged}/google-drive/<slice>/` (one slice per Sheet tab) |
| `@data_organizer` | (no MCP yet) | Walks `merged/`, proposes ETL rules, samples candidate rows, reports on `derived/` state — the seat for the derivation layer while the skills are designed | `dwh_dir/derived/<rule>/` (writes once skills land) + `deliverables/` (surveys, proposals today) |

All five sync agents run in **two modes**: scheduled sync (one tool
call, one-line reply) on a `<!-- clawmeets:<source>-sync-trigger -->`
DM, or conversational on any other DM. The sync bookkeeping (state
file, window math, atomic writes, watermark advancement, soft time/row
budgets) lives inside each MCP / skill in deterministic Python — not in
LLM prose. `@data_organizer` is conversational only today and gains
trigger-marker skills in a follow-up.

## Install

```bash
clawmeets init --from-url https://<your-server>/templates/personal_data/setup.json
clawmeets start
```

`clawmeets init` registers the three agents and installs the matching
MCPs. It does **not** finish their setup — the data warehouse needs a
path you control, and the mailbox + calendar MCPs each need a
`config.json` plus runner env vars for credentials. All of that has to
happen after init.

## Manual setup steps (after `clawmeets init`)

### 1. Set the Data Warehouse Directory on each agent

The `dwh_dir` is the on-disk root where raw rows + watermarks land —
typically a single network shared file system mount (e.g. `/mnt/dwh`) or a local path
(e.g. `~/dwh`). Set it per agent in **Agent Settings → Runner Settings →
Data Warehouse Directory**:

- `mailbox` → `/mnt/dwh`
- `calendar` → `/mnt/dwh`
- `photo` → `/mnt/dwh`
- `website_monitor` → `/mnt/dwh`
- `gdrive_inbox` → `/mnt/dwh`

Five agents, one shared path is the typical setup — they each own a
different sub-directory under `dwh_dir/sources/`.

> CLI alternative: `clawmeets agent set-dwh-dir <agent> /mnt/dwh`. The
> runner picks up the change on the next task (no restart needed).

### 2. Configure `mailbox`, `calendar`, `photo`, and `gdrive_inbox`

Each MCP reads its config from a per-agent file at
`{agent_dir}/mcp-hub/configs/<mcp>.json`. Edit it from the **Configure**
button on the MCP's row in **Agent Settings → MCPs** — the modal opens
a JSON editor pre-filled with the starter template (comments + slice
examples included). Saves write through to disk automatically; no SSH.

`folders_to_sync` (mailbox) and `calendars_to_sync` (calendar) are now
**lists of named slices**, each binding one IMAP folder / CalDAV
calendar to its own warehouse dataset. Empty list ⇒ no-op (so a fresh
install before configuration is harmless on the first scheduled trigger).

#### `mailbox` — `{agent_dir}/mcp-hub/configs/mailbox.json`

```json
{
  "imap": {
    "host": "imap.fastmail.com",
    "port": 993,
    "ssl": true,
    "username": "${MAILBOX_USERNAME}",
    "password": "${MAILBOX_PASSWORD}"
  },
  "smtp": {
    "host": "smtp.fastmail.com",
    "port": 587,
    "starttls": true,
    "username": "${MAILBOX_USERNAME}",
    "password": "${MAILBOX_PASSWORD}",
    "from": "${MAILBOX_USERNAME}"
  },
  "folders_to_sync": [
    {
      "name": "inbox",
      "folder": "INBOX",
      "merge_policy": "upsert",
      "merge_policy_upsert_id_column": "message_id_hash",
      "howto": "..."
    }
  ]
}
```

Add additional slices (`{name: "sent", folder: "[Gmail]/Sent Mail", ...}`)
to fan out into separate per-folder datasets. See `mcps/mailbox/README.md`
for the full slice schema.

#### `calendar` — `{agent_dir}/mcp-hub/configs/calendar.json`

```json
{
  "caldav": {
    "url": "https://caldav.fastmail.com/dav/calendars/user/me@example.com/",
    "username": "${CALENDAR_USERNAME}",
    "password": "${CALENDAR_PASSWORD}"
  },
  "sync_lookback_days": 90,
  "sync_lookahead_days": 365,
  "calendars_to_sync": [
    {
      "name": "personal",
      "calendar": "Personal",
      "merge_policy": "upsert",
      "merge_policy_upsert_id_column": "uid",
      "howto": "..."
    }
  ]
}
```

#### `osxphotos` — `{agent_dir}/mcp-hub/configs/osxphotos.json` (optional)

osxphotos has no slice list (single Photos library on the host). Its
config carries only an optional `howto` field — leave the file empty /
unconfigured for vanilla indexing.

```json
{ "howto": "..." }
```

#### `gdrive_inbox` — `{agent_dir}/mcp-hub/configs/google-drive.json`

`gdrive_inbox` uses the same `google-drive` MCP as other Drive-syncing
agents but is purpose-named "inbox" because in this template its slices
point at Google Sheets that **external trigger services append rows to**
— IFTTT, Zapier, Google Apps Script — rather than at user-authored Drive
folders. Each slice binds **one Sheet tab** via `sheet_tabs`:

```json
{
  "slices": [
    {
      "name": "ring-motion",
      "sheet_tabs": [
        { "file_id": "1abcDEF...your sheet id...XYZ", "sheet_name": "ring_motion" }
      ],
      "merge_policy": "upsert",
      "merge_policy_upsert_id_column": "tab_id",
      "howto": "One row per Ring doorbell motion event, appended by an IFTTT applet. Columns: event_id (= {{OccurredAt}}), ts, device_name, event_kind, image_url (short-lived signed URL — refetch within minutes), raw. Downstream ETL watermarks by event_id."
    }
  ]
}
```

Auth uses Google OAuth — click the **Authorize** button on the
`google-drive` row in **Agent Settings → MCPs** to seed
`{agent_dir}/mcp-hub/servers/google-drive/token.json`. No `${VAR}` env
vars required. Add additional slices to ingest other IFTTT/Zapier-fed
sheets (parcel deliveries, smart-lock unlocks, GitHub stars, etc.) — one
slice per tab.

See [Push-driven inbox setup (Ring example)](#push-driven-inbox-setup-ring-example)
below for the full Ring → IFTTT → Sheet → warehouse walkthrough.

### 3. Export credentials as env vars on the runner

```bash
export MAILBOX_USERNAME="me@example.com"
export MAILBOX_PASSWORD="..."          # see provider notes below
export CALENDAR_USERNAME="me@example.com"
export CALENDAR_PASSWORD="..."

clawmeets start
```

The MCPs resolve `${VAR}` placeholders from `os.environ` at runtime.
Missing vars surface as a clean `unset env vars: [...]` error envelope —
the agent will tell you what to export.

### 4. (Photos only, macOS) Grant Full Disk Access

The `osxphotos` MCP reads `~/Pictures/Photos Library.photoslibrary`,
which is TCC-protected. Grant **Full Disk Access** to your Python
interpreter:

```bash
which python3   # → e.g. /Users/you/.pyenv/versions/3.14.3/bin/python3.14
```

Open **System Settings → Privacy & Security → Full Disk Access**, click
`+`, paste the path, toggle on, restart the runner.

Also install osxphotos in the runner's Python:

```bash
pip install osxphotos
```

## Provider notes

### Gmail (mailbox)
- Enable IMAP in Gmail settings.
- Generate an [App Password](https://myaccount.google.com/apppasswords)
  (requires 2FA) — Google blocks plain password auth.
- `imap.host = imap.gmail.com`, `smtp.host = smtp.gmail.com`.

### iCloud (mailbox + calendar)
- Generate an [App-Specific Password](https://appleid.apple.com/account/manage).
- `imap.host = imap.mail.me.com`, `smtp.host = smtp.mail.me.com`.
- `caldav.url = https://caldav.icloud.com`.

### Fastmail (mailbox + calendar)
- Generate an [App Password](https://www.fastmail.com/settings/security/devicekeys).
- `imap.host = imap.fastmail.com`, `smtp.host = smtp.fastmail.com`.
- `caldav.url = https://caldav.fastmail.com/dav/calendars/user/<your-email>/`.

### Outlook / Microsoft 365 (mailbox)
- Generate an [App Password](https://account.microsoft.com/security)
  (requires 2FA).
- `imap.host = outlook.office365.com`, `smtp.host = smtp.office365.com`.

### ProtonMail (mailbox)
- Run [ProtonMail Bridge](https://proton.me/mail/bridge); use the
  Bridge's local IMAP/SMTP creds (typically `127.0.0.1:1143` / `1025`).

### Nextcloud (calendar)
- Use account password or an app password.
- `caldav.url = https://<host>/remote.php/dav/calendars/<username>/`.

### Self-hosted (Dovecot/Postfix, Radicale, mailcow, Mailu, SOGo)
- Plug in your server's host/port/credentials.

## Push-driven inbox setup (Ring example)

`gdrive_inbox` shines on signals that originate **outside** your machine
and that some upstream service can be coaxed into shipping to a Google
Sheet. The canonical example is a Ring doorbell: motion events fire on
the device, IFTTT (or Zapier) translates them into Sheet rows, and
`gdrive_inbox` ingests rows on a schedule — no Ring API auth, no
webhook server, no rotating refresh tokens, no battery impact from
periodic `get_snapshot()` calls.

The same pipe absorbs any other webhookable trigger: GitHub starred
repos, Stripe charges, parcel-delivery notifications, smart-lock
unlocks, weather threshold alerts. One slice per Sheet tab.

### 1. Create the destination Google Sheet

Make a new Sheet (any account that `gdrive_inbox` has OAuth access to).
Add a tab named `ring_motion` with a single header row:

```
event_id    ts    device_name    event_kind    image_url    raw
```

Note the Sheet's file id — it's the path segment after `/d/` in the
Drive URL (`docs.google.com/spreadsheets/d/<file_id>/edit`).

### 2. Configure the IFTTT applet

Ring requires **IFTTT Pro** (~$3.99/mo as of writing). On
[ifttt.com](https://ifttt.com/create):

- **Trigger**: Ring service → "New motion detected" (or "New ring") →
  pick your doorbell.
- **Action**: Google Sheets service → "Add row to spreadsheet" → pick
  the Sheet above and `ring_motion` tab.
- **Ingredient mapping** (tab-separated, in order matching the header):
  - `event_id` → `{{OccurredAt}}`
  - `ts` → `{{OccurredAt}}`
  - `device_name` → `{{DeviceName}}`
  - `event_kind` → `motion` (or `{{Event}}` if the Ring trigger exposes
    it for your applet)
  - `image_url` → leave empty, or `{{ImageUrl}}` if your applet exposes
    it (Ring's signed image URLs are short-lived — usually unusable by
    the time the next sync cycle runs)
  - `raw` → empty or `{{TriggerName}}: {{OccurredAt}}`

Save the applet. Trigger a motion event manually (walk past the
doorbell) and confirm a new row lands in the Sheet within ~1–2 minutes.

### 3. Wire `gdrive_inbox`

Authorize the `google-drive` MCP on the agent via **Agent Settings →
MCPs → google-drive → Authorize**, then open the **Configure** pill on
the same row and paste:

```json
{
  "slices": [
    {
      "name": "ring-motion",
      "sheet_tabs": [
        { "file_id": "1abcDEF...your sheet id...XYZ", "sheet_name": "ring_motion" }
      ],
      "merge_policy": "upsert",
      "merge_policy_upsert_id_column": "tab_id",
      "howto": "One row per Ring doorbell motion event, appended by an IFTTT applet. Columns: event_id (= {{OccurredAt}}), ts, device_name, event_kind, image_url, raw. Downstream ETL watermarks by event_id."
    }
  ]
}
```

Save. Then DM the agent with the trigger marker to test:

```
<!-- clawmeets:gdrive-inbox-sync-trigger -->
Sync IFTTT-fed inbox sheets.
```

You should get back a one-line summary: `status / rows_written / window /
watermarks` plus a per-slice breakdown. Confirm
`{dwh_dir}/merged/google-drive/ring-motion.tsv` contains a header row +
data rows mirroring the Sheet.

### 4. Schedule it

A 10-minute cycle is a sensible default — sheets are tiny, the only
real cost is one Drive metadata call per slice on empty cycles. See the
[Schedule the syncs](#schedule-the-syncs) section for the
`clawmeets schedule add` command.

### Caveats

- **End-to-end latency** is dominated by IFTTT's polling/dispatch and
  the sync cadence — typically motion → row in `merged/` lands in
  5–15 min. Not real-time alerting.
- **`{{ImageUrl}}` is a short-lived signed URL** and likely expired by
  the time the next sync cycle reads it. If you want a persisted
  thumbnail, add a downstream `etl` rule on `@data_organizer` that
  fetches the URL during the same sync window and writes a local jpg —
  or accept metadata-only and skip the image.
- **Row-level dedup is downstream**, not in sync. `gdrive_inbox` does
  tab-level upsert (the merged TSV is rewritten from the Sheet's
  current contents each cycle). If IFTTT ever double-fires, the
  duplicate row stays in the Sheet (and the TSV) until ETL filters by
  `event_id`. Most applets don't double-fire; just be aware.
- **IFTTT Pro tier required** for Ring as a trigger. Free tier caps at
  2 applets and excludes Ring.

## Schedule the syncs

Once each agent has its `dwh_dir`, `config.json`, and env vars, set up
cron triggers:

```bash
# Hourly mail sync
clawmeets dm schedule <username>-mailbox \
  $'<!-- clawmeets:mailbox-sync-trigger -->\nHourly mail sync.' \
  --cron "0 * * * *" -u <username> -p <password>

# Hourly calendar sync, offset 15 min
clawmeets dm schedule <username>-calendar \
  $'<!-- clawmeets:calendar-sync-trigger -->\nHourly calendar sync.' \
  --cron "15 * * * *" -u <username> -p <password>

# Nightly photo index, 3 AM
clawmeets dm schedule <username>-photo \
  $'<!-- clawmeets:photo-sync-trigger -->\nNightly photo index.' \
  --cron "0 3 * * *" -u <username> -p <password>

# IFTTT/Zapier-fed Sheet inbox, every 10 min
clawmeets dm schedule <username>-gdrive_inbox \
  $'<!-- clawmeets:gdrive-inbox-sync-trigger -->\nSync IFTTT-fed inbox sheets.' \
  --cron "*/10 * * * *" -u <username> -p <password>
```

Or fire any sync from the agent's DM zero-state launchpad — the **Sync**
sample requests pre-load the right trigger marker and copy to your
clipboard with one click.

## Watermark semantics

| Source | Filter field | What it catches |
|---|---|---|
| Mailbox | `INTERNALDATE` (server-assigned arrival time) | Mail received since the last sync. IMAP day-rounded SINCE search; Python re-filters to exact precision. |
| Calendar | `LAST-MODIFIED` (event update time, fallback `DTSTAMP`) | Events created OR modified since the last sync — including events scheduled for past dates that someone moved, RSVP'd, or invited new attendees to. |
| Photos | `added_date` (when imported into Photos.app) | Photos added since the last sync, even if their EXIF date is years old. |
| Gdrive inbox | Parent Sheet's `modifiedTime` (Drive API; no per-tab mtime) | Tab is re-pulled whenever any row append touches the parent Sheet. Empty syncs cost one Drive metadata call per slice; row-level dedup is downstream. |

## First-run defaults and backfilling

Day one is **empty**: each slice's `sync-state.json` initializes with
`low_watermark = high_watermark = now`, so the first scheduled tick
ingests nothing historical. Three ways to backfill:

1. **Preferred:** set `start_at` (ISO-8601) on the slice in its config
   *before* the first sync runs. Consulted only on the first sync —
   ignored once `last_sync_at` has been stamped. Re-apply by `rm`-ing
   the slice's `sync-state.json` and re-firing.
2. Switch the slice to `merge_policy: "replace"` for one cycle (every
   matching row pulled, watermark ignored) — heavyweight on large IMAP
   folders, fine for calendars and Drive.
3. Hand-edit `low_watermark` / `high_watermark` in
   `{dwh_dir}/sources/<source>/<slice>/sync-state.json` to a past date,
   then fire a sync trigger.

Each MCP caps each call at a wall-clock budget (default 1500 s = 25
min). When the budget fires mid-run, the slice flushes whatever it's
written, advances its `high_watermark` to the latest row's timestamp,
and returns `has_more=true`. The next scheduled trigger continues
naturally — backfills span multiple cycles without LLM-side loop logic.
The server-side batch-timeout extension ensures the agent's reply
doesn't get killed mid-sync.

## Derivation layer (in progress)

The point of the warehouse is to feed **derivation skills** that
classify, extract, and normalize the raw sync output into views
downstream agents in other templates can consume.

`@data_organizer` owns this layer. The contract:

- **Read-only on `sources/` and `merged/`.** The sync agents are the
  only writers there.
- **One rule = one directory** under `dwh_dir/derived/<rule>/`. Each
  rule owns its own `etl-state.json` with independent `etl_low/etl_high`
  watermarks; the sync layer's watermarks are separate again.
- **Image-based provenance on every row.** Every NDJSON row carries an
  `image_paths[]` pointing into `derived/<rule>/sources/<row_key>/` so
  downstream UIs render "here's what this row came from" without
  branching on source type.
- **Closed-vocab fields** wherever a row participates in categorical
  analysis (`form_type`, `event_type`, `purpose`, etc.); `misc`/`other`
  is the escape hatch.

### Intended cross-template handoff

| Derived view | Consumer | Used for |
|---|---|---|
| `dwh_dir/derived/receipts/data.ndjson` | `@budget_analyst` (finance template) | Monthly spend dashboards, anomaly detection, lifestyle-creep tracking |
| `dwh_dir/derived/tax_statements/data.ndjson` | `@tax_strategist` (finance template) | Year-end CPA packet, deduction tracking, quarterly estimate inputs |
| `dwh_dir/derived/food_logs/data.ndjson` | `@nutritionist` (wellness template) | Eating-pattern analysis, restaurant frequency, macro estimates |

Consumer templates need `dwh_dir` set on their agents to actually read
these — that wiring is a follow-up after the skills themselves land.

### Status today

Most rule skills not shipped yet. `@data_organizer` runs the four
pre-skill workflows from its sample requests:

1. **Survey the warehouse** — row counts + headline categories per
   source → `deliverables/dwh-survey.md`
2. **Propose ETL rules** — 2–3 derivation rules with filter + schema +
   intended consumer → `deliverables/etl-proposals.md`
3. **Sample candidate rows for a rule** — sanity-check the filter on
   real source rows before committing a SKILL.md
4. **Derivation-layer status** — read each `derived/<rule>/etl-state.json`
   when rules exist (no-op until then)

#### `etl` (shipped, config-driven)

General-purpose derived-table ETL. Reads JSON/TSV sources from
`dwh_dir/merged/`, runs a per-rule prompt against new rows (`upsert`
mode) or the full source (`replace` mode), and writes a derived TSV
at `dwh_dir/derived/<rule>.tsv` against a config-defined column
schema. Rules live in `$CLAWMEETS_AGENT_DIR/skill-hub/configs/etl.json`.
Trigger: `<!-- clawmeets:<rule>-etl-trigger -->` in DM — the `<rule>`
segment selects which rule to run; per-source watermarks at
`dwh_dir/derived/<rule>.sync-state.json` for `upsert` rules.

Install + first run:

```bash
clawmeets skill install data_organizer etl
# Open Agent Settings → @data_organizer → Skills → etl → Configure.
# Edit the starter `rules[]` array (inventory rollup, business
# receipts, photo tags shipped as examples) and Save.
# DM @data_organizer: <!-- clawmeets:photo-tags-etl-trigger -->
# (or any other configured rule name)
```

Replaces the previous per-source skills (`receipts-etl`,
`photo-tagger`) with a single config-driven dispatcher. Deterministic
aggregations (e.g. inventory rollup) delegate to a personal code-
template skill from the rule's `prompt`, e.g.
`Invoke /personal:inventory-rollup...`.

## Storage layout

```
{dwh_dir}/
├── sources/
│   ├── mailbox/<slice>/
│   │   ├── sync-state.json
│   │   ├── howto.md                                  (mirrored from cfg.howto, if set)
│   │   └── <YYYYMMDDTHHMMSSZ>/<message_id_hash>.json (per-run dump; up to KEEP_RECENT_DUMPS retained)
│   ├── calendar/<slice>/
│   │   ├── sync-state.json
│   │   ├── howto.md
│   │   └── <YYYYMMDDTHHMMSSZ>/<event_uid_safe>.json
│   └── osxphotos/                                    (no slice tier — single dataset)
│       ├── sync-state.json
│       ├── howto.md
│       └── <YYYYMMDDTHHMMSSZ>/<uuid>.json
├── merged/
│   ├── mailbox/<slice>.json                          (consolidated JSON array, sorted by `ts`)
│   ├── mailbox/<slice>.howto.md                      (mirrored sibling)
│   ├── calendar/<slice>.json
│   ├── calendar/<slice>.howto.md
│   ├── osxphotos.json
│   └── osxphotos.howto.md
└── derived/                                          (etl skill output, one file family per rule)
    ├── <rule>.tsv                                     (header + rows; columns from cfg.rules[].columns)
    ├── <rule>.sync-state.json                         (per-source high_watermarks + last_run_*)
    └── <rule>.howto.md                                (mirrored from cfg.rules[].howto)
```

Two layers, one purpose:

- **`sources/`** — per-run timestamped dumps. Each slice owns one
  directory. Up to `KEEP_RECENT_DUMPS` (currently 5) timestamp folders
  are retained per slice for audit/recovery; older ones are GC'd after
  each successful merge.
- **`merged/`** — the consolidated dataset rebuilt after every sync,
  per the slice's `merge_policy`. **This is the source of truth for
  downstream consumers.** ETL skills and analyst queries should read
  `merged/`, not `sources/`.

`howto.md` files are mirrored from each slice's optional `howto` field
on every successful sync; downstream consumers read them before writing
queries to learn each dataset's units / encoding / join shape.

Single-writer-per-slice discipline: each MCP is the only writer to its
own subtree. Readers are unrestricted, so multiple runners on the same
network shared file system can share the same warehouse safely.

## Deliberate non-features

- **No OAuth.** Credential-only via runner env vars. The Gmail / Google
  Calendar MCPs (installable per-agent via the MCP catalog) remain the
  path for users who prefer OAuth.
- **No LLM-side `has_more` looping.** The next scheduled trigger
  continues — keeps the only complex bit of state out of the LLM
  contract.
- **No write-back to source during sync.** Sync triggers never call
  `send_message`, `create_event`, `export_photo`. Writes are only valid
  in interactive mode.
- **No cross-source joining in v1.** One source per derivation rule;
  joining (mail × calendar) is a v2 ETL skill.
- **No account-level dwh_dir.** Set per-agent (CLI or UI) for now.
