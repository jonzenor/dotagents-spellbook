---
name: evening-summary
description: Evening summary routine for Jon's Obsidian life vault. Reads today's note, performs Keeper/log work, and captures every executable next action in OmniFocus using GTD rules and a durable failure queue. Use when the user says "evening summary", "summarize my day", "end of day", or asks for a daily summary.
---

# Evening Summary

## What this does

Appends an `## AI Summary` section to today's daily note. **Never overwrites or inserts — append only.**

## Steps

1. **Determine the target date** from the `currentDate` context variable and the time of day.
   - **If invoked before 14:00 (2:00 PM) local time and no explicit date was specified in the arguments**, assume this is a catch-up summary for *yesterday* — use `currentDate - 1 day` as the target date. Proceed silently without commenting on the timing.
   - **If invoked at 14:00 or later**, use `currentDate` (today) as the target date.
   - **If the user explicitly names a date or says "for yesterday"**, always use what they specified regardless of time.
   - All subsequent steps refer to "today's note" and "today" as the *target date*, not necessarily the calendar date.

2. **Read the target daily note** in full. Check `Daily/YYYY-MM-DD.md` first; for an explicitly requested older date, fall back to `Daily/YYYY/MM/YYYY-MM-DD.md`.

2.5. **Repair unprocessed AI-owned state after a gap.** Before processing the target note, look backward up to 14 calendar days from the target date or to the most recent earlier daily note containing `## AI Summary`, whichever comes first. Any intervening daily note without an AI Summary is a catch-up candidate. Never start an unbounded historical sweep; older notes require an explicitly requested catch-up.
   - Process candidates oldest first so Scripture scheduling, Keeper instructions, Almanac additions, OmniFocus captures, durable logs, and explicit project lifecycle changes remain chronological.
   - Run only the mechanical and durable-routing work from Steps 7–20. Do not append retroactive AI Summaries, reconstruct the day from calendar data, add routine weekly reflections, or comment on the gap.
   - Read every destination before writing and use the source daily-note link as an idempotency key. If an equivalent entry from that source already exists, treat that part as processed rather than duplicating it. OmniFocus capture retains its semantic duplicate check.
   - Preserve ambiguity. Queue failed OmniFocus writes normally and defer any decision that cannot be supported by the source note.
   - This is resilience, not backlog processing: never report missed-day counts, adherence, or debt. Mention catch-up only when a write failed or Jon must answer a genuine ambiguity.

3. **Read the last 3–5 daily notes** for context on ongoing threads. If recent notes are sparse or empty, work with what's there — never comment on the gap or count missed days. Cross-day trend observations belong in weekly planning, not here.

4. **Read this week's weekly note** (`Weekly/YYYY-WXX.md`, weeks start Sunday) in full — both for broader context and to identify what needs updating.

5. **Update the current weekly note only when today changes the week's record:**
   - The current schema is `Weekly Focus`, `Quarterly Objectives`, `Goals`, `Prayer Focus`, `Meal Plan`, `On the Radar`, and `Notes & Reflections`. Do not create obsolete sections such as `Active Action Items`, `Mentee Check-ins This Week`, or `Cooking This Week` in a current note.
   - Check off a current weekly `Goals` item only when today's note explicitly confirms that exact goal outcome is complete.
   - Append a dated `Notes & Reflections` entry only when today changed the week's capacity, direction, a relationship, a project, or the interpretation of the week's arc. Do not compress routine days into weekly prose merely because evening summary ran.
   - Historical weekly notes may retain older schemas. Update an older section only when it already exists and the target daily note explicitly supports the change.

6. **Follow wikilinks in today's note** — identify any `[[File]]` or `[[Folder/File]]` links mentioned in the daily note. Read those linked files and look for content added today (dated entries, new rows in tables, new meeting log entries, recently added items). Summarize what was added in the **Don't lose this** section if it's time-sensitive or easily forgotten. Pay particular attention to Mentoring notes (meeting logs), Prayer lists, and any file with dated entries.

7. **Medical logs** — scan today's daily note for any health- or medical-related mentions (symptoms, appointments, medications, diagnoses, test results, injuries, or anything health-adjacent).
   - Jon's health → append to `Reference/Logs/Medical Log.md` under `## Log` (newest at top).
   - Danae's health → append to `Reference/Logs/Danae's Med Log.md` under `## Log` (newest at top).
   - Format: `**[[Daily/YYYY-MM-DD|YYYY-MM-DD]]** — [brief factual summary of what was mentioned]`.
   - If nothing health-related came up for a given person, skip their log entirely.

8. **Answered prayers** — scan today's daily note for any mention of a prayer being answered or God responding to a specific ask. If found, prepend a dated entry to `Prayer/Answered Prayers.md` under `## Log` (newest at top). Format: `**[[Daily/YYYY-MM-DD|YYYY-MM-DD]]** — [what was prayed for, what happened, any context that makes it meaningful]`. Only log things explicitly described as answered — don't infer.

9. **Cooking log** — scan today's daily note for any mention of actually cooking a meal: what was made, how it turned out, what worked, what didn't, substitutions, timing notes. If found, prepend a dated entry to `Reference/Logs/Cooking Log.md` under the `## Log` section (newest at top). Format: `**[[Daily/YYYY-MM-DD|YYYY-MM-DD]]** — [meal name, result, any notes for next time]`. Only log meals that were actually cooked today — skip plans, intentions, carries, or mentions that cooking didn't happen.

10. **Book log** — scan today's daily note for any mention of reading, starting, finishing, or making notes about a book. If found, prepend an entry to `Reference/Logs/Book Log.md` under `## Log` (newest at top). Check whether a note exists at `Reference/Books/[Title].md` — if it does, use a wikilink. Format: `**[[Daily/YYYY-MM-DD|YYYY-MM-DD]]** — [[Reference/Books/Title|Title]] — [Started / Finished / Made a note / etc.]`. Only log if a book is explicitly mentioned — skip plans or intentions to read.

11. **Spiritual log** — scan today's daily note for anything spiritually significant: meaningful convictions, spiritual milestones, commitments made or broken, notable insights from prayer or scripture, significant moments of spiritual growth or struggle. If found, prepend an entry to `Reference/Logs/Spiritual Log.md` under `## Log` (newest at top). Format: `**[[Daily/YYYY-MM-DD|YYYY-MM-DD]]** — [what happened, why it matters spiritually]`. Only log things of genuine significance — not routine Bible reading or prayer (those belong in the Book Log or are implied by the daily rhythm). Skip if nothing spiritually notable occurred.

12. **Lighthouse log** — scan today's daily note for *significant* Lighthouse moments: direction shifts, staffing changes (join/leave/role change), key conversations with officers or mentors, spiritual discernment about the ministry, external connections (other ministries, collaborators, advisors), or reframes of the ministry's trajectory. If found, prepend an entry to `Reference/Logs/Lighthouse Log.md` under `## Log` (newest at top). Format: `**[[Daily/YYYY-MM-DD|YYYY-MM-DD]]** — [what happened, why it matters, link to relevant people/notes]`. Use wikilinks for people (`[[Organizations/Lighthouse/People/OnlineName]]` or `[[Mentoring/Name]]`) and related notes. **Only log significant moments — skip routine status updates, everyday server activity, or passing mentions.** If nothing significant surfaced, skip it.

13. **Scripture memory deck** — this skill is the only writer of `Bible/Scripture Memory.md`. Read it, then read whatever Jon wrote under `## How did scripture memory go?` and anywhere else in the note that reports a recitation.

   **The deck is the full definition of this system.** Its `Rules` section owns every mechanic — the verse ladder, the three-reps-to-a-rung minimum, blocks, chapter graduation, the Review ladder and its 180-day floor, relearning, intake, and the caps. **Read those rules and apply them. Do not reproduce them here**; a second copy is how this skill went stale before. If this skill and the deck ever disagree, **the deck wins and this step gets corrected.**

   **Reading Jon's marks:**
   - `- [x] Galatians 1:5 — 1× — struggled, need more practice`
   - **Never modify Jon's checkmarks or his notes.** Read them; leave the line exactly as he wrote it.
   - **The appended note is the grade.** Accept plain language ("nailed it", "totally blanked on verse 4"); never make him use a keyword.

   **Two grades, and neither is a punishment:**
   - **`clean`** — checked box with no note, or a positive one → counts toward the three reps that rung needs
   - **`struggled`** — checked box with any sign of difficulty (struggled, blanked, couldn't get it, peeked, stumbled) → holds the rung, does not count toward advancement, and **does not take away reps already banked**
   - **Unchecked box** → *not attempted*. **Nothing happens at all** — not a miss, not a step back, not a reset, not a broken streak. Reschedule, change nothing, log nothing, and never mention it in the AI Summary.

   **Demotion takes a pattern, never one bad day.** A unit steps back one rung only after **two `struggled` reps in a row**. A `clean` clears the streak; a missed day is inert and neither counts toward it nor clears it. Blocks step back whole. **Relearning is exempt from demotion entirely** — cold material is expected to be rough.

   **The Perfected checkbox.** A verse leaves the Daily rung only when Jon checks the nested `- [ ] *Perfected — promote*` box beneath its line. When he does, advance it that evening. His check wins even if the line also carries a note. **Never count or remark on how long the box has gone unchecked.**

   **Silent by design.** Never show the allowance, deficit, tier, word counts, or any adherence figure — in the daily note, the AI Summary, or conversation. Never describe intake as behind, never count days it has been closed, never tie it to a lapse. Surfacing the dial would turn the practice into a metric.

   **Never surface the Backlog.** Those are passages Jon set down; they are not unfinished business and move only when he says so. When one does activate, establish how much survives before planning — do not assume a restart from zero.

   Count words from the ESV text when a chapter first comes up and store them in the deck's `Reference data`. Update the deck's `State` tables in place. Only write to `Reference/Logs/Spiritual Log.md` for a genuine milestone — a chapter graduating to Review, or a cold chapter relearned — never for a daily rep.

14. **Keeper instructions** — scan today's daily note for any line containing `Keeper,` or `Keeper:` that does **not** already end with `📚✅`.

   For each unprocessed Keeper instruction:
   - Read the surrounding context (the instruction may reference content written several paragraphs earlier)
   - Interpret the instruction with judgment — common actions include: creating or appending to a project or topic note, updating a person file, adding an entry to `Almanac.md` for a future date or meeting, generating reflection questions to append to tomorrow's note, or ensuring an idea gets filed in the right vault location
   - Execute the action fully — don't just summarize or note it
   - After completing the action, append `📚✅` to the end of that Keeper line in today's daily note
   - Note what was done in the **Logs updated today** section of the AI Summary
   - If the instruction explicitly recurs by event, context, or cadence, add it to `Keeper Instructions.md` with its trigger, action, and source daily note. Do not use the Almanac for recurring instructions.

   Skip any Keeper line already ending with `📚✅` — it has been handled.

15. **Almanac entries** — scan today's full narrative (not just explicit Keeper lines) for anything tied to a future date or a specific future meeting/event — e.g. "bring this up at next week's Dev Chapter meeting," "remind me about X on the 15th," "need to follow up on Y next month." This includes both Keeper instructions handled in Step 14 that ask for something to be surfaced later, and plain narrative that states a clear future-surfacing intent without invoking Keeper at all.

   For each one found:
   - Determine the target date. If a specific date is given, use it. If it's tied to a recurring meeting/event with no fixed date yet, use the date of its next known occurrence and name the event in the entry text.
   - Read `Almanac.md` at vault root (create it with a one-line header if it doesn't exist yet).
   - Add the entry as a bullet under the matching `## YYYY-MM-DD` header, creating the header in the correct date-ascending position if it doesn't exist. Tag it with `*(added YYYY-MM-DD)*` using today's date.
   - For a birthday, anniversary, or other explicitly annual reminder, tag it `*(annual; added YYYY-MM-DD)*`. Annual entries renew for the following year when morning-startup consumes them. Do not tag an event annual merely because it could recur; require explicit annual meaning.
   - Note the addition in the **Logs updated today** section.

   Only add entries with a genuine future-surfacing intent — not every mention of a date. This skill only ever *adds* to Almanac.md; entries are removed by morning-startup once they've surfaced.

16. **Person file updates** — scan today's note for people worth logging. For each qualifying person, prepend a dated entry under `## Log` in their `Person/Name.md` file (creating the file if it doesn't exist).

   **Who qualifies:** People with a real relationship to Jon — family, friends, mentees, close colleagues. Ask: would future-Jon want this on record when interacting with this person again? Skip FOTF coworkers mentioned only in a routine work context. Skip Danae (covered by her Med Log and existing notes). For mentees with a Mentoring note, use the Mentoring note for session content — but still use Person/ for significant life events outside of sessions.

   **Two tiers:**

   *Brief note* — a single dated bullet. Use when something happened involving the person that's worth knowing later but doesn't need context or texture. Examples: attended an event together, a health update, a quick observation.
   Format: `**[[Daily/YYYY-MM-DD|YYYY-MM-DD]]** — [one sentence]`

   *Deeper entry* — a dated paragraph. Use when: Jon had a strong emotional reaction, a significant relational development surfaced, a pattern emerged, or the entry needs context and texture to be meaningful later. Examples: a difficult conversation, a revealed family dynamic, a moment of concern or pride.
   Format: `**[[Daily/YYYY-MM-DD|YYYY-MM-DD]]** — [paragraph with context, Jon's reaction, relevant detail]`

   **File handling:**
   - If `Person/Name.md` exists: read it first, then prepend the entry under `## Log` (add the section if it's missing).
   - If it doesn't exist: create the file with a minimal template, then add the entry:
     ```markdown
     # [Name]

     *[Relationship to Jon — e.g. "Jon's mother", "family friend"]*

     ## Log

     <!-- Dated entries — events, health, relational notes. Newest at top. -->
     ```
   - Never duplicate content already captured in a Mentoring note for today.

17. **Invoke `gtd-omnifocus` in `capture` mode automatically** — supply the target daily note, its source path/date, explicit Keeper actions, relevant project names, and **every unchecked item under `## Quick List`**. Do not ask Jon to run the GTD skill separately.
   - Quick List items need no qualifying language. Jon wrote them there because he intended to do them today, so an unchecked box *is* the commitment — pass them to capture mode as commitments rather than testing them for commitment phrasing.
   - Do not pass a checked item, or one Jon annotated as dropped or no longer needed.
   - Leave the list itself exactly as Jon wrote it. Capturing an item does not check it off, strike it, annotate it, or delete it, and the list is never carried into tomorrow's note.
   - Let the GTD skill own commitment tests, project routing, duplicate checks, action wording, tags, dates, flags, waiting/someday states, verification, and the failure queue.
   - Treat OmniFocus as best-effort: if capture mode reports an outage after its retry, continue the evening summary and report the safely queued writes.
   - Report each verified action and destination under **OmniFocus captured** and each queued write under **OmniFocus queued**.

18. **Scan for new ideas** — look through the day's full narrative and passing mentions for uncommitted possibilities (notes before 2026-08-24 may also have a `## Random Thoughts` section). Substantial project-shaped ideas belong in `Ideas/`, never `Projects/` unless Jon explicitly committed to implementation. Reusable learning belongs in `Topics/`. Update the relevant collection index when creating a durable note.

19. **Theater movie candidates** — scan for movies Jon explicitly says he may want to see **in a theater**. This is not a general watchlist.
   - Read or create `Reference/Theater Movie Candidates.md` and keep it linked from `Reference/Reference.md`.
   - Before adding or substantially updating a candidate, browse for missing decision-support information: verified theatrical release date (or the narrowest supported window), current MPAA rating or `Not yet rated`, main star or stars, and a brief spoiler-free description. Prefer official studio/distributor pages and authoritative rating sources; preserve descriptive source links.
   - Record Jon's stated reason for interest from the daily note. Do not invent a reason from genre, cast, or Keeper synthesis.
   - Use these fields: title, theatrical release date, rating, main stars, brief description, why Jon is interested, status, and sources. Default status is `Candidate`; weekly-planning may change it to `Soft plan (YYYY-MM)`, `Saw`, or `Passed`.
   - If a title or date is uncertain, preserve that uncertainty and the search result rather than guessing. Do not add every movie Jon mentions—require explicit theatrical interest.

20. **Review committed project lifecycle state** — read `Projects/Projects.md`, then open every project listed there.
   - Attention states are `Backlog`, `This Quarter`, and `In Progress`, with optional `Paused until YYYY-MM-DD`. Never silently demote a project. If it appears stalled, ask whether it is paused, waiting, or needs a smaller next action.
   - Compare each active project's stated outcome and status with explicit evidence in today's daily note, the recent daily notes read in Step 3, and this week's weekly note.
   - If the evidence explicitly confirms that the project's outcome was achieved, close it during this run: set `**Status:** Complete`, add `**Completed:** [[Daily/YYYY-MM-DD|YYYY-MM-DD]]` using the supporting daily note, move it to `Archive/Projects/`, repair wikilinks throughout the vault, and synchronize `Projects/Projects.md`.
   - If Jon explicitly says the project was abandoned, follow the same lifecycle using `**Status:** Dropped` and record the decision source/date.
   - For every explicit close, drop, pause, resume, or rename handled here, invoke `gtd-omnifocus` in `lifecycle` mode automatically so the matching OmniFocus project remains synchronized.
   - If completion or abandonment seems likely but is not explicit, do not change project status. Ask Jon whether the project should be closed, and identify the evidence that made its state uncertain.
   - Never treat inactivity, completed individual tasks, or a past event date as proof that the project outcome is complete.
   - When Jon explicitly changes a project's outcome, scope, governing constraint, approach, attention state, pause/resume state, name, completion, or abandonment, record a concise dated entry under `## Decisions & History` in that project note. Use `**[[Daily/YYYY-MM-DD|YYYY-MM-DD]]** — [decision and why it changes pursuit of the outcome]` when sourced from a daily note.
   - Do not record routine progress, completed actions, status chatter, or AI recommendations as decisions. Do not create an action checklist in the project note.
   - Include every project file, index, or repaired-link location actually written in **Logs updated today**.

21. **Append** the AI Summary section to today's daily note (see format below).

## Output format

```markdown
## AI Summary

**What happened:** [1–3 sentence narrative of the day — events, tone, what mattered]

**Lessons learned:**
- [Specific, actionable insight from something that went wrong or went well]
- [Only include if there's a real lesson — skip filler]

**Don't lose this:**
- [Things that could fall through the cracks: upcoming deadlines, important decisions pending, time-sensitive relationships]

**Tomorrow's context:**
- [Non-task context that will help tomorrow's orientation: capacity, a pending decision, or why a particular OmniFocus perspective may matter]

**New ideas worth capturing:**
- [Idea from today's narrative — with a suggested vault home if it belongs somewhere: Topics/, project note, Mentoring, etc.]
- [Omit this section entirely if nothing new surfaced]

**Logs updated today:**
- [[Log/File/Path]] *(one line per file actually written to — omit the weekly note unless something notable was checked off or added; omit entirely if no logs were updated)*

**OmniFocus captured:**
- [One line per action actually added — omit entirely if none]

**OmniFocus queued:**
- [One line per failed intended write safely added to the failure queue — omit entirely if none]
```

### Section guidance

- **What happened**: Write it like a friend summarizing the day — honest, human, not clinical. Acknowledge hard days as hard.
- **Lessons learned**: Only include genuine insights. A lesson should change future behavior. Skip platitudes.
- **Don't lose this**: The most important section. Capture things that are easy to forget but costly if forgotten — upcoming events, pending decisions, relationship moments, financial deadlines. Also flag anything from today that belongs in a deeper note (Mentoring, Person, Prayer, etc.).
- **Tomorrow's context**: Actions belong in OmniFocus and are reported once under **OmniFocus captured**. Use this optional section only for non-task continuity that tomorrow's briefing should know: unusual capacity, an unresolved decision, or why a context/perspective may matter. Never reproduce captured actions or create a second execution list. If a genuine multi-day pattern matters, it gets raised during weekly planning, framed as a question.

## Rules

- **Proportionality — the most important rule for this skill.** The AI Summary is a response to Jon's journaling, not a replacement for it. Aim for a summary no longer than what Jon wrote today. A thin note (a line or two) gets 1–3 sentences and the log scans, nothing more. If the note is essentially empty, append at most two lines — anything genuinely time-sensitive found in linked notes, or simply a quiet placeholder — and skip every optional section. Never reconstruct the day from calendar data Jon didn't write about.
- **Daily note: append only** — never insert into or overwrite existing content in the daily note.
- **Weekly note: significance-filtered edits only** — use the current weekly schema, require explicit evidence for completed goals, and append to Notes & Reflections only when the day changes the week's arc. Never rewrite or delete existing weekly note content.
- **Medical Logs: prepend entries under `## Log`** — Jon's health → `Reference/Logs/Medical Log.md`; Danae's health → `Reference/Logs/Danae's Med Log.md`. Read each file first. Only write if there is actual health content for that person.
- **Answered Prayers: prepend entries under `## Log`** — `Prayer/Answered Prayers.md`. Read the file first. Only write if a prayer is explicitly described as answered in today's note.
- **Cooking Log: prepend entries under `## Log`** — add newest entry at the top of the log section (`Reference/Logs/Cooking Log.md`). Read the file first. Only write if cooking was mentioned in today's note.
- **Book Log: prepend entries under `## Log`** — add newest entry at the top (`Reference/Logs/Book Log.md`). Read the file first. Check for an existing note at `Reference/Books/[Title].md` and use a wikilink if it exists. Only write if a book is explicitly mentioned as read, started, or finished today.
- **Lighthouse Log: prepend entries under `## Log`** — add newest entry at the top (`Reference/Logs/Lighthouse Log.md`). Read the file first. Only write for *significant* Lighthouse events — direction shifts, staffing changes, key conversations, spiritual discernment, external connections, or reframes. Skip routine mentions, everyday server activity, or passing references. If nothing significant surfaced, skip it entirely.
- **Person files: prepend entries under `## Log`** — `Person/Name.md`. Read the file first if it exists; create with a minimal template if not. Two tiers: brief (single sentence) for events and passing notes, deeper (paragraph) for significant relational developments, strong reactions, or anything that needs context to be useful later. Skip people with no real relationship to Jon, skip Danae (her own logs), skip FOTF coworkers in routine work contexts. Never duplicate content already written to a Mentoring note for today.
- **Person versus Mentoring** — mentoring-session content belongs in the Mentoring note. Person notes receive only identity, relationship facts, and significant life events outside the session; use a brief linked pointer instead of duplicating session details.
- **Keeper instructions: act and mark complete** — scan for `Keeper,` or `Keeper:` lines not ending with `📚✅`. Execute each instruction with full context, promote explicit recurring behavior to `Keeper Instructions.md`, then append `📚✅` to the line. Log what was done in **Logs updated today**. Skip already-marked lines.
- **Scripture Memory deck: this skill is its only writer** — `Bible/Scripture Memory.md`. **The deck's `Rules` section is authoritative for every mechanic; read it rather than working from memory.** In the daily note, read Jon's checkboxes and appended notes but **never alter them** — those marks are his words. An unchecked box means *not attempted* and changes nothing at all. There is no overdue state, no backlog, and no streak. Enforce the deck's caps — one passage in New, one chapter in Relearning, one Review chapter a day — so the load never grows past what Jon can carry; that enforcement is Keeper's job, not Jon's.
- **Almanac.md: add only, under the matching date header** — `Almanac.md` at vault root. Read the file first (create it if missing). Only add entries with a clear future-surfacing intent — a specific date or a named future meeting/event. Never delete entries here — deletion happens in morning-startup, at the moment an entry actually surfaces.
- **Theater candidates are theater-specific** — `Reference/Theater Movie Candidates.md` supports choosing one or two theater outings each month. Do not turn it into a general streaming, rental, or someday watchlist. Preserve Jon's stated reason for interest and cited release information.
- **Ideas are not projects** — create project-shaped possibilities under `Ideas/`. Move one to `Projects/` only after Jon explicitly commits to implementing it.
- **Provenance** — every durable AI-created dated entry links to the supporting daily note. Label cross-note inference as Keeper synthesis; never promote uncertainty into fact.
- **Linked files: read for context** — edit only the destinations explicitly authorized by this workflow (weekly note, logs, Person/Mentoring notes, Almanac, Keeper Instructions, Ideas/Topics/Projects when a Keeper instruction requires it).
- Read each file before writing to confirm current state.
- Use `[[Person/Name]]` wikilink syntax when referencing people.
- Keep the tone direct and personal — this is a private journal, not a report.
- If a section has nothing worth saying (e.g. no real lessons learned today), omit it rather than padding with filler.
- Do not summarize things already captured well in the note — add insight, not repetition.
- Before writing to another layer, require new retrieval value: relationship context for Person, session context for Mentoring, a significant domain fact for a log, a decision or milestone for a project, or reusable knowledge for a Topic.

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
