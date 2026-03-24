---
name: capture-idea
description: Capture a rough product or feature idea as a lightweight GitHub issue for later refinement into a PRD. Use when the user wants to quickly save ideas, backlog possibilities, early concepts, or anything that should be labeled as an idea and revisited later with write-a-prd.
---

# Capture Idea

Create a lightweight GitHub issue that preserves an idea without turning it into a full PRD.

## Goal

Optimize for speed. Capture just enough context that the issue can later be turned into a PRD with `write-a-prd`.

Always treat this as an intake step, not a specification step.

## Process

1. Ask a short set of questions. Keep it moving and do not over-interview.
2. Normalize the answers into a concise GitHub issue using the template below.
3. Create the issue with labels `idea` and `needs-prd`.
4. Return the issue number and suggest using `write-a-prd` later when the user is ready to refine it.

## Interview Rules

Ask only for the minimum useful detail. Prefer 4-6 quick questions max:

- What is the idea?
- What problem or opportunity does it address?
- Who is it for?
- What is the rough shape of the solution?
- Are there important constraints, context, or dependencies?
- What is still unknown or undecided?

If the user already gave enough information, do not ask redundant questions.

If details are fuzzy, capture the ambiguity in `Open Questions` rather than forcing precision.

## Issue Template

Use this template for the GitHub issue body:

<idea-template>

## Summary

A short paragraph describing the idea.

## Problem / Opportunity

Why this might be worth doing.

## Who It Helps

The primary users, teams, or stakeholders affected.

## Rough Direction

The likely shape of the solution at a high level. Keep this intentionally lightweight.

## Constraints / Context

Known constraints, dependencies, or relevant background.

## Open Questions

- Question 1
- Question 2

## Next Step

Refine this issue into a full PRD using `write-a-prd` when ready to prioritize and scope implementation.

</idea-template>

## GitHub Issue Creation

When creating the issue:

- Use a concise, descriptive title.
- Apply labels `idea` and `needs-prd`.
- Do not add implementation acceptance criteria.
- Do not break the work into slices.
- Do not create child issues yet.

If the repository uses a consistent area label and the correct label is obvious from context, add it. Otherwise, keep labeling minimal.

Prefer this command shape:

```bash
gh issue create \
  --title "<concise idea title>" \
  --label "idea" \
  --label "needs-prd" \
  --body "<filled idea template>"
```

If an additional area label is clearly warranted, add another `--label` flag.

Before running the command, make sure the body is fully drafted from the template so the issue can be created in one shot.

## Handoff

After creating the issue, provide:

- The GitHub issue number or URL
- A one-line summary of what was captured
- A suggested handoff such as: `Use write-a-prd with issue #123 when you want to turn this into a full PRD.`
