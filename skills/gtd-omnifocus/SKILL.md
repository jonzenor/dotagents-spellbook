---
name: gtd-omnifocus
description: Central GTD workflow for Jon's OmniFocus system. Use automatically from morning startup, afternoon check-in, evening summary, project triage, weekly planning, quarterly planning, project lifecycle work, or whenever an agent captures, organizes, surfaces, reviews, pauses, resumes, or completes tasks and projects in OmniFocus. Supports capture, orient, triage, review, and lifecycle modes; this is an internal capability and never an additional routine Jon must remember to run.
---

# GTD OmniFocus

Keep OmniFocus trustworthy by ensuring every visible action is something Jon can honestly do now. Obsidian project notes hold outcome and context; OmniFocus holds every executable next action.

## Select a mode

- `capture` — clarify and file explicit actions. Evening summary invokes this automatically.
- `orient` — read current execution views and recommend perspectives without changing tasks. Morning startup invokes this automatically.
- `triage` — select genuinely actionable task-level work without changing it. Afternoon check-in and project morning brief invoke this automatically.
- `review` — perform the GTD weekly review. Weekly planning invokes this automatically and may delegate the audit to a sub-agent.
- `lifecycle` — synchronize OmniFocus when a vault project is committed, renamed, paused, resumed, completed, or dropped. Quarterly planning and project-lifecycle work invoke this automatically.

Never ask Jon to run this skill separately when a parent routine has already invoked it.

## System invariants

1. Store every task in OmniFocus. Put project-specific actions in the matching OmniFocus project and standalone actions in the appropriate single-action list or Inbox. Never create task checklists in Obsidian project notes.
2. Require commitment before creating an action. Do not convert ideas, questions, aspirations, or “maybe” language into obligations.
3. Phrase each task as one visible physical next action. Separate outcomes from actions and split independent actions.
4. Keep unavailable work out of execution perspectives:
   - Use a defer date for a committed action that should reappear on a known date.
   - Use `Waiting On` and the appropriate `Waiting On: <name>` child tag for a dependency or delegation.
   - Use `Someday Maybe` for an uncommitted possibility or unaffordable work with no honest start date.
   - Use an on-hold or paused project state when the whole project cannot currently move.
5. Use due dates only for real external deadlines. Never manufacture urgency with aspirational dates.
6. Use flags only for the small set of available actions Jon chooses to protect this week. Flags populate `King - Protect This Week`; they are not a priority scale.
7. Duplicate-check semantically before every create. Similar meaning counts even when wording differs.
8. Preserve uncertainty. Ask during review when commitment, affordability, project mapping, or state requires Jon's judgment.

## Taxonomy

Apply one accurate context, one energy tag, and one time tag when sufficient information exists. Context accuracy outranks estimates; use the broader valid context when uncertain.

- Contexts include `@Home`, `@Office`, `@Home Computer`, `@Work Computer`, `@Computer`, `@Phone Calls`, `@Errands` and its children, `@Denver`, and `@Anywhere`.
- Energy tags live under `Mental Energy`: `High Energy`, `Medium Energy`, and `Low Energy`.
- Time tags live under `Time`: `5 Minutes - Quick Win`, `15 Minutes - Light Focus`, `30 Minutes - Medium Focus`, and `60 + Minutes - Deep Focus`.
- Generic `@Computer` means home/personal computer work. Work actions require `@Work Computer`.

Never invent a near-match; OmniFocus may silently create a new tag.

## Capture mode

1. Drain `Reference/Logs/OmniFocus Failure Queue.md` before new captures. Remove only entries successfully written and verified.
2. Extract direct commitments from the supplied source. Reject noncommittal language. **Exception:** unchecked items the caller supplies from a daily note's `## Quick List` are already commitments by virtue of being written there — accept them as-is, even as bare fragments, and do not reject them for lacking commitment phrasing. Word them as proper next actions when creating the task.
3. Determine whether each action belongs to a matching OmniFocus project or a single-action list.
4. Search for an equivalent task before writing.
5. Create or update the action with an accurate context, energy, time, availability state, deadline, and flag only when supported.
6. Verify every write. Report the action and destination to the caller.
7. Retry a failed read or write exactly once. If it still fails, append the complete intended write—including action, destination, tags, dates, flag, and source—to the failure queue. Never strand it only in narrative.

## Orient mode

Remain read-only.

1. Use calendar location, available time, likely energy, and weekly priorities supplied by the caller.
2. Query the smallest useful set of existing perspectives: `Bishop - Home`, `Bishop - Work`, `Home Computer`, `Work Computer`, `Phone Calls`, `Anywhere`, `Knight - Out & About`, `Pawn - Quick Wins`, `Rook - Deep Focus`, and `King - Protect This Week`.
3. Query overdue and due-today counts separately because deadlines form the hard landscape.
4. Return exact perspective names, concise reasons, and nonzero counts. Do not copy task lists unless the caller explicitly requests task-level triage.
5. Never surface deferred, waiting, someday/maybe, paused-project, completed, or dropped work as executable.

## Triage mode

Remain read-only. Use this mode only when the caller needs task-level selection rather than perspective guidance.

1. Accept the caller's scope, such as planned today, flagged, due soon, overdue, or a confirmed set of OmniFocus projects.
2. Resolve task state from current OmniFocus data. Keep only available actions; exclude deferred, waiting, someday/maybe, paused/on-hold project, completed, and dropped work even when stale flags or planned dates remain.
3. Never invent an action from project notes, narrative, outcomes, or unchecked items outside OmniFocus.
4. Rank hard deadlines first, then explicit weekly flags, current context/capacity, and finishability. Finishability never overrides a real deadline.
5. Select the minimum useful set. Normally return zero or one action per project; return two only when both are time-sensitive or sequentially necessary.
6. For project triage, classify projects as advance now or not selected. Order selected projects by real urgency, then weekly commitment, context/capacity, and finishability. Keep a concise evidence-based reason for every nonselected project.
7. Apply `⚠️ At risk` only when time, missing plan, blockage, or documented slippage threatens the stated outcome. Apply `🔥 Urgent` only when action is needed today to protect a real deadline or committed plan. Never infer risk from inactivity alone.
8. Return stable task ID, exact task name, OmniFocus project, relevant perspective/context, and a concise selection or exclusion reason.
9. If an action appears unaffordable, uncommitted, blocked, or repeatedly rejected but is still visible, flag it for `review` mode rather than silently reclassifying it.

## Review mode

1. Capture loose inputs and process the OmniFocus Inbox to zero: do, delegate, defer, schedule only genuine calendar events, move to `Someday Maybe`, or delete/drop.
2. Review every active OmniFocus project for alignment with its vault project's stated outcome and at least one physical next action, or an explicit waiting, deferred, or on-hold state.
3. Review `Waiting On` for follow-ups and changed dependencies.
4. Review `Someday Maybe` without pressure. Activate something only after explicit commitment and confirmation that money, time, and prerequisites support it.
5. Review upcoming deadlines and remove soft due dates.
6. Reset flags to the small set Jon intends to protect this week.
7. Inspect every execution perspective. Anything visible must be genuinely actionable now.
8. Return completed housekeeping, proposed changes requiring judgment, and focused questions to the parent weekly-planning routine.

When delegated to a sub-agent, perform the read-only audit and return recommendations. Let the parent agent obtain Jon's decisions and apply ambiguous or material changes. Do not require delegation when the review is small.

## Lifecycle mode

1. Keep each committed vault project paired with an identically named OmniFocus project in the correct Area-of-Focus folder.
2. On commitment, create or locate the OmniFocus project and add at least one concrete next action. Keep the Obsidian project note task-free.
3. On rename or move, update the OmniFocus project without losing actions.
4. On pause, waiting, or resume, apply the state explicitly and preserve an honest resurfacing date when one exists.
5. On completion or drop, update OmniFocus only from explicit evidence; never infer closure from inactivity or completed individual actions.
6. Verify the final project state and report it to the caller.
