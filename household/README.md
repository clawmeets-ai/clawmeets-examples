# Household Team

A four-agent life-management crew that runs your family's week on one rhythm — meals, groceries, calendar, and home upkeep — so nothing quietly falls through the cracks.

## The Team

| Agent | What they do | Deliverables |
|-------|--------------|--------------|
| `@meal_planner` | Weekly menu shaped to tastes, allergies, and weeknight realities; leftover strategy | 5-dinner weekly menus, prep-time schedule, leftover plan |
| `@grocery_buyer` | Consolidated shopping list, pantry tracking, store-aware routing, price-aware swaps | Store-split shopping lists, substitution picks, pantry inventory |
| `@family_scheduler` | Everyone's calendar in sync: kids' activities, school events, RSVPs, carpool logistics | Unified family calendar, conflict flags, carpool plan |
| `@home_keeper` | Chore rotation, repairs, vendor contacts, seasonal maintenance | Chore schedule, vendor list, maintenance calendar, repair tracker |

## Install

```bash
clawmeets init --from-url https://raw.githubusercontent.com/clawmeets-ai/clawmeets-templates/main/household/setup.json
clawmeets start
```

Drop your family's dietary notes, rotating recipes, and vendor contacts into each agent's `knowledge_dir` — the weekly plan gets sharper on the second pass.

## Compelling Project Requests

Drop any of these into your coordinator and watch all four agents fan out in parallel — one weekly rhythm, one source of truth:

1. **Sunday weekly plan** — *"It's Sunday night. Plan our family week: weeknight dinners that account for the commitments in my knowledge folder (kids' activities, my busy workdays), a consolidated grocery list to support the menu (split across the stores I've listed), the family calendar with everyone's commitments and conflicts surfaced, and this week's chore rotation. One document I can pin to the fridge."*

2. **Holiday hosting** — *"We're hosting **[holiday / event]** for **[N]** guests, with **[M]** staying overnight. Plan the menu and oven choreography, build the shopping list split across the stores I use, lay out seating + sleeping arrangements, and assign cleanup chores so I'm not doing dishes until midnight."*

3. **Vacation prep** — *"We leave for a **[N]-day** trip to **[destination]** in **[N]** days. Build a pre-trip checklist (mail hold, plant watering, pet boarding, fridge cleanout, packing list per family member), pause our scheduled grocery delivery, line up a house-sitter, and freeze the trash and recycling pickups for the week we're gone."*

4. **Big move** — *"We're moving in **[N]** weeks. Build a packing schedule by room, create a checklist for booking movers + cleaners for both ends, list every utility / license / bank / subscription that needs an address change, and plan meals using what's already in the pantry (list in my knowledge folder) so we don't waste food."*

5. **Back-to-school kickoff** — *"Back-to-school for our kids (ages in my knowledge folder). Sync the school calendar I uploaded, suggest fall activity options that fit our schedule, plan school-lunch packing for the month, set a homework + bedtime weeknight routine, and add every parent-teacher conference and field trip to the family calendar before they sneak up on us."*

6. **Quarterly home reset** — *"Run a household reset for this quarter. `@home_keeper`: HVAC service + filter replacement timing, deep-clean rotation by room, declutter pass on each kid's bedroom (donate / sell / keep piles), and vendor-list refresh (knowledge folder) — flag anyone we haven't used in a year. `@family_scheduler`: find the right weekend for each item so it doesn't collide with school breaks, travel, or busy sports weeks on the family calendar. `@grocery_buyer`: consolidate replacement filters, cleaning supplies, and donation-run prep into one bulk order so I'm not running errands mid-reset. `@meal_planner`: plan no-prep dinners for the deep-clean weekend so the kitchen can stay torn apart."*

## Why This Team Works

Household apps each own one slice — meals here, calendar there, chores in a third place — and none of them talk. This team treats your house as one system: the meal plan drives the shopping list, the calendar drives the meal plan, and the chore rotation knows who's actually home. Your family's schedules, vendor contacts, and dietary notes stay on your machine.
