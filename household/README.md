# Household Team

A four-agent life-management crew that runs your family's week on one rhythm — meals, groceries, calendar, and home upkeep — so nothing quietly falls through the cracks.

## The Team

| Agent | What they do | Deliverables |
|-------|--------------|--------------|
| `@meal_planner` | Weekly menu shaped to tastes, allergies, and weeknight realities; leftover strategy | 5-dinner weekly menus, prep-time schedule, leftover plan |
| `@grocery_buyer` | Consolidated shopping list, pantry tracking, store-aware routing, price-aware swaps | Store-split shopping lists, substitution picks, pantry inventory |
| `@family_scheduler` | Synthesis layer for the family week: kids' activities, school events, RSVPs, carpool logistics. Coordinates with `@mailbox` + `@calendar` from the **personal_data** team for mail/calendar reads | Unified family calendar synthesis, conflict flags, carpool plan, RSVP-sweep memos |
| `@home_keeper` | Chore rotation, repairs, vendor contacts, seasonal maintenance | Chore schedule, vendor list, maintenance calendar, repair tracker |

## Install

```bash
clawmeets init --from-url https://raw.githubusercontent.com/clawmeets-ai/clawmeets-templates/main/household/setup.json
clawmeets start
```

Drop your family's dietary notes, rotating recipes, and vendor contacts into each agent's `knowledge_dir` — the weekly plan gets sharper on the second pass.

## Pairs with

`@family_scheduler` reads mail and calendar through the **personal_data** team's `@mailbox` and `@calendar` agents — it has no direct Gmail / Google Calendar OAuth of its own. Install both teams to get RSVP sweeps, school-event watches, and Sunday-night week syncs:

```bash
clawmeets init --from-url https://raw.githubusercontent.com/clawmeets-ai/clawmeets-templates/main/personal_data/setup.json
```

Then create projects with `@family_scheduler` + `@mailbox` + `@calendar` together. The carpool / pickup-chain requests are pure synthesis off `@family_scheduler`'s knowledge_dir and don't need the personal_data team.

## Compelling Project Requests

Drop any of these into your coordinator and watch all four agents fan out in parallel — one weekly rhythm, one source of truth:

1. **Weekly fridge sheet** *(needs personal_data team installed)* — *"It's Sunday night. Build our weekly family plan as a printable fridge sheet. All deliverables go to `.bus-files/week-of-[Date]/`. `@meal_planner`: 5 weeknight dinners + 1 weekend dish that fit this week's commitments; leftover plan + one fridge-cleanup meal at the end; write `MENU.md`. `@grocery_buyer`: consolidated shopping list grouped by store section, split across the stores I've listed; write `GROCERIES.md`. `@calendar`: list every event between this Sunday 18:00 and next Sunday 18:00 across the family calendars. `@family_scheduler`: take that list + kids' recurring routines from your knowledge_dir, surface conflicts, decisions needed before Monday; write `CALENDAR.md`. `@home_keeper`: chore rotation by room and person, plus the one-off task of the week; write `CHORES.md`. Coordinator: stitch `FRIDGE-SHEET.md` — one-page printable with menu-by-night, chore assignments, this week's family commitments, and the ONE thing not to screw up."*

2. **Holiday hosting** — *"We're hosting **[holiday]** for **[N]** guests, with **[M]** staying overnight. `@meal_planner`: menu + oven choreography (prep timeline back from serving time; what to make the day before). `@grocery_buyer`: shopping list split across the stores I use; flag items that have to be ordered ahead. `@family_scheduler`: seating + sleeping arrangements; arrival/departure logistics on the family calendar. `@home_keeper`: pre-event house prep + cleanup chore assignments so I'm not doing dishes until midnight."*

3. **Vacation prep** — *"We leave for a **[N]-day** trip to **[destination]** in **[N]** days. `@home_keeper`: pre-trip checklist (mail hold, plant watering, pet boarding, fridge cleanout, security walkthrough); house-sitter brief if applicable. `@grocery_buyer`: list scheduled grocery deliveries to pause with cancel-by deadlines; light restock list for the day we're back. `@family_scheduler`: list the trash / recycling pickups to freeze (so I can call the city); per-family-member packing list. `@meal_planner`: use-up-the-pantry meals for the 3 days before we leave."*

4. **Big move** — *"We're moving in **[N]** weeks. `@home_keeper`: packing schedule by room with sequencing; checklist for booking movers + cleaners for both ends. `@family_scheduler`: every utility / license / bank / subscription / school-record that needs an address change with deadlines; flag move dates that collide with school or work. `@meal_planner`: low-prep meals for the moving week (paper plates fine). `@grocery_buyer`: pantry-use plan from the inventory in my knowledge folder so we don't waste food."*

5. **Back-to-school kickoff** *(needs personal_data team for the calendar piece)* — *"Back-to-school for our kids (ages in my knowledge folder). `@family_scheduler`: read the school calendar I uploaded into your knowledge_dir; suggest fall activity options that fit our schedule. `@mailbox`: scan the inbox for parent-teacher conferences and field trips announced this month. `@calendar`: for each event `@mailbox` surfaces, `create_event` on the family calendar after `@family_scheduler` confirms it doesn't conflict. `@meal_planner`: school-lunch rotation for the month with kid-by-kid notes. `@grocery_buyer`: monthly shopping list to support the lunch rotation. `@home_keeper`: weeknight homework + bedtime routine and assigned chores for the new school week."*

6. **Quarterly home reset** *(needs personal_data team for the calendar piece)* — *"Run a household reset for this quarter. `@home_keeper`: HVAC service + filter replacement timing, deep-clean rotation by room, declutter pass on each kid's bedroom (donate / sell / keep piles), and vendor-list refresh (knowledge folder) — flag anyone we haven't used in a year. `@calendar`: list every event in the next 90 days. `@family_scheduler`: pick the right weekend for each `@home_keeper` item so it doesn't collide with school breaks, travel, or busy sports weeks. `@grocery_buyer`: consolidate replacement filters, cleaning supplies, and donation-run prep into one bulk order so I'm not running errands mid-reset. `@meal_planner`: plan no-prep dinners for the deep-clean weekend so the kitchen can stay torn apart."*

## Why This Team Works

Household apps each own one slice — meals here, calendar there, chores in a third place — and none of them talk. This team treats your house as one system: the meal plan drives the shopping list, the calendar drives the meal plan, and the chore rotation knows who's actually home. Your family's schedules, vendor contacts, and dietary notes stay on your machine.
