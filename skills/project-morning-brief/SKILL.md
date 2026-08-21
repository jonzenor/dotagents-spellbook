---
name: project-morning-brief
description: Reviews every active life project and produces a compact, deadline-aware project triage for today's morning brief. Use during morning startup or when Jon asks which active projects need attention today, whether a project is at risk, or what project actions should be prioritized now.
---

# Project Morning Brief

## Workflow

1. Read `Projects/Projects.md`, then open every listed project note in full for outcome, decisions, constraints, and context.
2. Read today's date, this week's weekly note, yesterday's daily note, and relevant calendar deadlines already supplied by the caller.
3. Verify the vault project attention states from explicit evidence. Never infer completion, abandonment, pause, or resume.
4. **Invoke `gtd-omnifocus` in `triage` mode automatically** — supply the confirmed project names, outcomes, deadlines, recent progress, weekly priorities, today's capacity, and hard landscape. Do not ask Jon to run the GTD skill separately.
   - Let GTD triage load the matching OmniFocus projects and own task availability, waiting/deferred/someday exclusions, urgency, finishability, and action selection.
   - Never derive or invent tasks from the Obsidian project notes.
5. Divide the returned results into two groups:
   - **Projects to Advance Today** — preserve the order and exact actions returned by GTD triage.
   - **Other Active Projects** — preserve GTD triage's concise exclusion reason.
6. Always link every active project so Jon can open its outcome and context note. When task-level output was explicitly requested, preserve the exact OmniFocus task name returned by triage.

Use risk labels exactly as returned by GTD triage. Do not independently add or reinterpret them.

## Output

```markdown
**Projects to Advance Today:**
- [[Projects/Project File|Project Name]] — [optional risk flag and one-sentence explanation]
  - OmniFocus: [exact selected action name]

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

Keep this a compact read-only view, never a copied checklist or second task system.
