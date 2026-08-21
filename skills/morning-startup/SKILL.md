---
name: morning-startup
description: Morning startup routine for Jon's Obsidian life vault. Checks calendars and current context, then writes a Today's Focus with the hard landscape and exact OmniFocus execution perspectives to open—never copied task lists. Use when the user says "morning startup", "start my day", "morning routine", or asks what to focus on today.
---

# Morning Startup

## What this does

Creates or refreshes the AI-owned `## Today's Focus` section in today's daily note. It contains the calendar hard landscape, the OmniFocus context lists worth opening, and the useful weekly radar. It never copies individual tasks or the active-project list into the journal.

The safety boundary is **section ownership**, not append-only file handling:
- Never modify Jon-authored journal content under `## What Happened Today?`, `## What Stood Out?`, `## What Needs Follow Up?`, or `## Random Thoughts`, including Jon-authored subheadings such as Captain's Log entries.
- AI-designated sections may be updated by the AI. For this skill, those are `## Today's Focus` and the skill-generated blockquote under `## Brainstorm Today`.
- Preserve any content whose ownership is unclear. Do not treat an unfamiliar heading as AI-owned merely because it appears near an AI section.

This is a **morning briefing, not a task ledger**. Its job is orientation: gather the full shape of the day and the important things coming that get lost in the fullness of the calendar app, all in one place. Deadlines get surfaced plainly, once. Jon decides what the day holds — the brief describes, it doesn't direct.

## Steps

1. **Determine today's date and day of week** from the `currentDate` context variable.
   - Derive the full day name (e.g. Monday, Thursday) — this is needed for matching rhythms and weekly note items.

2. **Open today's daily note** at `Daily/YYYY-MM-DD.md`.
   - If it doesn't exist, create it using the standard template (see below).
   - **If today is Sunday**, insert the following line at the very top of the note (above all other content), then the rest of the template below it:
     ```
     [[Bible/Church/Church Notes YYYY-MM-DD]]
     ```
     Use today's actual date. Do **not** create the linked file — just place the wikilink so it's ready to tap in Obsidian.
   - Read the full file before changing anything. Identify the exact boundaries of existing AI-owned sections and Jon-authored sections.

3. **Read yesterday's daily note**. Check `Daily/YYYY-MM-DD.md` first; if absent, check `Daily/YYYY/MM/YYYY-MM-DD.md`.
   - Use unfinished narrative only as context for selecting useful OmniFocus lists; never copy it forward as a task.
   - If yesterday's note is empty or missing, skip this step silently — build today's brief from the calendar, weekly note, and Rhythms instead. After a multi-day gap, at most one warm line acknowledging the restart; never list or count what was missed.

4. **Read this week's weekly note** at `Weekly/YYYY-WXX.md` (weeks start Sunday).
   - Extract the week's focus, priorities, and any action items relevant to today.
   - Pull any items explicitly tied to today's day of the week (e.g. cooking schedule entries for today, day-specific tasks). Surface these in the brief.
   - Read the `## Meal Plan` table. If today has a planned cook, mention it under "Useful context" unless it has a real scheduled time. Pull the **remaining** planned meals for the rest of the week into "On the radar this week" as a single line. Skip blank, leftovers, or quick/simple entries.
   - Use weekly priorities to decide which context lists matter today; do not turn weekly goals into daily-note tasks.

5. **Read `Rhythms.md`**.
   - From the **Structure** table: note if today is an on-site work day.
   - From the **Commitments** table: pull any commitments scheduled for today's day of the week. Note the time and whether others are counting on you.
   - From the **Rhythms** table: pull any softer rhythms scheduled for today (e.g. cook for family on Tuesday/Friday).
   - From **Daily Rhythms**: include the standard morning/daytime/evening habits as a reminder in the output.

6. **Check calendars** (timezone: America/Denver).

   Use the `mcp-ical` tool exclusively for all calendar queries. Query all of the following in parallel:

   - **"Jon's Calendar"** — personal events, appointments, paydays, Jon's own medical appointments
   - **"Family"** — query twice; there are two calendars with this name and both may have events
   - **"danaezenor@gmail.com"** — Danae's calendar
   - **"Calendar"** — FOTF Exchange calendar (work meetings)

   For each calendar:
   - Fetch **all events today** — include every timed event in the hard landscape.
   - Fetch events for the **rest of the week** — filter to actionable or personally significant items only. Skip purely informational entries.
   - Also scan **up to two weeks out** for significant one-off items only (appointments, trips, surgeries, school events, deadlines) — these are exactly the things that get buried in a full calendar app. Surface them at the bottom of "On the radar" with their date. Skip all recurring events in this look-ahead.
   - Convert all event times from UTC to America/Denver (MDT = UTC−6 in summer, MST = UTC−7 in winter).

   **Event series** (F1 races and practice sessions, sports schedules, any cluster of related events — typically on the Family calendar): never list each session as its own line. Collapse the whole series into one line with only the times that matter, e.g. "F1 weekend: practice Fri, quali Sat 09:00, race Sun 07:00". No need to attribute who added them.
   **Recurring logistical reminders** (e.g. Trash Day): only surface on the day itself or the evening before — never earlier in the week radar.
   **Danae's appointments**: flag any in-person (non-virtual) doctor or medical appointments — these require Jon to work from home and drive her. Treat these as high-priority items. Surface them clearly in "Today's hard landscape" if today, or prominently in "On the radar" if later this week, with a note: *"In-person — work from home, drive Danae."*
   **FOTF Exchange (Calendar) — radar guidance**: include notable one-time events, conflicts, or unusual schedule items. Recurring standup noise can be omitted from the radar unless there's something worth flagging.
   **Schedule conflicts**: if two calendar events or commitments overlap in time, flag the conflict inline in the hard landscape with ⚠️ and a note.

7. **Weather check** — After the calendar pass, scan today's hard landscape and the week's radar for any event that involves outdoor activity (disc golf, Ren Fair, hiking, camping, sports, outdoor festivals, etc.) or significant travel (a trip day, a long drive, an out-of-town event).

   For each such event, fetch the forecast using `WebFetch` to `https://wttr.in/LOCATION?format=j1` (replace LOCATION with the city or area; use `Woodland+Park,CO` as the default for local events, `Colorado+Springs,CO` for on-site work days). Parse the JSON to pull the relevant day's high temp, low temp, and weather description.

   Include a brief inline weather note alongside the event in the hard landscape or radar. Keep it to one phrase — enough to know whether to bring water, a jacket, or an umbrella. Examples: *"92°F, sunny — bring water"*, *"scattered afternoon showers"*, *"looks great, mid-70s"*. Only add a note when the weather is meaningfully good, bad, hot, cold, or rainy. Skip it if conditions are unremarkable.

   **Default locations:**
   - Local outdoor events: `Woodland+Park,CO`
   - Travel events: use the destination from the calendar event name or notes

8. **Check standing instructions** — read `Keeper Instructions.md`.
   - Run entries under `## Daily`.
   - Run any `## Before Events` instruction whose trigger matches today's calendar or radar. For example, before a Kevin Shireman 1:1, read his Person note and surface the requested preparation brief.
   - Standing instructions remain active after use; never remove or mark them complete.

9. **Check the Almanac** — read `Almanac.md` at vault root. Pull every entry dated today **or earlier** into today's daily note. Put meeting-specific context near the matching meeting and general information under "Useful context." Preserve an overdue entry's original target date inline.
   - After copying a one-time entry, delete it immediately and remove any empty date heading. Surfacing is its terminal state; do not wait for the underlying matter to be completed.
   - An entry tagged `*(annual; added YYYY-MM-DD)*` is renewable. Before deleting the consumed occurrence, add the same reminder under the same month and day in the following year, preserve its provenance and wording, and replace the metadata with `*(annual; added [today's date])*`. Keep date headings ascending and avoid duplicates. Then delete the consumed occurrence.
   - If `Almanac.md` is missing or has no due/overdue entries, skip silently.

10. **Build one Brainstorm item** — write one AI-generated question drawn from recent notes, plans, and live tensions. It must be generative rather than accountability framed. Do not copy an OmniFocus task into the daily note.

11. **Invoke `gtd-omnifocus` in `orient` mode automatically** — supply today's hard landscape, location, weekly priorities, and likely energy/time. Do not ask Jon to run the GTD skill separately.
   - Use the exact perspective recommendations and nonzero deadline/flag counts it returns. Do not copy individual tasks into the daily note.
   - Orient mode is read-only; morning startup never creates, edits, or reclassifies OmniFocus work.
   - If orient mode reports an outage after its retry, omit OmniFocus guidance and counts, continue the brief from the vault and calendar, and mention the omission only in the final report.

12. **Person context** — for each person who appears in today's hard landscape as a meeting, call, or appointment, read their Person and/or Mentoring note if it exists. Put a brief “Before you talk to X” line under "Useful context" or beside the matching event. Keep it to 1–2 lines.

13. **Write or refresh** the Today's Focus section (see format below).
   - Replace an existing AI-owned section in place through the next heading of equal or higher level, leaving surrounding Jon-authored sections byte-for-byte unchanged.
   - If it is missing, add it after Jon's template sections at the bottom of the note.
   - On reruns, consolidate prior AI-created duplicates into one current `## Today's Focus`.
   - Never replace the whole daily note to accomplish a section update.

## Output format notes

- **Week's focus**: Pull the first line of the `## Focus This Week` section from the weekly note and include it as a single italic line at the top of Today's Focus, e.g. `*Week's focus: ...*`
- **Mentoring links**: When a mentoring session appears in the hard landscape, use the wikilink to the mentoring note, e.g. `[[Mentoring/Zac Story]]`, `[[Mentoring/Jeremy]]`, `[[Mentoring/Tyler]]`
- **Brainstorm Today**: The `### Brainstorm Today` section contains one AI-generated question. It is a background prompt, not a copied task.

## Output format

```markdown
## Today's Focus

**Today's hard landscape:**
- HH:MM — [Event or commitment with a specific time]
- (All day) — [All-day calendar event worth noting]

**OmniFocus perspectives to open:**
- `[Exact perspective name]` — [why it fits today's location, capacity, or protected commitments]

*[Concise overdue/due/flagged counts, only when nonzero]*

**Useful context:**
- [Due Almanac information, person preparation, or today's meal/rhythm; omit when empty]

**On the radar this week:**
- [Day Mon DD] — [Event or commitment] *(time if applicable)*

*[Daily habits tagline — one line, always at the bottom: e.g. "Rhythms: Bible → exercise → family time → read before bed"]*

```

### Section guidance

- **Today's hard landscape**: Include only calendar events and commitments with a real time. Put untimed meal/rhythm information under "Useful context." If nothing is scheduled, omit this section.
- **OmniFocus perspectives to open**: Name exact existing custom perspectives. Do not list individual tasks, projects, copied checkboxes, or raw tag combinations.
- **On the radar**: Only actionable or personally significant items for the rest of the week. One line each. Skip purely informational entries. For any day that is going on-site but is NOT a standard on-site day (standard = Monday and Wednesday), include the reason in parentheses, e.g. *"On-site (April Chapel 15:00)"*. No reason needed for Monday/Wednesday since those are always on-site. Include remaining planned cook nights/meals from the weekly note's Meal Plan table (see step 4) — skip days with no meal planned.
- **Daily habits tagline**: Always one line at the bottom. Never a section — just a quiet reminder of the daily rhythm. Use the Daily Rhythms from Rhythms.md.

## Standard daily note template

On Sundays, the note begins with a church notes link before any other content:

```markdown
[[Bible/Church/Church Notes YYYY-MM-DD]]

## What Happened Today?
```

All other days (and the remainder of Sunday's note after the link):

```markdown
## What Happened Today?

## Brainstorm Today

> **AI:** [Assistant-generated question — what Jon's recent notes, plans, and threads suggest he needs to be thinking about; never a reworded OmniFocus item]

## What Stood Out?

## What Needs Follow Up?

## Random Thoughts

## Today's Reading

See also: [[Prayer/War-Room]]

```

## Rules

- **Protect Jon's writing** — never alter Jon-authored journal sections or their subheadings. Existing AI-designated sections may be replaced in place on reruns.
- **No duplicate AI sections** — keep one current `## Today's Focus`. Refresh it instead of appending another copy.
- **Almanac.md is the one exception** — this skill both reads it and deletes due/overdue entries (Step 9), once those entries have been pulled into today's note. Never delete an entry without also surfacing it. Renew annual entries for the following year before deleting the consumed occurrence.
- Read the full file before writing to confirm section ownership and boundaries.
- Use `[[Person/Name]]` wikilink syntax when referencing people.
- Keep the tone direct and personal — this is for Jon, not a report.
- **Briefing voice, not supervisor voice.** Describe the shape of the day; don't issue orders or apply pressure ("no excuses", "no better day than today"). State deadlines plainly and trust Jon with them.
- **After missed days, never enumerate or count what was missed.** The brief always starts from today.
