# Health & Wellness Team

A four-agent wellness crew that coordinates nutrition, training, sleep, and mind around **your** goals, **your** wearable data, and the life you actually have time for. Whoop / Oura / Apple Watch exports, food logs, and mood notes never leave your machine.

## The Team

| Agent | What they do | Deliverables |
|-------|--------------|--------------|
| `@nutritionist` | Calorie and macro targets, meal patterns, adjustments as the data comes in | Weekly meal plans, macro targets, grocery list, adherence check-ins |
| `@fitness_coach` | Training program, progression, mobility, recovery, calendar-aware volume | Periodized program, session-level load plan, mobility blocks |
| `@sleep_coach` | Sleep schedule, evening routine, recovery tracking against wearable data | Bedtime protocol, sleep-window plan, recovery-adjusted intensity calls |
| `@mind_coach` | Stress regulation, journaling cadence, mindfulness, mood tracking | Daily reflection prompts, stress-down drills, escalation signals |

## Install

```bash
clawmeets init --from-url https://raw.githubusercontent.com/clawmeets-ai/clawmeets-templates/main/wellness/setup.json
clawmeets start
```

Export your last 90 days of wearable data and drop it into each agent's `knowledge_dir` on the first run — the weekly plan starts calibrating off real numbers on the second request.

## Compelling Project Requests

Drop any of these into your coordinator and watch all four agents fan out in parallel — your wearable data, food logs, and goals never leave your machine:

1. **Weekly wellness plan** — *"Build my week: 3 strength workouts + 2 zone-2 sessions matched to the open slots on my calendar (in my knowledge folder), a meal plan hitting my macro target, a sleep schedule with a consistent wake time, and a 5-minute morning journaling prompt. One page I can put on my desk."*

2. **Body-composition cut** — *"I want to lose **[target]** lbs over **[N] weeks** for **[event / reason]**. Set my macros, build a progressive lifting program with deloads, design a sleep + stress routine that supports the deficit, and define a weekly check-in I can run against the wearable / weigh-in data I'll drop into my knowledge folder."*

3. **First half-marathon** — *"I'm running my first half-marathon in **[N] weeks**; my current 5K pace is in my training log. Build a weekly training plan, layer in strength + mobility for injury prevention, dial in race-fueling and hydration practice, and prep me mentally for the long runs and the race itself."*

4. **Burnout recovery** — *"I'm coming off a brutal stretch — sleep is wrecked, my resting heart rate is elevated, energy is gone, mood is flat. Run a 4-week recovery protocol: rebuild sleep first, ease back into training at low intensity, fix nutrition (I've been skipping meals), and add daily stress-down practices. Weekly check-ins against the wearable export I put in my knowledge folder."*

5. **Travel-week prep** — *"I leave for a **[N]-day** trip across **[N]** time zones, with daily meetings once I land. Build a pre-trip jet-lag protocol (light + caffeine + sleep timing), pack a few hotel-room workouts I can do without equipment, plan how to eat well on the road without obsessing, and design a recovery week for when I get back."*

6. **Annual reset** — *"Pull last year's metrics from the wearable export and training log in my knowledge folder (weight, sleep score, resting HR, training volume, mood trend), set 3 specific health goals for this year, build the first-quarter plan to start them, and define what 'good enough' looks like across all four agents so I don't chase perfection and burn out by spring."*

## Why This Team Works

Generic fitness and nutrition apps can't read your Whoop recovery, your Apple Health logs, your food journal, and your calendar at the same time — they each live in a silo. This team reads all of it locally, adjusts the training plan when sleep tanks, backs off the deficit when stress spikes, and keeps the weekly plan in contact with reality. Nothing about your body ever leaves the machine.
