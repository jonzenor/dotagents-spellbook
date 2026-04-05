---
name: quarterly-planning
description: Forward-looking quarterly planning for Jon's Obsidian vault. Loads trends from recent weekly/daily notes, Vision.md, Rhythms.md, OmniFocus stalled projects, and last quarter's review, then runs a back-and-forth brainstorm conversation to set goals across all life areas before writing a Quarterly/YYYY-QX-Plan.md note. Use when user says "quarterly planning", "plan my quarter", "q2 planning", "plan the next quarter", or similar forward-looking quarterly phrases.
---

# Quarterly Planning

## Quick Start

Run this skill at the start of a new quarter (or the week before). It loads data, presents findings, then has a back-and-forth conversation before writing the plan note.

## Workflow

### 1 — Load Context (parallel)
- Last 6–8 weekly notes → recurring themes, rolled-over goals, struggles
- Last 2–3 weeks of daily notes → energy and avoidance patterns
- `Vision.md` → stated values and priorities
- `Rhythms.md` → current standing commitments
- OmniFocus stalled/on-hold projects: `query_omnifocus {"status": ["OnHold"], "fields": ["name", "status", "deferDate"]}`
- Previous quarter review note (`Quarterly/YYYY-QX-Review.md`) → lessons and "things not to lose"
- Determine the upcoming quarter label (Q1=Jan–Mar, Q2=Apr–Jun, Q3=Jul–Sep, Q4=Oct–Dec)

### 2 — Present Findings First
Synthesize what you found. Do NOT ask questions yet. Surface:
- Recurring struggles and themes
- Goals that kept rolling without completing
- Stalled OmniFocus projects
- Drift between Vision.md priorities and actual patterns
- Lessons from last quarter's review

End with: *"Does this match what you've been feeling? Anything surprising or missing?"*

### 3 — Back-and-Forth Brainstorm
Conversation, not a form. One life area at a time — see [REFERENCE.md](REFERENCE.md) for area details and conversation guidelines.

### 4 — Rhythms Check
Compare agreed goals against Rhythms.md. Suggest additions, changes, or removals. Let Jon decide.

### 5 — Finalize and Write
Summarize goals, ask about stop/avoid items, then write `Quarterly/YYYY-QX-Plan.md`.
Offer to add OmniFocus projects for any goals that need them.

See [REFERENCE.md](REFERENCE.md) for the plan note structure and goal format.
