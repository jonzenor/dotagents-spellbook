---
name: morning-startup
description: Morning startup routine for Jon's Obsidian life vault. Checks Google Calendar, reads the weekly note and yesterday's daily note, then appends a Today's Focus section to today's daily note. Use when the user says "morning startup", "start my day", "morning routine", or asks what to focus on today.
---

# Morning Startup

## What this does

Appends a `## Today's Focus` section to today's daily note. **Never overwrites or inserts — append only.**

## Steps

1. **Determine today's date and day of week** from the `currentDate` context variable.
   - Derive the full day name (e.g. Monday, Thursday) — this is needed for matching rhythms and weekly note items.

2. **Open today's daily note** at `Daily/YYYY-MM-DD.md`.
   - If it doesn't exist, create it using the standard template (see below).
   - Read the full file before appending anything.

3. **Read yesterday's daily note** (`Daily/YYYY-MM-DD.md` for yesterday).
   - Pull any unfinished items from `### What Needs Follow Up?` and `## Today's Focus` must/try lists.
   - Note any active trends from the AI Summary.

4. **Read this week's weekly note** at `Weekly/YYYY-WXX.md` (weeks start Sunday).
   - Extract the week's focus, priorities, and any action items relevant to today.
   - Pull any items explicitly tied to today's day of the week (e.g. cooking schedule entries for today, day-specific tasks). Surface these in the brief.

5. **Read `Rhythms.md`**.
   - From the **Structure** table: note if today is an on-site work day.
   - From the **Commitments** table: pull any commitments scheduled for today's day of the week. Note the time and whether others are counting on you.
   - From the **Rhythms** table: pull any softer rhythms scheduled for today (e.g. cook for family on Tuesday/Friday).
   - From **Daily Rhythms**: include the standard morning/daytime/evening habits as a reminder in the output.

6. **Check Google Calendar** (timezone: America/Denver):
   - Fetch **all events today** — include every one of them.
   - Fetch events for the **rest of the week** — filter to only items that are actionable or important (meetings, deadlines, birthdays worth acknowledging, payday). Skip purely informational entries like cancelled events or reminders that nothing is happening.

7. **Append** the Today's Focus section (see format below).

## Output format

```markdown
## Today's Focus

**Today's hard landscape:**
- HH:MM — [Event or commitment with a specific time]
- (All day) — [All-day calendar event worth noting]

**Get done today:**
- [Prioritized list of tasks — unfinished follow-ups, weekly priorities, open actions. No must/try split, just ordered by importance.]

**On the radar this week:**
- [Day Mon DD] — [Event or commitment] *(time if applicable)*

*[Daily habits tagline — one line, always at the bottom: e.g. "Rhythms: Bible → exercise → family time → read before bed"]*
```

### Section guidance

- **Today's hard landscape**: The fixed shape of the day (GTD concept). Include only items with a specific time — calendar events and Rhythms.md commitments that have a scheduled time. If a time-based commitment has a known exception (e.g. Youth Group cancelled for spring break), note it inline. Do NOT include untimed items like "work on-site" — those are tasks, not landscape. If a meal from the weekly cooking schedule has a time context (e.g. Thursday Lunch), include it here; otherwise put it in "Get done today." If nothing is scheduled, omit this section.
- **Get done today**: Everything that needs doing but isn't pinned to a time. Pull from yesterday's unfinished follow-ups, weekly note action items, and anything with a deadline or dependency. Include untimed rhythms (e.g. work on-site, cook for family tonight) and cooking if no specific time. Order by importance — most critical first. If a recurring item keeps sliding (doctor's appointment, Bible reading, GTD), surface it with a brief nudge. Keep this list tight — 4–6 items max.
- **On the radar**: Only actionable or personally significant items for the rest of the week. One line each. Skip purely informational entries.
- **Daily habits tagline**: Always one line at the bottom. Never a section — just a quiet reminder of the daily rhythm. Use the Daily Rhythms from Rhythms.md.

## Standard daily note template

```markdown
### What Happened Today?

### Who Did I Interact With?

### What Stood Out?

### What Needs Follow Up?

### Random Thoughts
```

## Rules

- **Append only** — never insert into or overwrite existing content.
- Read the file before writing to confirm where it ends.
- Use `[[Person/Name]]` wikilink syntax when referencing people.
- Keep the tone direct and personal — this is for Jon, not a report.
