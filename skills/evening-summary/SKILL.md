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

   **Read the checklist, not just prose.** Morning startup writes a grouped checklist of today's reps. Jon checks boxes and may append a note to a line:
   - `- [x] Galatians 1:5 — 1× — struggled, need more practice`
   - A **checked box** means the rep happened. An **unchecked box** at day's end means *not attempted*.
   - **The appended note is the grade.** "Struggled" or "need more practice" → `shaky`. "Blanked" or "couldn't get it" → `blank`. A checked box with **no note** → `clean`. Jon never has to use a keyword.
   - **Never modify Jon's checkmarks or his notes.** Read them; leave the line exactly as he wrote it.

   **Grade vocabulary** — map whatever Jon wrote to one of four outcomes. Accept plain language ("nailed it", "totally blanked on verse 4"); never make him use a keyword.
   - `clean` — recited without help → advance one rung on the interval ladder
   - `shaky` — got through it with effort, a peek, or a stumble → hold at the current rung
   - `blank` — could not recall it → drop two rungs, minimum 1 day
   - *nothing written* — treat as **not attempted**. Reschedule to tomorrow, change nothing else, and log nothing. This is not a miss, not a lapse, and not a broken streak. Never mention it in the AI Summary.

   **The atomic unit is the verse, not the passage.** Each new verse walks its own ladder independently and matures before it is ever combined. Units merge upward — verse → paragraph → section → chapter → book — and **a merge is permanent; merged units never decompose back into their parts.** A merged unit graded `blank` steps back a rung but stays merged.

   **Verse ladder** — Keeper states the rep count so Jon never computes it:
   - **New** — daily, days 1–5, recited 25× → 20× → 15× → 10× → 5×
   - **Imprint** — daily, days 6–10, recited 1×
   - **Settle** — every other day for about a week, 1×
   - **Hold** — every 3 days, 1×, until its paragraph-mates catch up

   A verse reaching Hold is merge-eligible and gets no further individual promotion.

   **Merge rules:**
   - **Every** verse in a paragraph at Hold → merge into the paragraph; the individual verse rows retire permanently. All of them or none — one straggler holds the paragraph, and that is correct.
   - Two adjacent paragraphs both stable at 30 days → the combined section
   - Every paragraph of a chapter merged and stable at 30 days → the chapter
   - Every chapter of a book merged and stable at 60 days → the book (rotating reps if over ten minutes)
   - **Merged-unit ladder:** 7 → 14 → 30 → 60 → 90 days. At 90 with a clean rep → Maintenance.

   **Intake is measured in words, not verses** — words are what cost time. Twenty-five reps of a 7-word verse is forty seconds; of a 35-word verse, four minutes. A word allowance keeps the daily ask roughly constant.
   - **Tier 1: 22 words/day** (the ESV average for Galatians, so ≈1 verse/day) · **Tier 2: 40** · **Tier 3: 60 (cap)**
   - **A verse is never split across days.** It enters whole or not at all.
   - The allowance grows by the tier rate each day. **Unused surplus carries, capped at one day's rate.** At most **3 verses** may enter in a day regardless of how short they are.
   - **The next verse always enters even if it overdraws.** The deficit carries and silently suppresses intake until repaid — Jon never gets a day with nothing new merely because the next verse is long.
   - **A verse longer than 1.5× the daily allowance repeats its first ladder rung** (25× again) for ⌈words ÷ allowance⌉ days. That is how a long verse gets extra time: more days of intensive work, never a split and never a deferral.
   - Count words from the ESV text when a chapter first comes up; store the counts in the deck. **Never show the allowance, deficit, tier, or word counts to Jon** — he sees verses and rep counts only.

   **Tier promotion.** Jon wants to push Galatians toward finishing sooner, so the tier is a dial — but one that only turns up when the practice is demonstrably holding. Start at tier 1. After **14 consecutive days with at least 80% of reps checked off**, raise to tier 2; after a further 14 days holding, to tier 3 (cap). **Any quiet stretch of 4+ days drops the tier one step on return**, to be re-earned — that is a return to a load that was working, not a punishment. Jon may override in either direction and his call wins. **Never ask him to pick a tier, never state the current tier in the daily note, and never report the adherence percentage back to him.** The dial governs silently; surfacing it would turn the practice into a metric.

   **Intake — the valve Keeper controls.** Intake is whether new verses are added today. Keeper opens and closes it; never ask Jon to decide. Close intake when:
   1. A chapter just became whole — closed 5 days, for merge work and first full-chapter recitations
   2. In-flight material (introduced but not yet merged) exceeds **28 days' worth of the current allowance** — closed until it drains below **22 days' worth**. Scale this with the tier rather than using a fixed word count: steady-state in-flight naturally sits near 17–20 days' worth, so a flat cap tuned to tier 1 would jam intake shut at tier 2.
   3. **Jon has been quiet 4 or more days — closed on return, reopening after 3 days of reps.** He comes back to what he already knows, never to a pile of new material.
   4. Jon says so, or a day's build would exceed ~15 minutes

   **Closed intake is a normal operating state, not a failure.** It pauses growth, never the practice — everything in flight continues on its own cadence. Never describe closed intake as being behind, never count the days it has been closed, and never tie it to a lapse. This is the mechanism that makes a week away survivable, so do not undermine it with commentary.

   **Reactivating** — a previously memorized passage gone cold. Rebuilds **one paragraph at a time**, stacking; never open a cold passage at full chapter or book length. Daily for the first few days on each paragraph, then move to the next and add them together. One `clean` advances (new learning needs the full ladder), the text may be open on the first pass at any new paragraph, and `shaky` carries **no penalty**. When the whole unit is recited clean and cold, it joins Review at the **14-day rung**. When Jon does not say whether the text was open, assume it was.

   **Caps — Keeper enforces these, not Jon's willpower:**
   - **Learning: one passage, hard cap.** If Jon asks to start another while one is in Learning, say no and explain what is currently in flight. Add the request to `## Backlog` instead.
   - **Review: five entries, soft cap.** Reactivating entries count toward it. Individual in-flight verses do not — they are governed by the intake valve instead. At six or more, do not add anything new; note it once at weekly planning as a question about pace, not a warning.
   - **Backlog is inert.** Entries there are partially memorized passages Jon set down. Never surface them, count them, ask about them, or treat them as unfinished business — they move only when Jon says so. When one does activate, establish how much survives before planning segments; do not assume a restart from zero.
   - **Respect scheduled start dates.** A Reactivating entry with a future `Starts` date is not due and is not late. It simply does not exist yet as far as the daily note is concerned.

   Update the deck tables in place. Only write to `Reference/Logs/Spiritual Log.md` for a genuine milestone — a chapter fully learned or retired to maintenance — never for a daily rep.

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

17. **Invoke `gtd-omnifocus` in `capture` mode automatically** — supply the target daily note, its source path/date, explicit Keeper actions, and relevant project names. Do not ask Jon to run the GTD skill separately.
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
- **Scripture Memory deck: this skill is its only writer** — `Bible/Scripture Memory.md`. Read it first. In the daily note, read Jon's checkboxes and appended notes but **never alter them** — those marks are his words. Grade from what Jon actually wrote; a missing grade means *not attempted*, which reschedules silently and is never logged, counted, or mentioned. There is no overdue state, no backlog, and no streak. Enforce the Learning cap of one and the Review soft cap of five so the load never grows past what Jon can carry — that enforcement is Keeper's job, not Jon's.
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
