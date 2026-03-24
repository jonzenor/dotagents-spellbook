---
name: prioritize-backlog
description: Fetch all GitHub issues labeled "idea"+"needs-prd" and "PRD" (not in-progress), analyze them for strategic value, and recommend the top 3 features to work on next. Use when user wants to prioritize the backlog, decide what to build next, review ideas, or triage upcoming work.
---

# Prioritize Backlog

Review all unstarted ideas and PRDs, then recommend the top 3 to work on next.

## Process

### Step 1 — Ask for Context

Before fetching or analyzing anything, ask the user:

- "What are the current board directives or strategic priorities for this trimester?"
- "Are there any urgent community needs, upcoming events, or deadlines I should factor in?"
- "Anything you want me to weight higher or lower this time around?"

Wait for their response. Use their answers to inform scoring in Step 4 — especially the **Strategic value** criterion. If the user says "none" or "just run it," proceed with the default criteria.

### Step 2 — Gather Issues

Run these commands to fetch the two pools of work:

```bash
# Ideas that need a PRD
gh issue list --label "idea" --label "needs-prd" --state open --json number,title,body,labels,createdAt

# PRDs that exist but haven't started
gh issue list --label "PRD" --state open --json number,title,body,labels,createdAt | jq '[.[] | select(.labels | map(.name) | index("in-progress") | not)]'
```

If `jq` is unavailable, filter in-progress PRDs manually from the JSON output.

### Step 3 — Read the CLAUDE.md and ARCHITECTURE.md

Read `CLAUDE.md` and `ai/ARCHITECTURE.md` for context on the current state of the project, what exists, and what's in flight.

### Step 4 — Score Each Item

Evaluate every issue against these criteria, weighted by importance:

| Criterion | Weight | Description |
|---|---|---|
| **Strategic value** | 40% | Does it enhance the community experience, build trust, or serve the ministry's mission? |
| **Staff burden reduction** | 25% | Does it automate manual work or reduce staff toil? |
| **Unblocks other work** | 15% | Is it a dependency for other ideas/PRDs in the backlog? |
| **Community reach** | 10% | How many users or user types benefit? |
| **Feasibility** | 10% | Can it be built on existing infrastructure with reasonable effort? |

Assign each item a 1-5 score per criterion, then compute a weighted total.

### Step 5 — Present Recommendations

Present the **top 3** in ranked order. For each recommendation, include:

1. **Issue number and title**
2. **Score breakdown** — a compact table showing scores per criterion
3. **Why this ranks high** — 2-3 sentences on strategic reasoning
4. **Dependencies or blockers** — anything that must land first
5. **Status** — whether it's an idea (needs PRD first) or a ready PRD

Also include a **full ranked list** of all remaining items below the top 3, as a compact table with overall scores.

### Step 6 — Discuss

After presenting, ask the user:

- "Do these priorities feel right, or should any item move up or down?"
- "Is there anything I'm weighting wrong given what you told me about current priorities?"
- "Want me to start a PRD for the top-ranked idea, or begin working the top-ranked PRD?"

Continue the conversation until the user is satisfied with the priority order. Adjust rankings based on their input and re-present if they make changes.

## Rules

- Assume whatever is in-progress will finish before the next item starts — don't penalize items that depend on current work.
- If an idea and a PRD cover the same feature, prefer the PRD (it's further along).
- Don't recommend items that are blocked by other unstarted items without noting the dependency chain.
- Keep the analysis grounded in what you know about the project from CLAUDE.md and the issue descriptions — don't invent business context.
