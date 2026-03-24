---
name: work-prd
description: Work through an entire PRD by implementing its child issues one at a time in dependency order. Spawns a fresh agent for each issue, pauses for review between issues, and stops for HITL issues that need human input. Use when the user wants to implement a whole PRD, work through all issues in a PRD, or build out a full feature from its PRD.
---

# Work PRD

Orchestrate implementation of an entire PRD by working its child issues in dependency order.

## Process

### 1. Identify the PRD and its issues

The user will provide a PRD GitHub issue number. Fetch it:

```bash
gh issue view <number>
```

Then find all child issues that reference this PRD as their parent. Search for issues that mention the PRD number:

```bash
gh issue list --label "prd-issue" --search "Parent PRD #<number>" --state open --limit 50
```

If that doesn't return results, ask the user for the issue numbers or search more broadly.

### 2. Build the dependency graph

For each child issue:
- Read the issue to find its "Blocked by" section
- Determine if it is HITL or AFK (from the issue body or from the PRD breakdown)
- Check for `in-progress` label — if present, skip it (another agent is working on it)
- Build a dependency-ordered list (topological sort)

Present the ordered work plan to the user:

```
PRD #280: Role-to-Position Migration

PRD branch: prd/role-migration

Work order:
1. #281 - Admin Flag & Role Infrastructure (AFK) — no blockers
2. #282 - Seed Permission Roles (AFK) — blocked by #281
3. #283 - Refactor Authorization Gates (AFK) — blocked by #282
4. #284 - Refactor Policies (AFK) — blocked by #282
5. #285 - Role Assignment UI (AFK) — blocked by #281

Issues #283, #284, #285 can be parallelized after their blockers complete,
but will be worked sequentially for safety.

Skipped (in-progress by another agent):
  (none)
```

Ask the user to confirm the work order before starting.

### 3. Create the PRD branch

Create a single PRD branch from staging that all issue work will be merged into:

```bash
git checkout staging && git pull
git checkout -b prd-<short-prd-name>
```

All issue commits go directly on this branch — no sub-branches needed.

### 4. Work each issue

For each issue in dependency order:

#### If the issue is AFK:

1. Ensure you are on the PRD branch (`prd-<short-prd-name>`).

2. Spawn a **new agent** to implement the issue. Give it a clear prompt:

   > Read `CLAUDE.md` first. Then implement GitHub issue #<number> for the Lighthouse Website project.
   > Fetch the issue with `gh issue view <number>`. Read the parent PRD issue #<prd-number> for context.
   > Read `ai/CONVENTIONS.md`, `ai/ARCHITECTURE.md`, and `ai/AGENT_IMPLEMENTATION_GUIDE.md`.
   > You are on branch `prd-<short-prd-name>`. Do NOT create a new branch.
   > Do NOT ask the user for confirmation — just implement all acceptance criteria.
   > Reference #<number> in every commit message. Use `Closes #<number>` in the final commit.
   > Run tests after each logical unit, and commit each logical unit separately.
   > Do not push. Summarize everything you did when complete.

3. Close the issue on GitHub and remove the `in-progress` label:

   ```bash
   gh issue edit <number> --remove-label "in-progress"
   gh issue close <number> --reason completed
   ```

5. Present a summary to the user:
   - Which issue was completed
   - What files were created/modified
   - Test results
   - Any notes or concerns from the agent

6. Ask the user: **"Issue #X is complete and merged into the PRD branch. Ready to proceed to issue #Y?"**

7. Wait for user confirmation before starting the next issue.

#### If the issue is HITL:

1. Inform the user that this issue requires human interaction:

   > Issue #<number> (<title>) is marked as HITL (Human-in-the-Loop).
   > This issue requires your input before or during implementation.
   >
   > <show the issue description and acceptance criteria>
   >
   > Would you like to:
   > - Work through this issue together interactively
   > - Skip it for now and continue with non-blocked issues
   > - Provide guidance so I can attempt it as AFK

2. If the user wants to work it interactively, create the issue branch from the PRD branch first, then invoke `/work-issue <number>` which will handle the interactive implementation (tell it NOT to create a new branch).

3. After interactive work completes, merge the issue branch into the PRD branch.

4. If the user wants to skip, mark it as skipped and continue with any issues that aren't blocked by it. Remind the user at the end which issues were skipped.

### 5. Handle parallel-eligible issues

When multiple issues share the same blocker and could theoretically run in parallel:
- Work them **sequentially** (one agent at a time) for safety and reviewability
- Note in the summary that they were parallel-eligible in case the user wants to adjust

### 6. Update documentation

After all issues are complete (before creating the PR), update documentation for the feature:

1. Run `/document-feature <feature-name>` to create or update the technical documentation.
2. Run `/write-user-docs <feature-name>` if the feature has user-facing aspects.
3. Run `/write-staff-docs <feature-name>` if the feature has staff-facing aspects.

These skills handle both creating new docs and updating existing docs. Running them at the end ensures documentation reflects the complete feature, not partial slices.

Commit documentation changes to the PRD branch.

### 7. Final summary

After all issues are complete (or skipped), present a final report:

```
PRD #280: Role-to-Position Migration — Complete

PRD branch: prd/role-migration

Completed:
  ✓ #281 - Admin Flag & Role Infrastructure
  ✓ #282 - Seed Permission Roles
  ✓ #283 - Refactor Authorization Gates
  ✓ #284 - Refactor Policies
  ✓ #285 - Role Assignment UI

Skipped:
  (none)

Documentation updated:
  ✓ docs/features/Role Authorization.md (technical)
  ✓ resources/library/books/staff-handbook/... (staff docs)

All issue branches have been merged into prd/role-migration.
Next steps:
  1. Push prd/role-migration to remote
  2. Create a PR from prd/role-migration → staging
  3. CodeRabbit will review the full diff
  4. Address any review feedback
  5. Merge into staging for testing
```

## Important rules

- **One PRD branch, one issue branch per issue.** PRD branch is the integration point. Issue branches merge into it.
- **One agent per issue.** Each issue gets a fresh agent to keep context clean.
- **Never push.** Pushing is always done by the user.
- **Always pause between issues.** Even for AFK issues, summarize and ask before continuing.
- **Stop for HITL.** Never attempt a HITL issue without user involvement.
- **Respect dependency order.** Never work an issue before its blockers are merged into the PRD branch.
- **Merge after each issue.** Issue branches are merged into the PRD branch immediately after completion, so subsequent issues have access to prior work.
- **Check for `in-progress` label.** Skip issues claimed by another agent (e.g., Codex). Note them as skipped.
- **Update docs at the end.** Documentation is updated after all code is complete, not between issues.
- **PR goes to staging.** The final PR targets `staging`, not `main`.
