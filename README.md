# dotagents-spellbook 🧙

My personal library of Claude Code agent skills. These power a PRD-driven development
workflow across projects — from idea capture through implementation, code review, and
documentation.

---

## Setup

```bash
# Clone the repo
git clone git@github.com:jonzenor/dotagents-spellbook.git ~/dotagents-spellbook

# Symlink skills into Claude Code's skill directory
ln -s ~/dotagents-spellbook/skills ~/.agents/skills
ln -s ~/dotagents-spellbook/skills ~/.claude/skills
```

> Claude Code looks for skills in `~/.claude/skills/`. The `~/.agents/skills` symlink
> is optional but keeps things tidy if you use `~/.agents/` as a hub.

---

## The Workflow

These skills implement a full PRD-driven development pipeline:

```
/capture-idea         →  save a rough idea as a GitHub issue
/prioritize-backlog   →  decide what to build next
/write-a-prd          →  flesh out a feature into a full PRD
/prd-to-issues        →  break the PRD into vertical-slice GitHub issues
/work-prd             →  implement all issues in a PRD (semi-supervised)
/auto-process-prd     →  advance a PRD autonomously on a schedule
```

Each project keeps its own GitHub repo and issue tracker. The skills are project-agnostic —
they read context from the project's `CLAUDE.md` and GitHub issues at runtime.

---

## Skills

### PRD Pipeline

| Skill | What it does |
|---|---|
| `/write-a-prd` | Interviews you, explores the codebase, and writes a full PRD as a GitHub issue |
| `/prd-to-issues` | Breaks a PRD into independently-workable vertical-slice GitHub issues |
| `/work-prd` | Implements all issues in a PRD in dependency order, pausing between issues for review |
| `/auto-process-prd` | Fully autonomous PRD lifecycle manager — implement, open PR, handle code review, process test feedback. Designed to run on a schedule via `/loop` |

### Issue Work

| Skill | What it does |
|---|---|
| `/work-issue` | Implements a single GitHub issue end-to-end: reads issue + parent PRD, explores codebase, branches, implements, tests, commits |
| `/fix-test-feedback` | Addresses open `test-feedback` GitHub issues with small targeted fixes on the PRD branch |

### Planning & Design

| Skill | What it does |
|---|---|
| `/prd-to-plan` | Turns a PRD into a multi-phase implementation plan saved as a local Markdown file |
| `/grill-me` | Relentlessly interviews you about a plan or design until you've thought through every branch |

### Backlog Management

| Skill | What it does |
|---|---|
| `/capture-idea` | Saves a rough idea as a lightweight GitHub issue for later PRD refinement |
| `/prioritize-backlog` | Analyzes open ideas and PRDs and recommends the top 3 to work on next |

### Meta

| Skill | What it does |
|---|---|
| `/write-a-skill` | Creates a new skill with proper structure and progressive disclosure |
| `/find-skills` | Helps discover and install skills from the registry |

---

## Project-Specific Skills

Some skills are project-specific and live in the project repo under `.claude/skills/`,
not here. These are typically documentation skills that know the project's handbook
structure.

To wire them into `/auto-process-prd`, add a `Documentation Skills` section to the
project's `CLAUDE.md`:

```markdown
### Documentation Skills

When `/auto-process-prd` completes all implementation issues, it runs these in order:

1. `/document-feature <feature-name>` — technical reference doc
2. `/write-user-docs <feature-name>` — user handbook pages
3. `/write-staff-docs <feature-name>` — staff handbook pages
```

`/auto-process-prd` reads this section at runtime and runs whichever skills are listed.
If no `Documentation Skills` section exists, it falls back to `/document-feature` only.

---

## Adding a New Skill

```bash
mkdir ~/dotagents-spellbook/skills/my-skill
# Write the skill
touch ~/dotagents-spellbook/skills/my-skill/SKILL.md
```

Skills are picked up immediately — no restart needed. Use `/write-a-skill` to scaffold
one with the right structure.
