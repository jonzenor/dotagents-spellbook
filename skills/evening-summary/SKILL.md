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

6. **Follow wikilinks in today's note** — identify any `[[File]]` or `[[Folder/File]]` links mentioned in the daily note. Read those linked files and look for content added today (dated entries, new rows in tables, new meeting log entries, recently added items). Summarize what was added in the **Don't lose this** section if it's time-sensitive or easily forgotten. Pay particular attention to Mentoring notes (meeting logs), Prayer lists, and any file with dated entries.

7. **Medical log** — scan today's daily note for any health- or medical-related mentions (symptoms, appointments, medications, diagnoses, test results, injuries, or anything health-adjacent). If found, append a dated entry to `Reference/Logs/Medical Log.md` under the `## Log` section (newest at top). Format: `**[[Daily/YYYY-MM-DD|YYYY-MM-DD]]** — [brief factual summary of what was mentioned]`. If nothing health-related came up, skip this step entirely.

8. **Cooking log** — scan today's daily note for any mention of actually cooking a meal: what was made, how it turned out, what worked, what didn't, substitutions, timing notes. If found, prepend a dated entry to `Reference/Logs/Cooking Log.md` under the `## Log` section (newest at top). Format: `**[[Daily/YYYY-MM-DD|YYYY-MM-DD]]** — [meal name, result, any notes for next time]`. Only log meals that were actually cooked today — skip plans, intentions, carries, or mentions that cooking didn't happen.

9. **Scan for new ideas** — look through `### Random Thoughts`, the day's narrative, and any passing mentions in the note for ideas, side projects, product thoughts, or anything that surfaced but has no next action or home yet. Note any suggested vault home (Topics/, project note, Mentoring, etc.).

9. **Append** the AI Summary section to today's daily note (see format below).

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

**New ideas worth capturing:**
- [Idea from today's Random Thoughts or narrative — with a suggested vault home if it belongs somewhere: Topics/, project note, Mentoring, etc.]
- [Omit this section entirely if nothing new surfaced]

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
- **Medical Log: prepend entries under `## Log`** — add newest entry at the top of the log section. Read the file first. Only write if there is actual health content from today's note.
- **Cooking Log: prepend entries under `## Log`** — add newest entry at the top of the log section (`Reference/Logs/Cooking Log.md`). Read the file first. Only write if cooking was mentioned in today's note.
- **Linked files: read only** — follow wikilinks to gather context; do not edit linked files during evening summary (except the weekly note and Medical Log).
- Read each file before writing to confirm current state.
- Use `[[Person/Name]]` wikilink syntax when referencing people.
- Keep the tone direct and personal — this is a private journal, not a report.
- If a section has nothing worth saying (e.g. no real lessons learned today), omit it rather than padding with filler.
- Do not summarize things already captured well in the note — add insight, not repetition.

### No assumptions — only what the note explicitly says

This is the most important rule for avoiding errors. Do not infer status, completion, or intent from indirect evidence.

**Action items — only check off when explicitly confirmed:**
- Only mark a weekly action item complete if today's note explicitly says the thing is done. Partial progress, related progress, or a downstream result is not confirmation.
- Example: "inbox is now clean" does not mean the upstream import task is complete. "Processed what I imported" is a different statement than "import is finished."
- If in doubt, leave the item unchecked.

**Multi-step processes — treat each step independently:**
- Many tasks in this vault have multiple steps (e.g. import stickies → process inbox → clear inbox). Progress on one step does not imply progress on others.
- When the note mentions a step in a process, describe only that step — do not extend the claim to the whole process.

**Calendar vs. actual:**
- Do not assume a calendar event happened just because it was on the schedule. Only mark meetings as done or note them in reflections if the daily note confirms they occurred.

**Carry-forward intent:**
- If Jon explicitly defers something ("doing this tomorrow"), reflect that in the summary as a plan — not as something that happened today.

**When uncertain, describe narrowly:**
- If the note is ambiguous about whether something is complete, describe only what is clearly stated and flag it rather than filling in the gap with an inference.
