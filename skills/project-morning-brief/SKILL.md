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
5. Return **Projects to Advance Today** — preserve the order returned by GTD triage and select no more than two projects.
6. Omit unselected projects by default. Include an unselected project only when GTD triage found a genuine `⚠️ At risk` or `🔥 Urgent` exception Jon needs to see today. A full all-project audit is available only when Jon explicitly requests it.
7. Link every project that is surfaced. When task-level output was explicitly requested, preserve the exact OmniFocus task name returned by triage; otherwise point Jon to the matching OmniFocus project or perspective without reproducing the action in the journal.

Use risk labels exactly as returned by GTD triage. Do not independently add or reinterpret them.

## Output

```markdown
**Projects to Advance Today:**
- [[Projects/Project File|Project Name]] — [optional risk flag and one-sentence reason it deserves attention]

**Project exceptions:**
- [[Projects/Another Project|Another Project]] — ⚠️ At risk: [concise evidence; omit this section when empty]
```

Omit either group when it is empty. Do not explain why every other project was not selected.

If there are no active projects, output:

```markdown
**Projects to Advance Today:**
- None
```

Keep this a compact read-only view, never a copied checklist or second task system.
