---
name: work-issue
description: Implement a GitHub issue from a PRD breakdown. Reads the issue and parent PRD, explores the codebase, creates a branch, implements acceptance criteria, runs tests, and commits. Use when the user wants to work on, implement, or build a specific GitHub issue.
---

# Work Issue

Implement a single GitHub issue end-to-end.

## Process

### 1. Read the issue

The user will provide a GitHub issue number (or URL). Fetch it:

```bash
gh issue view <number>
```

Identify:
- The acceptance criteria (checklist items)
- The parent PRD issue number (from the "Parent PRD" section)
- Any blocked-by issues (from the "Blocked by" section)
- The user stories this addresses

### 2. Read the parent PRD

Fetch the parent PRD issue for full context:

```bash
gh issue view <prd-number>
```

Pay attention to:
- Implementation Decisions (schema, architecture, authorization patterns)
- Testing Decisions (what to test, prior art)
- Any relevant sections that inform this specific slice

### 3. Check blockers

If the issue lists blockers, verify they are complete:

```bash
gh issue view <blocker-number> --json state -q '.state'
```

If blockers are still open, inform the user and ask how to proceed. The blocker's code may already be merged even if the issue is open — check the branch.

### 4. Check for in-progress label

Before starting work, check if another agent has already claimed this issue:

```bash
gh issue view <number> --json labels -q '.labels[].name'
```

If the `in-progress` label is present, inform the user that this issue is already being worked on by another agent. Ask how to proceed.

### 5. Claim the issue

Add the `in-progress` label and leave a comment noting work has started:

```bash
gh issue edit <number> --add-label "in-progress"
gh issue comment <number> --body "Work started by Claude Code on branch \`<branch-name>\`"
```

### 6. Read project conventions

Read these files to understand codebase patterns:
- `CLAUDE.md`
- `ai/CONVENTIONS.md`
- `ai/ARCHITECTURE.md`
- `ai/AGENT_IMPLEMENTATION_GUIDE.md`

### 7. Explore the codebase

Explore the specific areas of the codebase relevant to this issue. Read existing files that will be modified or that demonstrate patterns you need to follow. Do not guess at patterns.

### 8. Create or confirm a branch

Check the current branch name with `git branch --show-current`.

**Branch naming conventions:**
- PRD branches: `prd-<short-name>` (e.g., `prd-role-migration`) — all issue work commits directly here
- Standalone issue branches: `<descriptive-name>` (e.g., `admin-flag-role-infrastructure`) — for issues without a PRD

**If you are on a `prd-` PRD branch** (e.g., `prd-role-migration`):
- Work directly on this branch. Do not create sub-branches.

**If you are on `staging`, `main`, or any other branch:**
- Create a standalone branch:
  ```bash
  git checkout staging && git pull
  git checkout -b <descriptive-branch-name>
  ```

Use kebab-case. Name branches after the issue's purpose, not the issue number.

### 9. Plan the implementation

Before writing code, briefly plan the implementation:
- Identify the files you'll create or modify, in order
- Follow the implementation order: migration → model → action → notification → gate/policy → livewire component → tests
- If invoked by `/auto-process-prd`, proceed directly without waiting for confirmation
- If invoked standalone by the user, present the plan and wait for confirmation

### 10. Implement

Work through the acceptance criteria one at a time:

- Follow the incremental step protocol from `ai/AGENT_IMPLEMENTATION_GUIDE.md`
- Run `./vendor/bin/pest` after each logical unit of work
- Fix any test failures before moving on
- Commit each logical unit separately with descriptive commit messages
- **Every commit must reference the issue number** (e.g., `feat: add admin_granted_at column #281`)

### 11. Final commit and verification

After all acceptance criteria are met:

1. Ensure the **final commit message** includes `Closes #<issue-number>` to auto-close the issue when merged:
   ```
   feat: complete admin flag infrastructure

   Closes #281

   Co-Authored-By: Claude Opus 4.6 (1M context) <noreply@anthropic.com>
   ```
2. Run the full test suite: `./vendor/bin/pest`
3. Verify all acceptance criteria checkboxes can be checked
4. Summarize what was built and any notes for the user

### 12. Close the issue

After the final commit, close the issue on GitHub and remove the `in-progress` label:

```bash
gh issue edit <number> --remove-label "in-progress"
gh issue close <number> --reason completed
```

### 13. Do NOT push

Remind the user that pushing and creating the PR is their responsibility.

## Important rules

- **Never push.** Pushing is always done by the user.
- **Never add scope.** Only implement what the acceptance criteria specify.
- **Never skip tests.** Every new piece of functionality needs test coverage.
- **Ask when uncertain.** If an acceptance criterion is ambiguous, ask before implementing.
- **Follow existing patterns.** Read the codebase before writing; copy established conventions exactly.
- **Reference the issue in every commit.** Use `#<number>` in all commit messages, `Closes #<number>` in the final one.
