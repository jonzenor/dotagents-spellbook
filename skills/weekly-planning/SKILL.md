---
name: weekly-planning
description: Full weekly planning routine for Jon's Obsidian vault. Loads calendar, rhythms, OmniFocus tasks, and last week's rollover, then guides meal planning (Tue + Sat dinners plus 1 batch lunch), sets weekly goals across FOTF/Lighthouse/Personal, builds a day-by-day radar, and writes the week's note. Use when user says "weekly planning", "plan my week", "plan the week", or "weekly review".
---

# Weekly Planning

## Quick Start

Run this skill at the start of each week (Sunday or Monday morning). It loads full context, asks a few focused questions, then builds out the weekly note.

## Workflow

### Step 0 — Monthly Vault Maintenance

Read `Reference/Logs/Vault Maintenance Log.md`. If the current calendar month has no completed maintenance entry, run the monthly pass before planning:
- Move root-level `Daily/YYYY-MM-DD.md` notes more than 60 days old to `Daily/YYYY/MM/YYYY-MM-DD.md`.
- Update every explicit wikilink to each moved note, preserving aliases and heading references.
- Do not overwrite a destination collision; report it.
- Repair stale collection indexes and unambiguous wikilinks.
- Add missing provenance only when the source is unambiguous.
- Report, but do not automatically resolve, inactive-project candidates, conflicting duplicates, unclear provenance, ambiguous Person/Mentoring placement, or incomplete Keeper actions.
- Prepend the result to the maintenance log. Add a short weekly-note maintenance line only if something materially changed or needs Jon's decision.

Run this only during the first weekly-planning session of the month, never as a daily archive job.

### Step 1 — Load Context (run in parallel)
- Read `Rhythms.md` at the vault root
- Read the `## Weekly` section of `Keeper Instructions.md` and execute applicable standing instructions
- Read last week's weekly note (`Weekly/YYYY-WXX.md`) for rolled items
- Read this week's weekly note if it already exists
- Read the current quarterly plan (`Quarterly/YYYY-QN-Plan.md`) — determine which quarter it is from today's date
- Read `Reference/Theater Movie Candidates.md` if it exists. Use it only for the monthly theater-selection and release-week rules below.
- Query OmniFocus for overdue and flagged tasks: `query_omnifocus` with `{"status": ["Overdue", "DueSoon"], "fields": ["name", "project", "dueDate", "flagged"]}`
- Pull calendar for the full 7 days ahead using the `mcp-ical` tool. Query each of these calendars in parallel:
  - **"Jon's Calendar"** — personal events, appointments, paydays
  - **"Family"** (query twice — there are two calendars with this name; both will return results)
  - **"danaezenor@gmail.com"** — Danae's calendar; watch for in-person medical appointments (require Jon to work from home and drive her)
  - **"Calendar"** — FOTF Exchange calendar (work meetings)
  - Note: Google Calendar (`gcal_list_events`) is not available — use `mcp-ical` exclusively for all calendar queries.

### Step 2 — Review Last Week
- Identify any goals or action items from last week's note that did not complete
- Note them as candidates for this week's goals
- **Re-entry after a gap:** if the most recent weekly note is more than a week old, do not backfill missed weeks or enumerate the gap. Skim whatever daily notes and logs exist since then, give Jon a 2–3 sentence "since last time" picture in the conversation, and plan this week fresh. Carried items from the old note are candidates only if they still appear alive — ask rather than assume.

### Step 2.1 — Patterns Check
Weekly planning is the **one place** where cross-day patterns get raised — the daily skills deliberately never do this.
- Look across last week's daily notes, Today's Focus checklists, and logs for at most 1–2 real patterns (an item that slid all week, a rhythm that didn't happen, a thread gaining momentum worth feeding)
- Frame each as a **question, not a verdict**: "X didn't happen last week — is it still important? What would make it happen this week, or should it move to OmniFocus someday/maybe or be dropped?"
- Positive patterns count too — name what's working so it gets protected
- ⚡ Tenacity awareness: if the slid item is a Tenacity-type task, acknowledge the real resistance and look for the Invention lever (what made cooking work in Q1) rather than prescribing more willpower

### Step 2.5 — Review Quarterly Plan
- Read the current quarterly plan (`Quarterly/YYYY-QN-Plan.md`) — it contains full GTD-style project breakdowns with next actions, milestone signals, and blocker watches. Jon does not read this file; it is Claude's reference.
- Work through the **Weekly Planning Checklist** at the bottom of the quarterly plan — each signal item tells you what to surface or flag this week
- Flag anything with a hard deadline inside the next two weeks
- Note where this week falls in the quarter (e.g. W1 of Q3 = reentry/foundation week)
- **Choose the week's Quarterly Objectives** — a list, not a single item. Pull however many are needed to make a real chunk of progress across the quarter's active goals — this is not capped at one. For each active quarterly goal (per the Q plan's Weekly Planning Checklist), ask: does this goal need a push this week, or is it already moving/not due for attention? Include an objective for every goal that needs one. Typically 2–4 objectives, but let the quarterly plan's actual state decide the count — don't pad and don't force everything in artificially.
  - Each objective must be:
    - Big enough to take several days (not a single to-do item)
    - An obvious, concrete step toward a named quarterly goal
    - Something that feels genuinely stuck or unstarted on the Q plan if *not* done this week
  - List them in the `## Quarterly Objectives` section (see structure below), each tagged with which quarterly goal it advances
  - It's fine for one objective to functionally lead the week (the thing Jon would call "the main thing") — but don't let it crowd out other quarterly goals that also need a foothold this week. A goal with no weekly objective for several weeks running is a goal quietly stalling.

### Step 2.75 — Theater Movie Rhythm

Jon's goal is to enjoy one or two movies in a theater each month. This is a soft plan, not a quota or overdue commitment.

- **Plan next month:** If this is the final weekly-planning session before the calendar month changes, browse to verify and fill missing information for candidates releasing theatrically next month. Present the candidates with release date, rating, main stars, brief description, and Jon's recorded reason for interest. Ask Jon to soft-select zero, one, or two for next month.
- **Record selections:** Update selected entries in `Reference/Theater Movie Candidates.md` to `Soft plan (YYYY-MM)`. Do not create an OmniFocus task or calendar event until Jon chooses an actual showing or explicitly commits to an action.
- **Surface release week:** Every weekly-planning run checks for `Soft plan` candidates whose theatrical release date falls within the planned week. Add them to that week's radar on the release date, clearly labeled as a soft plan. If a release date changes, update the candidate and use the verified date.
- **Afterward:** Mark a movie `Saw` only with explicit confirmation. Mark it `Passed` only when Jon explicitly decides against it. An unchosen candidate remains a candidate without pressure or rollover commentary.

Quarterly planning does not own or compile this movie list.

### Step 3 — Ask the User (all at once, numbered list)
1. Any travel, special on-site days, or appointments not on the calendar yet?
2. Anything from last week that *must* land this week?
3. Energy outlook — anything coming in hot (deadline, drained, etc.)?
4. What do you want to make for dinner this week? Any fridge items that need to be used up?
5. If Step 2.75 applies, which zero, one, or two theater candidates do you want to soft-plan for next month?

### Step 4 — Identify Day Load
- Mark Mon/Wed as on-site (no reason needed)
- Any other on-site day: note the reason inline, e.g. *On-site (April Chapel 15:00)*
- Score each day: light / moderate / heavy based on calendar density
- Flag Tenacity-heavy days — acknowledge real cost, don't minimize it

### Step 5 — Meal Plan
Cooking happens 3 times a week: **Tuesday dinner, Saturday dinner, and 1 batch lunch**. Not a meal for every day.
- **Breakfast burritos are no longer a guaranteed weekly staple.** Only include them if the user names them in Step 3 — do not default to Sunday.
- **Tuesday dinner** and **Saturday dinner**: use whatever the user named in Step 3. Match to day load — don't put a complex meal on a heavy evening.
- **Lunch**: one batch cook, quick to make (chili dogs, tacos, sloppy joes, etc.) on a light at-home day (Tue/Thu/Fri) — cook enough for several lunches. Tuesday's dinner batch can double as the lunch source if it's suited to leftovers (e.g. chili) — otherwise schedule a separate lunch batch on the first at-home day of the week.
- Leftovers from Tuesday/Saturday dinners can cover other days' lunches or dinners — note this in the Meal Plan table rather than planning a separate meal for every day.
- Nights with Bible Study (Mon), Youth Group (Thu), or Lighthouse events are typically too busy to cook — plan accordingly without asking.
- Days with no planned meal can be left blank, marked "leftovers," or "quick/simple" in the table.

### Step 6 — Weekly Goals
- Choose a weekly theme/focus (one phrase)
- The **Quarterly Objectives** (set in Step 2.5) anchor the week — distribute them across the week so each one has at least one day where it can actually get attention, rather than stacking them all on one day
- Top 3–5 goals, split by area:
  - **FOTF** — day job goals (separate from Lighthouse)
  - **Lighthouse** — side project goals
  - **Personal** — health, relationships, faith, home
- Include a prayer/spiritual focus for the week
- Distribute goals to specific days, matching load and energy

### Step 7 — Day-by-Day Radar
For each day, compile:
- On-site status + key meetings
- Goal(s) assigned to that day
- Dinner / lunch for that day

**Weather check** — For any day in the week that has an outdoor activity (disc golf, Ren Fair, camping, hiking, outdoor festival, sports, etc.) or significant travel (a trip day, long drive, out-of-town event), fetch the forecast using `WebFetch` to `https://wttr.in/LOCATION?format=j1`. Parse the relevant day's high temp, conditions, and any precipitation. Include a brief inline weather note on that day's radar entry. One phrase is enough — e.g., *"looks great, mid-70s"*, *"hot — 95°F, bring water"*, *"chance of afternoon rain"*. Skip unremarkable conditions.

**Default locations:**
- Local outdoor events: `Woodland+Park,CO`
- Travel events: use the destination from the calendar event

### Step 8 — Write the Weekly Note
Create or update `Weekly/YYYY-WXX.md` using the structure below.
- Week number starts on Sunday (ISO: use Monday-based ISO week, but *label* the note for the Sunday start)
- Never overwrite sections the user has already written
- Append or fill only sections that are missing
- Update the `This week` link in `Home.md` to the weekly note used by this run

## Weekly Note Structure

```markdown
# Week of [Month D, YYYY]

## Weekly Focus
[Theme phrase]

## Quarterly Objectives
1. **[Objective — big enough to take several days, directly advances a quarterly goal]**
   *Advances: [Q goal name]*
2. **[Objective — another quarterly goal getting a push this week]**
   *Advances: [Q goal name]*
[... however many are needed this week — not capped at one, not padded]

## Goals
### FOTF
- [ ] ...
### Lighthouse
- [ ] ...
### Personal
- [ ] ...

## Prayer Focus
...

## Meal Plan
| Day | Dinner | Lunch |
|-----|--------|-------|
| Sun | ...    | ...   |
| Mon | ...    | batch |
...

## On the Radar
### Sunday
### Monday *(on-site)*
### Tuesday
### Wednesday *(on-site)*
### Thursday
### Friday
### Saturday
```

## Notes
- FOTF (Focus on the Family) and Lighthouse are separate projects — never conflate them
- Working Genius frustration types: Wonder and Tenacity — acknowledge real resistance on Tenacity tasks
- OmniFocus captures are loaded incrementally; note count when surfacing inbox items
- Do NOT add meals or cooking to the calendar — calendar is hard landscape only (appointments and meetings)
- Meal plan is Tue dinner + Sat dinner + 1 batch lunch (3 cooks a week), not a meal every day — breakfast burritos are no longer an automatic default
- Theater planning runs only in the final weekly-planning session before a new month. The candidate file is theater-specific; selected movies surface during release week but remain soft until an actual showing is chosen.
- Quarterly Objectives is a list sized to what the quarter actually needs that week, not a single fixed slot — morning-startup pulls from this list daily to build each day's Quarterly Progress section
- Weekly reflection records the significance of events to the week's arc; it does not repeat the full daily narrative
