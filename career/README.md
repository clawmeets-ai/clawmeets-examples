# Career Team

A four-agent job-search crew that works in parallel off **your** resume, **your** target list, and **your** interview history. Resume drafts, company briefs, mock-interview transcripts, and outreach drafts never leave your machine.

## The Team

| Agent | What they do | Deliverables |
|-------|--------------|--------------|
| `@career_coach` | Resume tailoring, story arc, application strategy, comp negotiation | Role-tailored resumes, cover letters, narrative frames, counter-offer scripts |
| `@researcher` | Deep dives on companies, teams, hiring managers, comp data | Company briefs, team + launch maps, salary benchmarks |
| `@interview_coach` | Mock interviews, STAR answers from your history, role-specific rounds | Drilled question banks, mock transcripts with feedback, rebuttal prep |
| `@outreach_writer` | Cover letters, recruiter / hiring-manager DMs, networking intros, follow-ups | Personalized sequences, thank-you templates, warm-network outreach |

## Install

```bash
clawmeets init --from-url https://raw.githubusercontent.com/clawmeets-ai/clawmeets-templates/main/career/setup.json
clawmeets start
```

Drop your resume, past job descriptions, and any prior writing samples into each agent's `knowledge_dir` on the first run — the drafts start sounding like you by the second request.

## Compelling Project Requests

Drop any of these into your coordinator and watch all four agents fan out in parallel:

1. **Final-round prep** — *"I have an interview coming up for a **[role]** at **[company]** (details and JD in my knowledge folder). Tailor my resume to the JD, deep-research the team and recent launches using anything I've saved, drill me on behavioral and product-sense questions, and draft a thank-you template I can personalize after each round."*

2. **Career pivot** — *"I'm pivoting from **[current field]** to **[target field]**. Reframe my resume narrative around transferable skills, suggest target roles that value my background, and prep me for the 'why are you switching?' question across 3 difficulty levels."*

3. **Targeted job hunt** — *"I'm a **[current role]** trying to break into **[target space]**. Suggest a target company list ranked by fit with my background (resume is in my knowledge folder), draft tailored cover letters for the top matches, and prep a 30/60/90-day plan I can pitch in interviews."*

4. **Offer negotiation** — *"I have competing offers (details in my knowledge folder). Benchmark both against market, model the equity expected value, draft a counter to whichever I'd want to push on, and prep my talking points for the negotiation call."*

5. **Layoff response** — *"I was just laid off. Update my resume, write a 'looking for work' LinkedIn post in my voice, build a target company list that hires my profile, and draft personalized outreach templates I can adapt for my warm network."*

6. **Mock interview marathon** — *"Mock-interview me for a **[role]** role at **[company]** — system design, behavioral, and coding rounds (or whichever apply). `@researcher`: pull what's public about **[company]**'s interview loop and the team's recent work so the questions are company-specific, not generic. `@interview_coach`: run the rounds using my resume (in knowledge) to predict deep-dive questions about my past projects. `@career_coach`: after each round, debrief the answers against my narrative arc and flag where I'm underselling."*

## Why This Team Works

Most AI resume tools train on generic resume corpora and return generic output. This team works from **your** history — the projects you actually shipped, the companies you actually worked at, the interview questions you actually bombed — because your resume, company research, and interview transcripts all live locally. Nothing about your job search ends up in someone else's training data.
