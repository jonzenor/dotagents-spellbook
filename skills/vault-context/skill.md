---
name: vault-context
description: Loads Jon's full life and work state into context. Reads active projects, recent daily notes, weekly note, and priorities from the last 7 days. Use when the user says "/vault-context", "load my context", "get up to speed", or wants the agent to know everything relevant before starting work.
---

# Vault Context

## What this does

Reads the vault and produces a concise briefing of Jon's current life and work state — active projects, recent reflections, open priorities, and anything time-sensitive. Output is displayed to the user, not written to any file.

## Steps

1. **Determine today's date and day of week** from the `currentDate` context variable.

2. **Read the last 7 days of daily notes** (`Daily/YYYY-MM-DD.md`). Pull:
   - Unfinished items from `### What Needs Follow Up?` and `## Today's Focus`
   - Any named priorities, decisions pending, or time-sensitive items
   - Active trends from AI Summaries

3. **Read this week's weekly note** (`Weekly/YYYY-WXX.md`):
   - Unchecked action items
   - Week's focus and schedule
   - Notes & Reflections

4. **Read `Projects/` folder** — scan all project notes for active work. For each, note the current status and any next actions mentioned.

5. **Read `Rhythms.md`** — note today's commitments and any pending/unscheduled items.

6. **Read `Vision.md`** if it exists — for values/priorities framing.

7. **Output the briefing** (see format below). Do not write anything to any file.

## Output format

```
## Current Context — [Day, Month DD YYYY]

**Who I am / what I'm optimizing for:**
[1–2 sentences from Vision.md or inferred from patterns — the "why" behind the work]

**Active projects:**
- [Project name] — [status and immediate next action]

**Open priorities (last 7 days):**
- [Item] — [context/deadline if known]

**On the calendar soon:**
- [Day] — [event]

**Commitments today:**
- [From Rhythms.md for today's day of week]

**Trends & things not to lose:**
- [Recurring theme or at-risk item from recent daily notes]
```

## Section guidance

- **Active projects**: Only include projects with open next actions. Skip anything stalled with no near-term movement.
- **Open priorities**: Pull the most important unfinished items across all notes from the last 7 days. De-duplicate — if the same item appears multiple days, list it once with the most recent context.
- **On the calendar soon**: Anything within the next 5 days that requires action or attendance. Pull from weekly note schedule.
- **Commitments today**: What's on the hard landscape for today specifically — from Rhythms.md and the weekly schedule.
- **Trends**: Surface 2–3 recurring themes from recent AI Summaries (spiritual, relational, organizational, work). Be honest and concise — this is a briefing, not a journal entry.

## Rules

- Output only — do not write to any file.
- Keep the briefing tight enough to read in 2 minutes.
- Use `[[wikilink]]` style only if referencing something by name that has a note — otherwise plain text is fine.
- If a section has nothing worth saying, omit it.
