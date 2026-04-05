---
name: afternoon-checkin
description: Afternoon check-in routine for Jon's Obsidian life vault. Reads today's daily note and weekly note, then outputs a focused summary of what still needs to get done today, what might have been forgotten, and only flags upcoming calendar items if action is required this afternoon. Does NOT update any notes — output only. Use when the user says "afternoon check-in", "afternoon review", or "midday check-in".
---

# Afternoon Check-In

## Steps

1. Read today's daily note: `Daily/YYYY-MM-DD.md`
2. Read this week's weekly note: `Weekly/YYYY-WXX.md`
3. **Check Google Calendar** (timezone: America/Denver) — fetch remaining events today from all three calendars: `jonzenor@gmail.com`, `family18039985066750084653@group.calendar.google.com`, `danaezenor@gmail.com`. Use this as the live source of truth for what's actually still on the schedule — it may differ from what was written in the morning note.
4. **Query OmniFocus** — two targeted queries only:
   a. Flagged or planned-today tasks that are still `Available` (not yet completed) — these were Jon's stated intentions for the day
   b. Tasks that are `Overdue` or `DueSoon` — anything that ticked into urgency since this morning
5. Output the afternoon review — **do not edit any files**

## Output Format

### Still Needs to Get Done Today
- Pull from "Get done today" and any open action items in the daily note
- Note status if mentioned (e.g. "in testing", "waiting on X")
- Include personal/financial tasks (bills, errands) — these slip easily

### Likely Forgotten / Easy to Miss
- If current time is before 13:00: mention any lunch plans or leftovers noted for today
- Food that needs to be cooked tonight
- Checks, payments, or deliveries being watched for
- Bible reading streak — only surface if it hasn't happened yet today (no reading in `## Today's Reading`); omit entirely if reading is already present in the note
- Anything promised to someone else

### Incomplete Intentions
- List any flagged or planned-today OmniFocus tasks still marked Available
- Keep it brief — one line each; this is a nudge, not a guilt trip
- Omit this section entirely if everything is done or nothing was flagged/planned today

### Urgent from OmniFocus
- List any tasks that are Overdue or DueSoon that weren't in this morning's note
- Omit this section entirely if nothing new has become urgent

### Later Today (Only If Action Required Now)
- Use the live calendar, not the morning note, as the source of truth for what's still on the schedule
- Only include if something happening later today needs prep *right now*
- If a meeting was cancelled since this morning, note the free time
- Skip all future days — tomorrow, this weekend, next week are out of scope

## Rules
- Output only — never write to or update any notes
- Keep it tight — this is a quick scan, not a full replanning session
- Calendar: use live data for today only; never surface future days
- OmniFocus: two queries only (flagged/planned-today incomplete + overdue/due-soon); never dump the full task list
- Omit any section that has nothing to surface — don't pad with filler
