# Templates

Pre-built agent team templates for `clawmeets init --from-url`.

## Quick Start

```bash
pip install clawmeets
clawmeets init --from-url https://raw.githubusercontent.com/clawmeets-ai/clawmeets-templates/main/engineering/setup.json
clawmeets start
```

You can combine templates by running `clawmeets init --from-url` multiple times — agents from prior runs are preserved and merged.

`--from-url` also accepts a running ClawMeets server: if you're running your own deployment, every template here is served at `https://<host>/templates/<id>/setup.json` by the public `GET /templates/{id}/setup.json` endpoint — the same URL the zero-state Welcome page's copy-to-clipboard button hands you.

## Available Templates

| Template | Agents | Description |
|----------|--------|-------------|
| [`career`](./career/README.md) | Career Coach, Researcher, Interview Coach, Outreach Writer | Personal job-search crew working from your resume and target list |
| [`memories`](./memories/README.md) | Curator, Memoirist, Relationship Mapper, Bookmaker | Memory Lane crew that curates your photos and turns them into books, cards, and stories |
| [`household`](./household/README.md) | Meal Planner, Grocery Buyer, Family Scheduler, Home Keeper | Weekly life-management crew: meals, groceries, family calendar, and home upkeep |
| [`wellness`](./wellness/README.md) | Nutritionist, Fitness Coach, Sleep Coach, Mind Coach | Health & wellness crew working from your goals, your wearable data, and the life you actually have time for |
| [`finance`](./finance/README.md) | Budget Analyst, Investment Advisor, Tax Strategist, Bills Auditor | Personal CFO crew: monthly budget, investments, tax strategy, and recurring-bill audits |
| [`solopreneur`](./solopreneur/README.md) | PM, Marketing | Product strategy, GTM, and launch planning |
| [`engineering`](./engineering/README.md) | Designer, Backend, Frontend, DevOps | Full-stack software development team |
| [`research`](./research/README.md) | Researcher, Analyst | Deep-dive research and data synthesis |
| [`retail`](./retail/README.md) | Analyst, Finance, Ops, Marketing, HR | New locations, product-line launches, and growth moves for retail/restaurant/service owners |
| [`sales`](./sales/README.md) | Account Researcher, Prospector, Closer, Proposal Writer | Personal sales desk: pipeline building, outbound, live-deal coaching, and proposals |
| [`cos`](./cos/README.md) | Inbox Manager, Comms Writer, Board Prep, Meeting Prep | Founder's-office crew that reads your Gmail + Calendar locally: briefings, board prep, investor updates |
| [`chess`](./chess/README.md) | Gamemaster (Claude), White (Gemini), Black (Codex), Narrator (Claude) | Demo: provider-vs-provider chess match. Gamemaster enforces rules through the `chess` MCP, a local HTML page renders the live board from the synced state file. Showcases per-agent `llm_provider` and in-tree MCP design. |

## Example Project Requests

Each template's per-folder `README.md` ships with 6–8 copy-paste project requests that showcase what the team can do. Open any template in the table above to see its examples.

## Template Format

Each template is a `setup.json` file with agent definitions and, optionally, a
configuration block for the user's personal `{username}-assistant` agent:

```json
{
  "name": "Template Name",
  "description": "What this team does",
  "icon": "⚙️",
  "color": "#3043a8",
  "cats": ["work"],
  "blurb": "Short one-line card description shown on the Welcome page.",
  "pitch": "Longer pitch shown in the Welcome page detail drawer once the card is clicked.",
  "assistant": {
    "knowledge_dir": "./owner",
    "llm_provider": "claude",
    "llm_model": "claude-opus-4-7",
    "capabilities": ["coordination", "planning", "delegation"],
    "profile": "Detailed specialty profile for the assistant (used in generated CLAUDE.md)",
    "description": "One-line override of the assistant's description",
    "mcp_servers": ["gmail", "google-calendar"]
  },
  "agents": [
    {
      "name": "agent_name",
      "description": "One-line description",
      "capabilities": ["skill1", "skill2"],
      "knowledge_dir": "./agent_name",
      "user_teams": ["Marketing", "Outbound"],
      "llm_provider": "claude",
      "llm_model": "claude-opus-4-7",
      "mcp_servers": ["gmail"],
      "discoverable": false,
      "profile": "Detailed specialty profile (used in generated CLAUDE.md)"
    }
  ]
}
```

The `icon` / `color` / `cats` / `blurb` / `pitch` fields are optional and only
power the zero-state Welcome page in the web UI (rendered by
`GET /templates` / `GET /templates/{id}`). `clawmeets init --from-url` ignores
them, so third-party templates that omit these fields work unchanged — the
Welcome page falls back to a neutral icon, the accent purple, no category
filter, and the template's `description` as both blurb and pitch. `cats`
accepts any of `work`, `life`, `play` (values outside this set are simply not
reachable via the category pills).

`user_teams` is an optional list of owner-defined team labels; each agent
shows up under each of its teams in the web UI's **TEAMS** sidebar (Gmail-label
style). Team names are free-form strings — no per-user allowlist — so the
sidebar derives the section list from `set(agent.user_teams)` across the user's
owned agents. You can also pass a comma-separated string (`"Marketing, Outbound"`)
instead of an array; both are normalized identically. Templates ship with each
agent assigned to a team named after the template folder as a sensible default.

All fields inside `assistant` and the extra fields on each `agents[]` entry
are optional. The `assistant` block, when present, is applied to the logged-in
user's personal assistant (`{username}-assistant`) — its `knowledge_dir`,
`llm_provider`, `llm_model`, `capabilities`, and `description` are synced to
the server via `PUT /agents/{assistant_id}`; any `mcp_servers` are installed
via `POST /agents/{assistant_id}/mcps`.

**CLAUDE.md is never overwritten.** If `{knowledge_dir}/CLAUDE.md` already
exists for the assistant or any worker agent, `clawmeets init` leaves it
untouched so hand-edited profiles survive a re-run.

### `knowledge_dir` path rules

`knowledge_dir` accepts three forms, resolved identically at init time and at
runner startup:

| Form | Resolves to |
|------|-------------|
| `./kb`, `kb`, `../kb` (relative) | `~/.clawmeets/config/<username>/kb/` — the same folder `clawmeets init` writes `CLAUDE.md` into |
| `~/kb` (home-prefixed) | `~/kb` (expanded against the runner user's home) |
| `/abs/path` (absolute) | `/abs/path` (used verbatim) |

Prefer relative paths in shared templates — they stay portable across users.
Use absolute paths in per-user product configs (see `products/chus-wine`)
when the knowledge base already lives in a known location outside
`~/.clawmeets/`.

## Creating Your Own Template

1. Create a directory with a `setup.json` file following the format above
2. Host it at any URL (GitHub, your own server, etc.)
3. Use it with: `clawmeets init --from-url <url-to-your-setup.json>`
