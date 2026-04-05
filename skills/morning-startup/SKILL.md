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

6. **Check calendars** (timezone: America/Denver).

   **Google Calendar** — Pull from all three: **Jon's Calendar** (`jonzenor@gmail.com`), **Family** (`family18039985066750084653@group.calendar.google.com`), and **Danae's calendar** (`danaezenor@gmail.com`).
   - Fetch **all events today** from all three — include every one of them.
   - Fetch events for the **rest of the week** from all three — filter to only items that are actionable or personally significant. Skip purely informational entries.

   **Apple Calendar / Exchange (FOTF work meetings)** — Use the `mcp-ical` tool to fetch from the **"Calendar"** calendar (this is the Exchange calendar synced from FOTF).
   - Fetch **all events today** from the "Calendar" calendar — include every timed work meeting in the hard landscape.
   - Fetch events for the **rest of the week** from the "Calendar" calendar — include in "On the radar" any meetings that are notable (new one-time events, conflicts, unusual schedule, etc.). Recurring standup noise can be omitted from the radar unless there's something worth flagging.
   - Convert event times from UTC to America/Denver (MDT = UTC−6 in summer, MST = UTC−7 in winter).
   - **F1 and sports events** (typically on the Family calendar): low priority — summarize briefly or group them ("F1 race weekend: Sat–Sun") rather than listing each one. No need to attribute who added them.
   - **Recurring logistical reminders** (e.g. Trash Day): only surface on the day itself or the evening before — never earlier in the week radar.
   - **Danae's appointments**: flag any in-person (non-virtual) doctor or medical appointments — these require Jon to work from home and drive her. Treat these as high-priority items. Surface them clearly in "Today's hard landscape" if today, or prominently in "On the radar" if later this week, with a note: *"In-person — work from home, drive Danae."*
   - **Schedule conflicts**: if two calendar events or commitments overlap in time, flag the conflict inline in the hard landscape with ⚠️ and a note.

7. **Query OmniFocus for the Brainstorm item** — query for `Available` tasks tagged `Thinking / Brainstorming`. Pick **one** item that is most timely or resonant given the day's context (e.g. prefer items with an upcoming due date, or that connect to something already surfaced from the weekly note or daily note). This item goes into the `### Brainstorm Today` section of the daily note (see template and format below) — not in "Get done today." The intent is a low-pressure prompt for background thinking during downtime, not a hard task.

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
- **Brainstorm Today**: The `### Brainstorm Today` section is written directly into the daily note body (not inside Today's Focus). It contains a single blockquote with the chosen Thinking / Brainstorming task. Keep it to one sentence or a focused question — this is a background prompt, not a task.

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
- **Get done today**: Everything that needs doing but isn't pinned to a time. Order: (1) OmniFocus overdue items ⚠️, (2) OmniFocus due today, (3) flagged/planned-today OF items, (4) weekly note action items and yesterday's unfinished follow-ups, (5) untimed rhythms (work on-site, cook for family, etc.). If a recurring item keeps sliding, surface it with a brief nudge. Keep this list tight — 4–6 items max, not counting stretch goals. Never surface OmniFocus tasks with a future defer date. Do not duplicate items already shown from the weekly note.
- **On the radar**: Only actionable or personally significant items for the rest of the week. One line each. Skip purely informational entries. For any day that is going on-site but is NOT a standard on-site day (standard = Monday and Wednesday), include the reason in parentheses, e.g. *"On-site (April Chapel 15:00)"*. No reason needed for Monday/Wednesday since those are always on-site.
- **Daily habits tagline**: Always one line at the bottom. Never a section — just a quiet reminder of the daily rhythm. Use the Daily Rhythms from Rhythms.md.

## Standard daily note template

```markdown
### What Happened Today?

### Brainstorm Today

> [Thinking / Brainstorming item — one sentence or question to hold in the back of your mind]

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
