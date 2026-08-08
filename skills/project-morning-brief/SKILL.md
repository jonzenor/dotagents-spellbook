---
name: project-morning-brief
description: Reviews every active life project and produces a compact, deadline-aware project triage for today's morning brief. Use during morning startup or when Jon asks which active projects need attention today, whether a project is at risk, or what project actions should be prioritized now.
---

# Project Morning Brief

## Workflow

1. Read `Projects/Projects.md`, then open every listed project note in full.
2. Read today's date, this week's weekly note, yesterday's daily note, and relevant calendar deadlines already supplied by the caller.
3. Verify each project is still active. Never infer completion or abandonment.
4. For each active project, evaluate:
   - deadline or target date and time remaining;
   - amount and sequence of remaining work;
   - blocked or waiting actions;
   - recent progress and explicit weekly priorities;
   - today's capacity and hard landscape;
   - for ongoing projects without an end date, whether cadence or momentum requires attention now.
5. Select only the action or actions that deserve attention today. Usually select zero or one per project; use two only when both are genuinely time-sensitive or sequentially necessary today.
6. Choose actions from the project's `## Next Actions` section.
   - Treat `Define what needs to be done` as a real planning action, not as the absence of an action.
   - When it is the project's only next action—especially for a newly committed project—normally surface it on the next day with workable capacity so the project does not remain active but inert.
   - State `No project action needed today` only when the project has actionable work but timing, dependencies, priorities, or today's capacity make deferral appropriate. Do not use that phrase merely because the project still needs definition.
7. Always link every active project so Jon can open the full note and checklist.

## Risk flags

Flag risk only from concrete evidence:

- `⚠️ At risk` — the remaining time, missing plan, blockage, or recent slippage makes the intended outcome uncertain. Explain why in one sentence.
- `🔥 Urgent` — action is needed today or the project will probably miss a stated deadline or committed plan. Explain why in one sentence.

Do not flag merely because a project was inactive for a few days or has many unchecked items. For ongoing projects, evaluate the promised cadence rather than inventing a deadline.

## Output

```markdown
**Active Projects:**
- [[Projects/Project File|Project Name]] — [optional risk flag and one-sentence explanation]
  - [ ] [Today's selected action]
- [[Projects/Another Project|Another Project]]
  - No project action needed today.
```

If there are no active projects, output:

```markdown
**Active Projects:**
- None
```

Keep this a triage view, never a copy of every incomplete project action.
