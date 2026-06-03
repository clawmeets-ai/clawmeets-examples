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

1. **Final-round prep packet** — *"Build my final-round prep packet for **[role]** at **[company]** from my attached resume and the JD. All deliverables go to `.bus-files/`. `@career_coach`: tailor the resume bullets to the JD; write `.bus-files/resume-tailored.md`. `@researcher`: WebSearch recent launches, leadership posts, and public engineering writing — public sources only (skip levels.fyi / Blind / Glassdoor and flag anything you can't ground). Write `.bus-files/company-brief.md` with a one-paragraph what-they-do, 5 recent moves with source URLs, 3 talking points, and 3 risks I should ask about. `@interview_coach`: read the resume + brief and write `.bus-files/star-bank.md` (6 STAR answers covering scope / ownership / conflict / failure) plus `.bus-files/likely-questions.md` (10 questions tuned to this company / role). `@outreach_writer`: write `.bus-files/thank-you-template.md` with `[PLACEHOLDER]` slots. Coordinator: stitch `.bus-files/PREP.md` — a one-page index plus the ONE thing I should rehearse hardest before the loop."*

2. **Mock interview marathon** — *"Mock-interview me for a **[role]** role at **[company]** — system design, behavioral, and coding rounds (or whichever apply). `@researcher`: pull what's public about **[company]**'s interview loop and the team's recent work so the questions are company-specific, not generic. `@interview_coach`: run the rounds using my resume (in knowledge) to predict deep-dive questions about my past projects. `@career_coach`: after each round, debrief the answers against my narrative arc and flag where I'm underselling."*

3. **Final-round prep** — *"I have an interview coming up for a **[role]** at **[company]**. `@career_coach`: tailor my attached resume to the role. `@researcher`: WebSearch the team and recent launches — public sources only (engineering blog, press, public talks); flag what's not groundable. `@interview_coach`: drill me on behavioral and product-sense questions. `@outreach_writer`: draft a thank-you template I can personalize after each round."*

4. **Targeted job hunt** — *"I'm a **[current role]** trying to break into **[target space]**. `@career_coach`: from my attached resume, build a tiered target list (reach / match / safety) using public-web research only — actively hiring, profile fit. `@outreach_writer`: draft tailored cover letters for the top 3. `@career_coach`: prep a 30/60/90-day plan I can pitch in interviews."*

5. **Layoff response** — *"I was just laid off. `@career_coach`: update my attached resume for the open market. `@outreach_writer`: write a 'looking for work' LinkedIn post in my voice, plus personalized outreach templates I can adapt for my warm network. `@career_coach`: build a target company list that hires my profile."*

## Why This Team Works

Most AI resume tools train on generic resume corpora and return generic output. This team works from **your** history — the projects you actually shipped, the companies you actually worked at, the interview questions you actually bombed — because your resume, company research, and interview transcripts all live locally. Nothing about your job search ends up in someone else's training data.
