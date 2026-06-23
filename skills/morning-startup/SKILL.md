---
name: morning-startup
description: Morning startup routine for Jon's Obsidian life vault. Checks Apple Calendar (Jon's Calendar, Family, Danae's, and FOTF Exchange), reads the weekly note and yesterday's daily note, then appends a Today's Focus section to today's daily note. Use when the user says "morning startup", "start my day", "morning routine", or asks what to focus on today.
---

# Morning Startup

## What this does

Appends a `## Today's Focus` section to today's daily note. **Never overwrites or inserts — append only.**

This is a **morning briefing, not a task ledger**. Its job is orientation: gather the full shape of the day and the important things coming that get lost in the fullness of the calendar app, all in one place. Deadlines get surfaced plainly, once. Jon decides what the day holds — the brief describes, it doesn't direct.

## Steps

1. **Determine today's date and day of week** from the `currentDate` context variable.
   - Derive the full day name (e.g. Monday, Thursday) — this is needed for matching rhythms and weekly note items.

2. **Open today's daily note** at `Daily/YYYY-MM-DD.md`.
   - If it doesn't exist, create it using the standard template (see below).
   - Read the full file before appending anything.

3. **Read yesterday's daily note** (`Daily/YYYY-MM-DD.md` for yesterday).
   - Pull any unfinished items from `### What Needs Follow Up?` and `## Today's Focus` must/try lists.
   - If yesterday's note is empty or missing, skip this step silently — build today's brief from the calendar, weekly note, and Rhythms instead. After a multi-day gap, at most one warm line acknowledging the restart; never list or count what was missed.

4. **Read this week's weekly note** at `Weekly/YYYY-WXX.md` (weeks start Sunday).
   - Extract the week's focus, priorities, and any action items relevant to today.
   - Pull any items explicitly tied to today's day of the week (e.g. cooking schedule entries for today, day-specific tasks). Surface these in the brief.

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

8. **Build the two Brainstorm items** — the `### Brainstorm Today` section gets **two** prompts, from two different sources:

   **a. The OmniFocus item** — query for `Available` tasks tagged `Thinking / Brainstorming`. Pick **one** task using this priority order:
   1. Tasks with a due date — soonest first
   2. Flagged tasks
   3. Tasks that are timely given today's context (e.g. a Lighthouse task on a day with heavy LH activity)

   Use the actual OmniFocus task — never reword or replace it with a synthesized topic. If the tag has no Available tasks, omit this item (the AI item below still appears).

   **b. The AI item** — one question the assistant genuinely thinks Jon needs to be thinking about, drawn from its knowledge of his recent notes, plans, and threads: pending decisions approaching a date, tensions visible across recent entries, ideas gaining momentum, or something Jon said he wanted to think about but never captured. This is **not** a reworded OmniFocus task — it must be distinct from item (a) and from everything currently under the Thinking / Brainstorming tag. Good picks are timely and generative ("What would a ChristianWarrior.me MVP look like with zero new time commitment?"), never accountability framed as a question ("Why hasn't X happened?"). Vary it day to day; only repeat a question if it's clearly still the live one, and re-angle it when you do.

   Both go into the `### Brainstorm Today` section of the daily note (see template and format below) — not in "Get done today." The intent is low-pressure prompts for background thinking during downtime, not tasks.

8. **Query OmniFocus** for three categories — in order of priority:

   a. **Hard deadlines** — query for tasks with `dueDate` of today or earlier and status `Available` or `Overdue`. These are non-negotiable. Mark overdue items with ⚠️. Weave into the top of "Get done today."

   b. **Today's intentions** — query for tasks that are `Available` and either `flagged: true` OR `plannedDate` = today. These are tasks Jon has already decided he wants to attempt today. Add below hard deadlines in "Get done today." Do not duplicate items already surfaced from the weekly note or yesterday's follow-ups.

   c. **Stretch goals** — based on two signals already known at this point:
      - **Location**: on-site (Monday/Wednesday, or any day flagged as on-site) vs. home
      - **Mental load**: light (0–2 timed events in hard landscape) vs. heavy (3+ events, or back-to-back blocks)
      
      Query for `Available` tasks with no due date, not flagged, no planned date — then filter by context-appropriate tags:
      - On-site → prefer tags like `Phone Calls`, `Errands`, `In Person`
      - Home → prefer tags like `5 Minutes - Quick Win`, `Low Energy`, `Deep Work`
      - Heavy day → only suggest `5 Minutes - Quick Win` or similar low-overhead tasks
      - Light day → suggest 1–2 more substantive tasks
      
      Surface 1–2 stretch items maximum, clearly labeled as optional. Never surface tasks with a future defer date (these appear as `Blocked` in OmniFocus and should be ignored entirely).

8. **Append** the Today's Focus section (see format below).

## Output format notes

- **Week's focus**: Pull the first line of the `## Focus This Week` section from the weekly note and include it as a single italic line at the top of Today's Focus, e.g. `*Week's focus: ...*`
- **Mentoring links**: When a mentoring session appears in the hard landscape, use the wikilink to the mentoring note, e.g. `[[Mentoring/Zac Story]]`, `[[Mentoring/Jeremy]]`, `[[Mentoring/Tyler]]`
- **Brainstorm Today**: The `### Brainstorm Today` section is written directly into the daily note body (not inside Today's Focus). It contains two blockquotes — the OmniFocus item labeled **OF:**, then the AI-generated item labeled **AI:** (just the AI item if the tag is empty). Keep each to one sentence or a focused question — these are background prompts, not tasks.

## Output format

```markdown
## Today's Focus

**Today's hard landscape:**
- HH:MM — [Event or commitment with a specific time]
- (All day) — [All-day calendar event worth noting]

**Get done today:**
- [ ] [Prioritized list — OmniFocus overdue ⚠️ and due-today first, then flagged/planned-today, then weekly note items and yesterday's follow-ups, then untimed rhythms]

*Stretch (if time allows):*
- [ ] [1–2 context-appropriate tasks from OmniFocus backlog — omit this block entirely if nothing fits or the day is already packed]

**On the radar this week:**
- [Day Mon DD] — [Event or commitment] *(time if applicable)*

*[Daily habits tagline — one line, always at the bottom: e.g. "Rhythms: Bible → exercise → family time → read before bed"]*
```

### Section guidance

- **Today's hard landscape**: The fixed shape of the day (GTD concept). Include only items with a specific time — calendar events and Rhythms.md commitments that have a scheduled time. If a time-based commitment has a known exception (e.g. Youth Group cancelled for spring break), note it inline. Do NOT include untimed items like "work on-site" — those are tasks, not landscape. If a meal from the weekly cooking schedule has a time context (e.g. Thursday Lunch), include it here; otherwise put it in "Get done today." If nothing is scheduled, omit this section.
- **Get done today**: Everything that needs doing but isn't pinned to a time. Order: (1) OmniFocus overdue items ⚠️, (2) OmniFocus due today, (3) flagged/planned-today OF items, (4) weekly note action items and yesterday's unfinished follow-ups, (5) untimed rhythms (work on-site, cook for family, etc.). Keep this list tight — 3–5 items max, not counting stretch goals. Mark overdue items ⚠️ with the due date, once, without commentary — never count days slipped, missed attempts, or "windows" that went unused; patterns belong in weekly planning, not the morning brief. Never surface OmniFocus tasks with a future defer date. Do not duplicate items already shown from the weekly note.
- **On the radar**: Only actionable or personally significant items for the rest of the week. One line each. Skip purely informational entries. For any day that is going on-site but is NOT a standard on-site day (standard = Monday and Wednesday), include the reason in parentheses, e.g. *"On-site (April Chapel 15:00)"*. No reason needed for Monday/Wednesday since those are always on-site.
- **Daily habits tagline**: Always one line at the bottom. Never a section — just a quiet reminder of the daily rhythm. Use the Daily Rhythms from Rhythms.md.

## Standard daily note template

```markdown
### What Happened Today?

### Brainstorm Today

> **OF:** [Thinking / Brainstorming task from OmniFocus — one sentence or question to hold in the back of your mind]

> **AI:** [Assistant-generated question — what Jon's recent notes, plans, and threads suggest he needs to be thinking about; never a reworded OmniFocus item]

### What Stood Out?

### What Needs Follow Up?

### Random Thoughts

## Today's Reading

See also: [[Prayer/War-Room]]
```

## Rules

- **Append only** — never insert into or overwrite existing content.
- Read the file before writing to confirm where it ends.
- Use `[[Person/Name]]` wikilink syntax when referencing people.
- Keep the tone direct and personal — this is for Jon, not a report.
- **Briefing voice, not supervisor voice.** Describe the shape of the day; don't issue orders or apply pressure ("no excuses", "no better day than today"). State deadlines plainly and trust Jon with them.
- **After missed days, never enumerate or count what was missed.** The brief always starts from today.
