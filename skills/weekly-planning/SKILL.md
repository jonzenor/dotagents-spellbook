---
name: weekly-planning
description: Full weekly planning routine for Jon's Obsidian vault. Loads calendar, rhythms, OmniFocus tasks, and last week's rollover, then guides meal planning (3 dinners + 1 lunch), sets weekly goals across FOTF/Lighthouse/Personal, builds a day-by-day radar, and writes the week's note. Use when user says "weekly planning", "plan my week", "plan the week", or "weekly review".
---

# Weekly Planning

## Quick Start

Run this skill at the start of each week (Sunday or Monday morning). It loads full context, asks a few focused questions, then builds out the weekly note.

## Workflow

### Step 1 — Load Context (run in parallel)
- Read `/Users/jonzenor/ObsidianVaults/Life of Asgard/Rhythms.md`
- Read last week's weekly note (`Weekly/YYYY-WXX.md`) for rolled items
- Read this week's weekly note if it already exists
- Read the current quarterly plan (`Quarterly/YYYY-QN-Plan.md`) — determine which quarter it is from today's date
- Query OmniFocus for overdue and flagged tasks: `query_omnifocus` with `{"status": ["Overdue", "DueSoon"], "fields": ["name", "project", "dueDate", "flagged"]}`
- Pull Google Calendar for the full 7 days ahead via `gcal_list_events`

### Step 2 — Review Last Week
- Identify any goals or action items from last week's note that did not complete
- Note them as candidates for this week's goals

### Step 2.5 — Review Quarterly Plan
- Identify which Q goals are most relevant *this* week — especially any with time pressure or early-quarter milestones
- Flag anything with a hard deadline inside the next two weeks
- Note where this week falls in the quarter (e.g. W1 of Q2 = foundation-setting week)

### Step 3 — Ask the User (all at once, numbered list)
1. Any travel, special on-site days, or appointments not on the calendar yet?
2. Anything from last week that *must* land this week?
3. Energy outlook — anything coming in hot (deadline, drained, etc.)?
4. What do you want to make for dinner this week? Any fridge items that need to be used up?

### Step 4 — Identify Day Load
- Mark Mon/Wed as on-site (no reason needed)
- Any other on-site day: note the reason inline, e.g. *On-site (April Chapel 15:00)*
- Score each day: light / moderate / heavy based on calendar density
- Flag Tenacity-heavy days — acknowledge real cost, don't minimize it

### Step 5 — Meal Plan
Plan 3 dinners + 1 lunch:
- **Breakfast burritos** are a weekly staple — default Sunday night. Assume Sunday unless user moves them. Do not ask about them.
- **Dinner 2 & 3**: Use whatever the user named in Step 3. Assign to cook nights (Tue/Fri/Sat-or-Sun per Rhythms). Match to day load — don't put a complex dinner on a heavy evening.
- **Lunch**: Something quick to batch on a light at-home day (Tue/Thu/Fri) — examples: chili dogs, tacos, sloppy joes. Cook enough for several lunches.
  - Schedule the cook on the first at-home day of the week
- Nights with Bible Study (Mon), Youth Group (Thu), or Lighthouse events are typically too busy to cook — plan accordingly without asking.

### Step 6 — Weekly Goals
- Choose a weekly theme/focus (one phrase)
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

### Step 8 — Write the Weekly Note
Create or update `Weekly/YYYY-WXX.md` using the structure below.
- Week number starts on Sunday (ISO: use Monday-based ISO week, but *label* the note for the Sunday start)
- Never overwrite sections the user has already written
- Append or fill only sections that are missing

## Weekly Note Structure

```markdown
# Week of [Month D, YYYY]

## Weekly Focus
[Theme phrase]

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
