# Information Team

Single-agent crawler that watches user-defined websites on a schedule and lands matched items in a local (network-shareable) data warehouse, addressable conversationally for ad-hoc lookup.

## The Team

| Agent | What it does | Storage owned |
|-------|--------------|---------------|
| `@website_monitor` *(website-monitor skill)* | Crawls each configured rule's entry URLs on the trigger marker via Claude Code's built-in WebFetch (falls through to `playwright-browser` for JS-rendered / login-walled sites); extracts items matching a free-text `content_of_interest` description into a fixed schema. Also responds conversationally to DMs — ad-hoc WebFetch, sample the warehouse, design a new rule. | `dwh_dir/sources/website/<rule>/<TIMESTAMP>/data.tsv` + `dwh_dir/merged/website/<rule>.tsv` (deduped) |

Output schema is fixed: `content_hash, title, summary, source_url, first_seen_at, crawled_at` — `first_seen_at` is set on insert only, so downstream consumers can detect newly-discovered items.

Starter rules ship for `nyc-culture`, `ai-news`, `burgundy-wines`, `nyc-restaurants` — URLs are blank placeholders; the operator fills them post-init.

## Install

```bash
clawmeets init --from-url https://<your-server>/templates/information/setup.json
clawmeets start
```

## Manual setup steps (after `clawmeets init`)

### 1. Set the Data Warehouse Directory

In **Agent Settings → Runner Settings → Data Warehouse Directory** (or via CLI: `clawmeets agent set-dwh-dir website_monitor /mnt/dwh`).

### 2. Configure the `website-monitor` skill

Open **Agent Settings → Skills → website-monitor → Configure pill**. The starter `rules[]` array ships with placeholder URLs and JSONC comments suggesting concrete candidates per rule (e.g. Eater for `nyc-restaurants`, Hacker News + Verge for `ai-news`). Replace the placeholders with real sites you actually want to watch.

Per-rule config lives at `$CLAWMEETS_AGENT_DIR/skill-hub/configs/website-monitor.json`. Each rule carries `website + content_of_interest + merge_policy + max_per_run/max_pages` plus the entry URLs to crawl.

### 3. (Optional) Install playwright-browser

For JS-rendered or login-walled sites WebFetch can't reach:

```bash
clawmeets bootstrap browser
```

One-time per machine. Verifies Node ≥ 20 and installs `playwright`'s Chromium.

### 4. Schedule the syncs

Each rule fires on its own trigger marker `<!-- clawmeets:<rule>-website-monitor-trigger -->`. Example:

```bash
clawmeets schedule add \
  --chatroom dm-<username>-website_monitor \
  --cron "0 * * * *" \
  --content $'<!-- clawmeets:ai-news-website-monitor-trigger -->\nHourly AI-news crawl.'
```

Or fire any rule manually from the agent's DM zero-state launchpad.
