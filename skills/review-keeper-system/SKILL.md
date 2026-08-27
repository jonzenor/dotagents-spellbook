---
name: review-keeper-system
description: Audits Jon's Keeper journaling and life-management system for drift, duplication, friction, broken handoffs, and misplaced human or AI responsibility. Use when running quarterly review or when Jon asks to review, audit, troubleshoot, or improve the Keeper system; it recommends changes but never implements them.
---

# Review Keeper System

## Purpose

Evaluate whether Keeper is helping Jon manage life with less friction and greater clarity, continuity, follow-through, and intentionality. The governing test is:

> Is Keeper handling friction while Jon retains reflection, judgment, prayer, creativity, relationships, and decisions?

This is a system-health review, not an invitation to optimize everything that can be automated.

## Scope

- **Quarterly pulse:** When invoked by `quarterly-review`, perform a compact review and return only material findings.
- **Full audit:** When Jon explicitly asks to audit or improve the system, examine the system comprehensively and open a conversation before any changes.

## Evidence to read

Read the minimum set needed for the selected scope:

- `AGENTS.md`, `Keeper Instructions.md`, `Horizons.md`, and `Rhythms.md`
- The current instructions for morning startup, afternoon check-in, evening summary, weekly planning, Weekly Advisor, quarterly planning/review, project morning brief, and GTD–OmniFocus
- Representative recent daily notes and weekly notes; include reruns, sparse notes, and missed-day recovery when available
- The current quarterly plan and most recent quarterly review
- `Reference/Logs/OmniFocus Failure Queue.md` and `Reference/Logs/Vault Maintenance Log.md`
- Project, Almanac, Scripture-memory, Person, Mentoring, or domain-log state only when needed to verify a suspected handoff

Prefer actual outputs and repeated behavior over theoretical concerns. A direct contradiction between instructions is evidence even before it produces a visible failure.

## Review tests

Check for:

1. **Single sources of truth** — actions remain in OmniFocus; reflection remains Jon's; context and decisions live in their intended vault layers.
2. **Human/AI boundary** — AI removes remembering, filing, tracking, and resurfacing work without replacing reflection, discernment, prayer, creativity, or relationships.
3. **Continuity** — meaningful information can move from today to week, projects, goals, and longer direction without repetition or disappearance.
4. **Resilience** — missed days, sparse notes, unavailable integrations, and interrupted routines recover without guilt, unbounded catch-up, or stranded state.
5. **Instruction drift** — schemas, paths, terminology, triggers, and ownership rules agree across skills and live notes.
6. **Cognitive load** — morning and afternoon outputs stay scannable; evening processing does not create duplicate execution lists; Keeper-only notes do not become required reading for Jon.
7. **Value of maintenance** — every prompt, checklist, log, review, and recurring process earns its cost through later retrieval, insight, or action.

Classify findings as **Confirmed defect**, **Observed friction**, or **Watch**. Do not present speculation as a defect.

## Output

For a quarterly pulse, return:

- **Healthy:** at most three mechanisms worth preserving
- **Repair:** at most three evidence-backed defects or recurring frictions
- **Watch:** at most two early signals that need more usage before intervention

For each Repair item, state the evidence, smallest proposed change, expected benefit, and implementation effort. Omit empty sections. If the system is healthy, say so instead of inventing improvements.

For a full audit, use the same categories with enough detail to support conversation, then rank recommendations by impact and effort.

## Boundaries

- Read-only: never edit skills, notes, tasks, integrations, or configuration during this review.
- Never create OmniFocus actions from system-improvement suggestions unless Jon explicitly commits to one later.
- Never add a routine, prompt, metric, dashboard, or note merely because it is possible.
- Never require Jon to read weekly notes or other Keeper working state. Jon's user-facing execution surfaces remain the daily note for orientation/reflection and OmniFocus for actions.
- Do not run more often than quarterly automatically. Explicit troubleshooting requests may invoke it at any time.
- End with recommendations and wait for Jon's decisions before implementation.
