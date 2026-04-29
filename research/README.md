# Research Team

A two-agent research desk that runs deep investigations off **your** reading list, **your** data, and **your** framing. Sources, drafts, and findings stay on your machine.

## The Team

| Agent | What they do | Deliverables |
|-------|--------------|--------------|
| `@researcher` *(research)* | Sourcing, literature review, competitive scans, market research, source evaluation | Source bibliography, findings memo, contrarian-takes list, primary-source excerpts |
| `@analyst` *(research)* | Quant analysis, pattern recognition, visualization, statistical reasoning, decision frameworks | Data summary, charts, decision memo, sensitivity ranges |

## Install

```bash
clawmeets init --from-url https://raw.githubusercontent.com/clawmeets-ai/clawmeets-templates/main/research/setup.json
clawmeets start
```

Drop relevant PDFs, spreadsheets, past research memos, and the raw datasets you want analyzed into each agent's `knowledge_dir` before the first run — the researcher cites what it can see, and the analyst can only crunch what's on disk.

## Compelling Project Requests

Paste any of these into your coordinator. Replace the bracketed details with your own.

### Size a market

> Size the **[market: e.g. AI-assisted contract review for mid-market law firms]**. Researcher: pull **top-down and bottom-up sizes** with the **2–3 sources** behind each, flag where the numbers disagree and why. Analyst: build the **TAM / SAM / SOM** with **transparent assumptions** I can flex, a **sensitivity table** showing how the answer moves when the 2 biggest assumptions change, and a **one-page memo** with the single number I should quote and the caveat that goes with it.

### Competitive landscape

> Map the landscape for **[category]**. Researcher: find **10–15 companies**, group them into **2–3 strategic clusters** by what they're actually betting on (not how they pitch themselves), and write a **one-paragraph "what they really believe"** for the top 5. Analyst: build a **feature matrix**, an **estimated-funding / revenue table**, and flag **2 whitespace bets** nobody's making yet with a brief argument for why each could work or is already a bad idea for a reason.

### Literature review

> I'm trying to form a view on **[topic]**. Use the papers, books, and articles I've dropped into my knowledge folder — cross-referenced with what you know about the space — to cluster the thinking into **the 3 camps** of the debate, with **1-paragraph summaries** of the most important source in each camp. Analyst: look at any numbers, charts, or tables across the sources I uploaded and tell me **where they agree, disagree, and quietly contradict each other**. Output a **"here's what I'd believe if I only read one thing, and here's what I'd believe if I read all of it"** memo.

### Data drill-down

> Here's a dataset of **[data: e.g. 18 months of support tickets]** in knowledge. Analyst: surface the **3 non-obvious patterns** — not the aggregate stats, the weird distributions, the segments that behave differently, the seasonality you only see after cleaning. Researcher: pull **external context** (industry reports, competitor notes) that explains *why* those patterns might exist. Output: a **3-chart brief** that would change how I run this quarter if it landed on my desk cold.

### Regulation scan

> Regulation scan for **[policy area]** across **[jurisdictions]**. Using the primary-source docs I've uploaded (plus your general knowledge of the space), summarize **what changed, what's pending, what's hinted** in each jurisdiction with **clear citations back to the uploaded files**. Analyst: build a **comparison table** and flag **the 3 changes most likely to affect [my company / industry]**, each with a **"what would we have to do differently" action line**.

### Trend synthesis

> Trend synthesis on **[topic]**. Using the release notes, conference talks, and trend notes I've dropped into my knowledge folder — plus what you already know about the space — cluster the signals into a coherent narrative. Analyst: fit a **trajectory**, forecast where this is in **12 months under status-quo / bull / bear scenarios**, and tell me **the one bet I should place now** if this keeps accelerating — plus the **single signal that would make me reverse** the bet.

## Why This Team Works

Most "AI research" today is either a shallow web-search summary or a one-shot analysis of a dataset you paste in. This team separates the two roles on purpose: the researcher insists on citing sources from **your** knowledge folder, not whatever ranks well in search, and the analyst works on **your** data with assumptions you can actually see and flex. The result is a memo you can defend — not a synthesis that sounds confident until you check the footnotes. Because both agents run locally, confidential datasets, unreleased strategy docs, and client NDAs stay on your machine.

**First-run checklist:** seed each agent's `knowledge_dir` with the raw materials (PDFs, CSVs, prior research memos) before you file the first request. The quality gap between "researcher can read 3 papers you uploaded" and "researcher is guessing from training data" is enormous.
