---
name: evening-summary
description: Evening summary routine for Jon's Obsidian life vault. Reads today's daily note and recent history, then appends an AI Summary with lessons learned, things not to lose, tomorrow's focus, and trends. Use when the user says "evening summary", "summarize my day", "end of day", or asks for a daily summary.
---

# Evening Summary

## What this does

Appends an `## AI Summary` section to today's daily note. **Never overwrites or inserts — append only.**

## Steps

1. **Determine today's date and day of week** from the `currentDate` context variable.

2. **Read today's daily note** at `Daily/YYYY-MM-DD.md` in full.

3. **Read the last 3–5 daily notes** to identify recurring themes and trends across days, not just today.

4. **Read this week's weekly note** (`Weekly/YYYY-WXX.md`, weeks start Sunday) in full — both for broader context and to identify what needs updating.

5. **Update the weekly note** with what happened today:
   - In **Active Action Items**: check off any items that were completed today (change `- [ ]` to `- [x]`).
   - In **Mentee Check-ins This Week**: mark any mentee sessions that happened today.
   - In **Cooking This Week**: fill in a Result if a meal was made today.
   - In **Notes & Reflections**: append a brief dated entry (e.g. `**Thu Mar 26** — [1–2 sentences on how the day went, anything notable for the week's arc]`). Do not overwrite existing reflections.

6. **Append** the AI Summary section to today's daily note (see format below).

## Output format

```markdown
## AI Summary

**What happened:** [1–3 sentence narrative of the day — events, tone, what mattered]

**Lessons learned:**
- [Specific, actionable insight from something that went wrong or went well]
- [Only include if there's a real lesson — skip filler]

**Don't lose this:**
- [Things that could fall through the cracks: upcoming deadlines, important decisions pending, time-sensitive relationships]

**Tomorrow's focus:**
- [Concrete next actions derived from today's unfinished items and follow-ups]

**Trends worth watching:**
- **[Theme]** — [Observation across multiple days. Name the pattern, why it matters, and what would move the needle]
```

### Section guidance

- **What happened**: Write it like a friend summarizing the day — honest, human, not clinical. Acknowledge hard days as hard.
- **Lessons learned**: Only include genuine insights. A lesson should change future behavior. Skip platitudes.
- **Don't lose this**: The most important section. Capture things that are easy to forget but costly if forgotten — upcoming events, pending decisions, relationship moments, financial deadlines. Also flag anything from today that belongs in a deeper note (Mentoring, Person, Prayer, etc.).
- **Tomorrow's focus**: Pull from `### What Needs Follow Up?`, unfinished `## Today's Focus` items, and anything that needs a next action. Keep it short — 2–4 items max.
- **Trends worth watching**: Look across the last several days. Name recurring themes (spiritual drift, health neglect, over-commitment, relational opportunities closing). Be honest, not harsh. If a trend has improved, note that too.

## Rules

- **Daily note: append only** — never insert into or overwrite existing content in the daily note.
- **Weekly note: targeted edits allowed** — check off completed items, fill in results, and append to Notes & Reflections. Never rewrite or delete existing weekly note content.
- Read each file before writing to confirm current state.
- Use `[[Person/Name]]` wikilink syntax when referencing people.
- Keep the tone direct and personal — this is a private journal, not a report.
- If a section has nothing worth saying (e.g. no real lessons learned today), omit it rather than padding with filler.
- Do not summarize things already captured well in the note — add insight, not repetition.
