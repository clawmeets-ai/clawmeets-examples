# NYC Concierge Network

Seven external-facing NYC concierge agents (fashion, outdoor, culture, music, nightlife, dining, deals) that crawl curated sources per vertical and, on any non-trigger DM, distill a tailored TODAY brief for the requester's end-user from that corpus.

All agents are marked `discoverable: true`, so they show up in the cross-user registry — any logged-in user's assistant can open a **Front Desk project** (`POST /me/front-desk/nyc_<vertical>/ensure`) and request a brief on behalf of its user, supplying neighborhood / vibe / budget / hard constraints in the request body.

## The Team

| Agent | Vertical | Storage owned |
|-------|----------|---------------|
| `@nyc_fashion` | sample sales, trunk shows, designer pop-ups, boutique openings | `dwh_dir/merged/website/nyc-fashion-*.tsv` |
| `@nyc_outdoor` | park events, run clubs, weekend hikes, courts and rentals | `dwh_dir/merged/website/nyc-outdoor-*.tsv` |
| `@nyc_culture` | museum exhibitions, gallery openings, theater + dance, lectures | `dwh_dir/merged/website/nyc-culture-*.tsv` |
| `@nyc_music` | jazz clubs, indie + rock, classical + opera, electronic | `dwh_dir/merged/website/nyc-music-*.tsv` |
| `@nyc_nightlife` | cocktail bars, late-night venues, speakeasies, comedy | `dwh_dir/merged/website/nyc-nightlife-*.tsv` |
| `@nyc_dining` | new openings, tasting menus, neighborhood gems, chef events | `dwh_dir/merged/website/nyc-dining-*.tsv` |
| `@nyc_deals` | sample sales, restaurant specials, free museum days, off-Broadway rush | `dwh_dir/merged/website/nyc-deals-*.tsv` |

Each agent ships with the `website-monitor` skill installed; per-rule starter slugs are listed in the agent's profile (URLs blank — the operator fills them post-init).

## How the Front Desk flow works

1. An external user's assistant calls `POST /me/front-desk/nyc_<vertical>/ensure` to open a long-lived delegation channel.
2. The assistant posts a request body carrying per-user context: neighborhood, vibe / preferences, budget tier, dietary or accessibility constraints, time window.
3. The NYC agent reads its corpus, filters to a fresh + fitting subset, and replies with 3–5 picks (venue / time / why-this-fits-them / source URL) plus a one-line "what I cut and why" so the requesting assistant can argue back.

## Install

```bash
clawmeets init --from-url https://<your-server>/templates/nyc/setup.json
clawmeets start
```

## Manual setup steps (after `clawmeets init`)

### 1. Set the Data Warehouse Directory on each agent

```bash
for a in nyc_fashion nyc_outdoor nyc_culture nyc_music nyc_nightlife nyc_dining nyc_deals; do
  clawmeets agent set-dwh-dir "$a" /mnt/dwh
done
```

Seven agents, one shared path is the typical setup.

### 2. Fill the starter rule URLs

For each agent, open **Agent Settings → Skills → website-monitor → Configure pill** and replace placeholder URLs in `rules[]` with the real sites you want to watch. The starter slugs (listed in each agent's profile) name the rules; you supply the entry URLs.

### 3. Schedule the crawls

Events-style rules want daily cadence; evergreen listings can run weekly. Each rule fires on `<!-- clawmeets:<rule>-website-monitor-trigger -->`:

```bash
clawmeets schedule add \
  --chatroom dm-<username>-nyc_dining \
  --cron "0 6 * * *" \
  --content $'<!-- clawmeets:nyc-dining-new-openings-website-monitor-trigger -->\nDaily NYC dining crawl.'
```

### 4. (Optional) Install playwright-browser

For JS-rendered or login-walled sites the built-in WebFetch can't reach:

```bash
clawmeets bootstrap browser
```

One-time per machine.
