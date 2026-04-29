# Chief of Staff Team

A four-agent founder's-office crew that reads your **actual** Gmail and Google Calendar locally (via MCP) and produces the work a $250K/year Chief of Staff would. Your inbox, calendar, and company context never leave your machine.

## The Team

| Agent | What they do | Deliverables |
|-------|--------------|--------------|
| `@inbox_manager` *(Gmail + Calendar)* | Triage inbox, draft replies in your voice, manage calendar, run morning briefing | Triaged inbox, reply drafts, cleaned calendar, daily briefing |
| `@comms_writer` | All-hands, investor updates, announcements, crisis comms | Weekly investor update, all-hands script, press statements |
| `@board_prep` | Board decks, KPI commentary, pre-reads, Q&A prep | Board deck, pre-read memo, hard-question Q&A, post-meeting notes |
| `@meeting_prep` | Pre-meeting briefs, 1:1 agendas, decision memos | 60-second briefs, 1:1 agendas, decision memos, follow-up tracker |

> The template also configures your personal assistant as the Chief of Staff itself — it owns the delegation and the morning briefing.

## Install

```bash
clawmeets init --from-url https://raw.githubusercontent.com/clawmeets-ai/clawmeets-templates/main/cos/setup.json
clawmeets start
```

You'll be prompted to authorize Gmail and Google Calendar access once. Tokens stay on your machine.

## Compelling Project Requests

Paste any of these into your coordinator. Replace the bracketed details with your own.

### Morning briefing (the killer demo)

> Give me my **morning briefing**: **inbox triage** (replies I owe today, what can wait, what's on fire), my **calendar for today with a 2-line pre-read** for every meeting, the **one thing I should focus on** grounded in what's actually open in my inbox and calendar, and **draft replies** for the emails I should answer before my first meeting.

### Prep for my board meeting

> I have a **board meeting coming up** (check my calendar for the date). Build the **deck narrative**, pull our **KPIs with commentary** on why each moved (KPI history is in my knowledge folder), draft the **3 asks** I need from the board, prep **Q&A for the hardest questions** I'll get, and tell me when to send the **pre-read to the board** so it lands 72 hours ahead.

### Reclaim my calendar

> Look at **all my meetings this week**. `@meeting_prep`: rank each one on **stakes × irreversibility**, recommending **keep / delegate / decline / convert-to-async** with a one-line reason. `@inbox_manager`: block **focus time** in the slots where I actually think well and move the resulting gaps around fixed commitments. `@comms_writer`: draft **decline messages** in my voice that preserve the relationship — different tone for peer, VC, and report.

### Weekly investor update

> Write this week's **investor update** in under **400 words**. `@board_prep`: pull the **KPI deltas and commentary** on why each moved, plus hires and fundraise progress. `@comms_writer`: turn that into the **3 wins / 3 lowlights / 2 asks** narrative and match the voice from prior updates in my knowledge folder. `@inbox_manager`: line up the send — distribution list, attachments, and the best time of day to land in inboxes.

### All-hands prep

> Build my **next all-hands**. `@comms_writer`: pick the **3 themes** worth the team's time this week (use past all-hands transcripts in my knowledge folder to avoid repeating), draft **talking points**, and build the **slides**. `@board_prep`: prep me for the **Q&A I usually dread** — pull the hardest questions from team-survey notes or Slack paste-ins in knowledge and draft crisp answers. `@inbox_manager`: confirm the room/call is on the calendar and draft the reminder going out the morning of.

### Crisis comms

> We just had a **[production outage / security incident / customer-facing issue]**. Draft the **customer email**, the **internal incident note**, the **investor heads-up**, and a **board update message** — sequence the rollout so nobody gets contradictory info, and pair it with an **internal FAQ** so my managers have the same script when their reports ask.

### 1:1 prep marathon

> For **all my 1:1s this week**, pull **what we agreed last time** from my calendar notes, surface **their recent blockers and asks** from my email threads with them, draft **3 questions I should ask** that push past the prepared update, and tell me **which report needs an escalation from me** before Friday.

### Plan my week

> It's **Sunday night**. Look at the week ahead — calendar, open inbox threads, and anything in my knowledge folder (fundraise commitments, quarterly OKRs, active deals). Tell me the **3 outcomes I should aim for this week**, where my calendar isn't matching those outcomes, and the **one uncomfortable conversation** I've been avoiding that should happen Monday.

## Why This Team Works

This is the only template on your list that can't exist without ClawMeets' architecture. A SaaS "AI assistant" can't read your inbox the way this can — your privacy bar doesn't allow it, and the quality bar of generic models reading generic data doesn't clear it. Because `@inbox_manager` runs **locally** with **your** Gmail, **your** calendar, and **your** past writing in its knowledge folder, the drafts sound like you, the briefings know what you care about, and nothing ever leaves the machine to train anyone else's model.

**First-run checklist:** authorize Gmail and Google Calendar when prompted. Drop prior sent emails, investor updates, or all-hands transcripts into each agent's `knowledge_dir` to sharpen voice-matching in the first week.
