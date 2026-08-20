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
5. If there are more than six active projects, apply a strong **finishability bias** before selecting today's work:
   - Give extra weight to projects whose outcomes can be completed soon with a small number of clear, unblocked actions.
   - Prefer finishing a nearly complete project over merely advancing a larger project, regardless of which has the earlier deadline, unless a concrete urgent commitment requires action today.
   - Estimate finishability from the remaining executable work and dependencies, not from the deadline alone. Do not call a project finishable when its apparent last action is blocked or when major unstated work remains.
   - Use this bias to reduce the active-project count and cognitive load; it does not override `🔥 Urgent` work that must happen today.
6. Divide projects into two groups:
   - **Projects to Advance Today** — projects with at least one selected action for today. Order these by urgency first, then finishability when there are more than six active projects, then weekly priority and fit with today's capacity.
   - **Other Active Projects** — projects that are blocked, waiting, not prioritized, or not workable today. Keep the concise reason each one is not being worked today.
7. Select only the action or actions that deserve attention today. Usually select zero or one per project; use two only when both are genuinely time-sensitive or sequentially necessary today.
8. Choose actions from the project's `## Next Actions` section.
   - Treat `Define what needs to be done` as a real planning action, not as the absence of an action.
   - When it is the project's only next action—especially for a newly committed project—normally surface it on the next day with workable capacity so the project does not remain active but inert.
   - State `No project action needed today` only when the project has actionable work but timing, dependencies, priorities, or today's capacity make deferral appropriate. Do not use that phrase merely because the project still needs definition.
9. Always link every active project so Jon can open the full note and checklist.

## Risk flags

Flag risk only from concrete evidence:

- `⚠️ At risk` — the remaining time, missing plan, blockage, or recent slippage makes the intended outcome uncertain. Explain why in one sentence.
- `🔥 Urgent` — action is needed today or the project will probably miss a stated deadline or committed plan. Explain why in one sentence.

Do not flag merely because a project was inactive for a few days or has many unchecked items. For ongoing projects, evaluate the promised cadence rather than inventing a deadline.

## Output

```markdown
**Projects to Advance Today:**
- [[Projects/Project File|Project Name]] — [optional risk flag and one-sentence explanation]
  - [ ] [Today's selected action]

> [!note]- Other Active Projects
> - [[Projects/Another Project|Another Project]]
>   - No project action needed today — [concise reason: blocked, waiting, lower priority, or poor fit for today's capacity].
```

Use Obsidian's collapsed callout for `Other Active Projects` instead of inline HTML or custom CSS. It visually quiets projects not being worked while preserving their links and reasons. Omit either group when it is empty; when every project is in the callout, write `- None selected today` under `Projects to Advance Today`.

If there are no active projects, output:

```markdown
**Projects to Advance Today:**
- None
```

Keep this a triage view, never a copy of every incomplete project action.
