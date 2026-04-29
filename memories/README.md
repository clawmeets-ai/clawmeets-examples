# Memory Lane Team

A four-agent photo-and-story crew that turns your library into books, cards, slideshows, and daily moments of reflection. Your photos, notes, and relationships never leave your machine.

## The Team

| Agent | What they do | Deliverables |
|-------|--------------|--------------|
| `@curator` | Organizes your library into events and trips, deduplicates near-identical shots, picks keepers | Event-grouped albums, caption drafts, print-worthy shortlists |
| `@memoirist` | Writes the narrative behind your photos: essays, trip recaps, milestone reflections, 'on this day' stories | Year-in-review essays, travel writeups, daily memory paragraphs |
| `@relationship_mapper` | Tracks who appears over time, builds people timelines, surfaces who you've seen most (and least) | People timelines, reconnection prompts, relationship heatmaps |
| `@bookmaker` | Turns selected photos and stories into shareable artifacts | Hardcover book layouts, holiday cards, slideshow scripts, social posts |

## Install

```bash
clawmeets init --from-url https://raw.githubusercontent.com/clawmeets-ai/clawmeets-templates/main/memories/setup.json
clawmeets start
```

Point each agent's `knowledge_dir` at your photo library export (or a subfolder like "2025") on the first run.

## Compelling Project Requests

Drop any of these into your coordinator and watch all four agents fan out in parallel — your photos and stories never leave your machine:

1. **Year in review** — *"Build a year-in-review from the **[year]** folder in my library. Surface the photos that actually mattered (shoot for ~50), write a narrative recap by season, map who I spent the most time with, and lay out a hardcover photo book ready to send to a print service."*

2. **Trip recap** — *"I just got back from a trip to **[destination]** — photos are in my knowledge folder. Group them by city and day, write a travel essay with captions, pick the print-worthy shots, and draft a short social-post carousel I can share tomorrow."*

3. **Daily memory lane** — *"Every morning, send me one 'on this day' memory from 1, 5, and 10 years ago. `@curator`: pick the best photo from each year (pull from my library, dedupe against near-identical shots). `@relationship_mapper`: tell me who was in the photo and when I last spent time with them — nudge me if it's been a while. `@memoirist`: write a one-paragraph reflection for each year that ties the photo to what was happening in my life at the time (notes in knowledge where I have them)."*

4. **Milestone birthday book** — *"**[Person]** is turning **[age]**. Pull every photo with them in it from my library, write a narrative arc of their life through the moments captured, and design a hardcover book layout with large-print captions I can hand them on the day."*

5. **Kid milestone book** — *"My **[kid]** is turning **[age]**. Build a one-photo-per-month album from the day they were born to today, with captions of what they were doing or saying at each age, and prep a bound book layout I can hand out at their birthday party."*

6. **Friendship time capsule** — *"I want to make something for my **[group: e.g. college friends / team / bandmates]**. Find every photo of us together in my library, surface the funniest / most meaningful ones, write captions that capture the inside jokes, and lay out a shareable album with a 'remember when' intro I can text the group."*

## Why This Team Works

Cloud photo apps are great at "here are 6 photos of your dog from 2019." They're not great at writing a 30-page narrative about your kid's first decade, because that would require handing your entire photo library to a model — and you shouldn't. This team reads your library locally, writes the story in your voice, and nothing about your family ends up training anyone else's model.
