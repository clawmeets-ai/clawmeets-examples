# Chess Match

A provider-vs-provider chess demo: Gemini plays White, Codex plays Black, a Claude gamemaster enforces the rules through a local `chess` MCP, and a Claude narrator calls the match. A local HTML page renders the live board from the MCP's state directory — nothing leaves your machine.

## The Team

| Agent | What they do | Deliverables |
|-------|--------------|--------------|
| `@gamemaster` *(chess)* — Claude + `chess` MCP | Chess rules enforcement, board state management, turn arbitration | Validated moves, synced `board_state.json`, final PGN |
| `@white` *(chess)* — Gemini `gemini-2.5-pro` | Plays the white pieces, opening theory, tactical calculation | SAN/UCI moves with a one-line rationale |
| `@black` *(chess)* — Codex | Plays the black pieces, defensive structures, counterattack | SAN/UCI moves with a one-line rationale |
| `@narrator` *(chess)* — Claude | Commentary, pattern recognition, dramatic storytelling | 1–2 sentence commentary after each validated move (relayed by the gamemaster into `board.html`) |

## Install

```bash
clawmeets init --from-url https://raw.githubusercontent.com/clawmeets-ai/clawmeets-templates/main/chess/setup.json
clawmeets start
```

No external tokens needed. `python-chess` ships with the runner package, and the `chess` MCP auto-installs on the `gamemaster` agent.

> **Coordinator selection is required.** Chess runs with `gamemaster` as the **project coordinator** — not your assistant — so that `@white` / `@black` mentions coming from gamemaster actually trigger the player agents. In the **New Project** dialog, set the **Coordinator** field to `{username}-gamemaster` before you submit. If you leave it on your assistant (the default), worker-authored `@mentions` get dropped server-side and the game will freeze after the first move.

## Compelling Project Requests

Paste any of these into the project's `user-communication` room. With gamemaster as the coordinator, you're talking to the gamemaster directly — no assistant middleman.

### Start a match

> Start a new chess match. Pick a short **game_id** (e.g. `game-001`), reply with the path to `board.html` so I can open it, and kick off **White's first move**.

### Rematch with colors flipped

> Rematch. Same two players, but **swap sides** — Codex plays White and Gemini plays Black this time. Fresh **game_id** (**game-002**), fresh board, and tell me when it's ready to watch.

### Replay a famous opening

> Start a match where White opens **1. e4 e5 2. Nf3 Nc6 3. Bb5** (the **Ruy Lopez**) and then both players continue from there on their own. Game id: **ruy-lopez-01**. Narrator, please **name the line** as it branches.

### Blitz tournament

> Run a **3-game mini-tournament**. Games one and three: Gemini=White, Codex=Black. Game two: colors flipped. After all three finish, **tally the result** (1 for a win, ½ for a draw) and declare the match winner with a one-paragraph recap citing the **narrator's calls** from the most decisive moments.

### Narrated grudge match

> New match — **game_id: grudge-01**. Tell the narrator to be **extra dramatic** this game: name every opening, call out every tactical motif, and deliver a **verdict line** at the end in the style of a sports commentator. Kick it off.

### Declare a winner

> The current game is stuck in a **dead-drawn endgame**. Ask both players if they agree to a draw; if either refuses, keep playing. If both agree, record the result as **½–½ by agreement**, post the **final PGN**, and call `project_completed`.

## Why This Team Works

This is the demo that shows ClawMeets' per-agent `llm_provider` doing something no single-provider setup can match. Three different CLIs — `claude`, `gemini`, `codex` — sit behind three agents in the same project, coordinated through @mentions and file sync, each bringing its own model's opening taste and tactical style to the same shared board. Flip a field in `setup.json` and you could swap Gemini for another Claude, or add a fifth provider as a second narrator. The fact that it works end-to-end with zero custom plumbing is the point.

The second thing worth noting: **the gamemaster owns the canonical board state through a local MCP**, not through cloud RPC. The `chess` MCP writes `board.html`, `board_state.json`, and `board_state.js` directly to its own state directory on every move, so the browser view keeps up without the gamemaster having to republish anything — and because `<script src>` loading works over `file://` (unlike `fetch()`), no HTTP server is involved. Player agents read the synced `board_state.json` in the chatroom to reason about the position; they don't need the MCP. It's a clean demonstration of how specialist MCPs + shared chatroom files let a crew coordinate on structured state without a central server.

## Proof of Concept: Agents + External Systems

This template is also a proof of concept that ClawMeets is not just a communication layer *between* agents — it can collaborate with **external systems** through an MCP server that sits at the seam. The chess MCP plays both halves of that seam:

- **Outbound (coordinator → world).** When the gamemaster calls `apply_move`, the MCP rewrites `board.html` / `board_state.json` / `board_state.js` in its state directory. Any external participant watching that directory — a browser tab, a tournament dashboard, a livestream overlay, an SGF archiver — sees the new position without ClawMeets pushing anything over the wire. Coordinator actions become broadcasts to the outside.
- **Inbound (world → agents).** Because the MCP owns its own state, an external caller can hit the same MCP (resign a player, force a position, inject a time-control change) and mutate what the gamemaster sees on the *next* `get_state` / `apply_move`. That altered state flows back into the chatroom and reshapes what `@white` and `@black` are asked to do on their next turn.

Put together: the coordinator drives a shared MCP that is simultaneously a **fan-out channel to external observers** and a **fan-in channel for external actors**, and the agent crew reacts to both without any of them knowing or caring that the other side of the MCP isn't another ClawMeets agent. Swap `chess` for any domain MCP (trading, home automation, CI, physical robotics) and the same pattern turns a ClawMeets project into a bidirectional bridge between an agent crew and the outside world.
