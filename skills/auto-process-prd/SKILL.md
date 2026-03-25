---
name: auto-process-prd
description: Automatically advance an in-progress PRD through its lifecycle — implement issues, open PRs, handle code review, and process test feedback. Use when user wants to auto-process a PRD, advance PRD state, or run the PRD pipeline. Designed to run on a recurring schedule.
---

# Auto-Process PRD

State-machine manager for advancing a PRD through its lifecycle. Executes exactly **one step** per invocation, then exits. Designed to run on a ~30-45 minute schedule via `/loop`.

**CRITICAL: NEVER touch the `main` branch. All PRs target `staging` only.**

**FULLY AUTONOMOUS: Do not ask the user for confirmation on ANY commands — git, gh, skill invocations, or implementation decisions. Run everything directly. The user will review the output after each step.**

**TOOL RULES: Never use shell `find` or `grep` commands (they trigger permission prompts and block autonomous execution). Always use the Glob and Grep tools instead. When spawning agents, instruct them to follow the same rule. Never use `$()` subshells, `&&` chains, shell redirects (`>`, `>>`, `|`), or nested quotes in Bash commands — these all trigger permission prompts. Keep every Bash call a single simple command. To save command output, read it from the tool result in context — do NOT redirect to files.**

## Branch Strategy

All work happens directly on the PRD branch (`prd-<short-name>`). No issue sub-branches — commits go straight to the PRD branch. This keeps things simple since issues are worked sequentially.

## State Detection

Find the active PRD:

```bash
gh issue list --label "prd" --label "in-progress" --state open --limit 1 --json number,title
gh issue list --label "prd" --label "code-review" --state open --limit 1 --json number,title
gh issue list --label "prd" --label "testing" --state open --limit 1 --json number,title
```

Exactly one PRD should be active. If multiple are found, report the conflict and exit. If none are found, report "No active PRD" and exit.

The PRD's state is determined by its labels:

| Labels | State |
|---|---|
| `prd` + `in-progress` | **Implementing** |
| `prd` + `in-progress` + all issues closed | **Documentation** (transition step — adds `documenting-prd` label for visibility) |
| `prd` + `code-review` | **Code Review** |
| `prd` + `testing` | **Testing** |

## State: Implementing (`in-progress`)

### Find the next issue

Fetch all child issues for this PRD:

```bash
gh issue list --label "prd-issue" --search "Parent PRD #<prd-number>" --state open --limit 50 --json number,title,labels
```

For each open issue:
1. Skip any with the `in-progress` label (claimed by another agent)
2. Read the issue body to find its "Blocked by" section
3. Skip any whose blockers are still open
4. Select the **lowest-numbered** unblocked issue

### If a next issue exists:

1. Ensure you are on the PRD branch (`prd-<short-name>`). Create it from staging if it doesn't exist.
2. Invoke `/work-issue <number>` to implement it.
   - `/work-issue` handles implementation, commits, and closing.
   - All commits land directly on the PRD branch.
3. Exit. The next invocation will pick up the next issue.

### If NO issues remain (all closed):

All implementation is complete. Run documentation, then transition to code review.

0. **Add visibility label** so progress is trackable from GitHub:
   ```bash
   gh issue edit <prd-number> --add-label "documenting-prd"
   ```

1. **Update documentation** for the feature:
   - Read the project's `CLAUDE.md` and find the `Documentation Skills` section (under `Agent Workflow` or a similar heading).
   - For each skill listed there, invoke it with the feature name as the argument (e.g., `/document-feature <feature-name>`).
   - If no `Documentation Skills` section exists in `CLAUDE.md`, fall back to running `/document-feature <feature-name>` only.
   - Commit documentation changes to the PRD branch.
   - Running docs at the end ensures documentation reflects the complete feature, not partial slices.

2. Push the PRD branch to remote:
   ```bash
   git push -u origin prd-<short-name>
   ```

3. Read the PRD issue body to extract:
   - The user stories / feature description
   - Acceptance criteria from each child issue
   - Any user-facing behavior changes

4. Open a PR targeting `staging` (**NEVER `main`**) with a tester-focused description.

   **IMPORTANT:** Write the PR body to a temp file and use `--body-file` instead of inline `--body` with heredocs. Inline heredocs with special characters trigger permission prompts and block autonomous execution.

   **IMPORTANT:** The Write tool requires reading a file before writing it if the file already exists. `/tmp/pr-body.md` may exist from a previous run. Always use the Read tool on `/tmp/pr-body.md` first, then write it — even if Read returns an error (file doesn't exist), you can proceed with Write.

   First, use the Read tool on `/tmp/pr-body.md`, then use the Write tool to create it with the PR description following this template:

   ```markdown
   ## What Changed

   <Plain-language summary of what this feature does from a user/tester perspective.
   No implementation details — focus on behavior.>

   ## How to Test

   Step-by-step instructions for testing each piece of functionality:

   ### <Feature area 1>
   1. Step one...
   2. Step two...
   3. Expected result...

   ### <Feature area 2>
   1. Step one...
   2. Step two...
   3. Expected result...

   ## Things to Watch For

   - <Edge cases or areas that might break>
   - <Permissions or role-specific behavior to verify>
   - <Any UI/UX changes to evaluate>

   ## Parent PRD

   #<prd-number>

   🤖 Generated with [Claude Code](https://claude.com/claude-code)
   ```

   Then create the PR:
   ```bash
   gh pr create --base staging --head prd-<short-name> --title "<PRD title>" --body-file /tmp/pr-body.md
   ```

5. Update labels on the PRD issue:
   ```bash
   gh issue edit <prd-number> --remove-label "in-progress" --remove-label "documenting-prd" --add-label "code-review"
   ```

6. Exit.

## State: Code Review (`code-review`)

### Find the open PR

```bash
gh pr list --head prd-<short-name> --base staging --state open --json number,url --limit 1
```

If no open PR exists, something is wrong — report and exit.

### Check PR status

Run checks in this order. Handle the **first** actionable item found, then exit.

#### 1. Check CI status

```bash
gh pr checks <pr-number>
```

- If checks are still **running/pending**, report status and **exit** — wait for next cycle.
- If the **"Pest + Coverage"** check has **failed**, investigate and fix it (see below).
- Other check failures: report and exit.

#### 2. If Pest + Coverage check failed:

The PR will have a comment starting with **"Lighthouse CI Status"** posted by the CI bot. This comment contains the pass/fail status and lists which tests failed (though not the detailed failure output).

1. Find the CI status comment:
   ```bash
   gh api repos/{owner}/{repo}/issues/<pr-number>/comments \
     --jq '[.[] | select(.body | startswith("## Lighthouse CI Status") or startswith("# Lighthouse CI Status"))] | last | .body'
   ```

2. Read the comment to identify which tests failed.

3. Check out the PRD branch and run the failing tests locally to get the detailed error output:
   ```bash
   ./vendor/bin/pest tests/Feature/Path/To/FailingTest.php
   ```

4. Investigate and fix the failures. Read the relevant source files, understand the issue, and make the fix.

5. Run the full test suite to confirm the fix doesn't break anything:
   ```bash
   ./vendor/bin/pest
   ```

6. Commit and push:
   ```bash
   git add <files>
   git commit -m "fix: address CI test failures for PRD #<prd-number>

   Co-Authored-By: Claude Opus 4.6 (1M context) <noreply@anthropic.com>"
   git push origin prd-<short-name>
   ```

7. Exit. The next cycle will re-check after CI runs on the new push.

#### 3. Check for new CodeRabbit comments

Only reach this step if all CI checks have passed.

**IMPORTANT:** Do NOT use `$()` subshell syntax or `&&` chained commands — these trigger permission prompts. Run each command separately and store results in your context.

Step 1: Get the last commit timestamp (run as a standalone command):
```bash
gh pr view <pr-number> --json commits --jq '.commits[-1].committedDate'
```

Step 2: Check for **any** CodeRabbit comments on the PR (not just after last push — CodeRabbit may have reviewed an earlier commit):
```bash
gh api repos/{owner}/{repo}/pulls/<pr-number>/comments --jq '[.[] | select(.user.login == "coderabbitai[bot]")] | length'
```

Step 3: Also check general PR comments:
```bash
gh api repos/{owner}/{repo}/issues/<pr-number>/comments --jq '[.[] | select(.user.login == "coderabbitai[bot]")] | length'
```

If there are CodeRabbit comments, read them ALL (not just recent ones) and check whether they've already been addressed by commits after the comment was posted. Only comments that haven't been addressed need action.

### If there are new CodeRabbit comments to address:

1. Read the comments to understand what needs to change.
2. Check out the PRD branch and address the feedback.
3. Commit fixes referencing the PRD number.
4. Resolve all addressed review threads using the GraphQL API. First get thread IDs:
   ```bash
   gh api graphql -f query='query { repository(owner: "<owner>", name: "<repo>") { pullRequest(number: <pr-number>) { reviewThreads(first: 50) { nodes { id isResolved } } } } }' --jq '.data.repository.pullRequest.reviewThreads.nodes[] | select(.isResolved == false) | .id'
   ```
   Then resolve each one (run separately per thread ID):
   ```bash
   gh api graphql -f query='mutation { resolveReviewThread(input: {threadId: "<thread-id>"}) { thread { isResolved } } }'
   ```
   This prevents re-processing the same comments and satisfies branch protection rules requiring resolved conversations.
5. Push the changes:
   ```bash
   git push origin prd-<short-name>
   ```
6. Exit. The next cycle will re-check after CodeRabbit reviews the new push.

### If there are NO new comments AND all checks pass:

Merge the PR:

```bash
gh pr merge <pr-number> --squash --delete-branch=false
```

Update the PRD label:

```bash
gh issue edit <prd-number> --remove-label "code-review" --add-label "testing"
```

Exit.

## State: Testing (`testing`)

### Check for test feedback issues

Check for any open test-feedback issues (they may or may not reference the PRD number):

```bash
gh issue list --label "test-feedback" --state open --json number,title
```

### If test-feedback issues exist:

**IMPORTANT:** Steps 1-6 below are ALL part of the same invocation. Do NOT exit after `/fix-test-feedback` returns — you MUST continue through the branch sync, push, PR creation, and label update before exiting. Stopping early leaves the PRD stuck in testing with no PR.

1. Invoke `/fix-test-feedback <prd-number>` to address all feedback issues.
2. After `/fix-test-feedback` completes and returns, **sync the branch onto the latest `staging`** before pushing. Squash merges cause diverged history that shows as "branch is out-of-date" on every subsequent PR — this step prevents that.

   Run each command as a separate Bash call (no `&&` chains):

   ```bash
   git fetch origin staging
   ```

   ```bash
   git log --oneline origin/staging..HEAD
   ```

   Read the output. Note each commit SHA — the list is newest-first, so you will cherry-pick in reverse (oldest first). If the list is empty, skip steps c-e.

   ```bash
   git reset --hard origin/staging
   ```

   Cherry-pick each commit from oldest to newest, one Bash call per commit:
   ```bash
   git cherry-pick <oldest-sha>
   ```
   ```bash
   git cherry-pick <next-sha>
   ```
   (continue for all commits)

3. Push with `--force-with-lease` (required because the remote still has the old diverged history):
   ```bash
   git push --force-with-lease origin prd-<short-name>
   ```

4. Open a new PR to `staging`. Use the Read tool on `/tmp/pr-body.md` first (it may exist from a previous run), then use the Write tool to write the PR body (avoids heredoc permission issues). Then create the PR:
   ```bash
   gh pr create --base staging --head prd-<short-name> --title "fix: address test feedback for <PRD title>" --body-file /tmp/pr-body.md
   ```

   The body should follow this template:
   ```markdown
   ## What Changed

   Fixes based on tester feedback for PRD #<prd-number>.

   ## Issues Addressed

   - #<feedback-issue-1>: <brief description of what was fixed>
   - #<feedback-issue-2>: <brief description of what was fixed>

   ## How to Re-Test

   <Step-by-step instructions focused on verifying the fixes.
   Reference the original test-feedback issues for context.>

   ## Things to Watch For

   - <Anything the fixes might have affected beyond the reported issues>

   🤖 Generated with [Claude Code](https://claude.com/claude-code)
   ```
5. Update labels:
   ```bash
   gh issue edit <prd-number> --remove-label "testing" --add-label "code-review"
   ```
6. Exit.

### If NO test-feedback issues exist:

Nothing to do. Report "PRD #X is in testing with no feedback issues" and exit.

## Escalation: HITL Issues

When something important needs human attention but doesn't block the current step, create a GitHub issue to flag it. Use this sparingly — only for things that genuinely need a human decision or awareness.

```bash
gh issue create --title "<short description>" --body "<context and details>" --label "needs-hitl" --assignee "jonzenor"
```

**When to use:**
- A CodeRabbit suggestion reveals a real architectural concern that's out of scope for the current PR
- A pre-existing bug is discovered that's unrelated to the PRD but should be tracked
- A design decision needs human input before future work can proceed
- Something feels off but isn't clearly a bug — needs human eyes

**When NOT to use:**
- CodeRabbit suggestions you've decided not to address (that's normal review triage)
- Test failures (fix them or escalate via `Refinement Needed` label)
- Anything that blocks the current step (handle it or ask the user directly)

## Cycle Summary Comment

Before every exit where an action was taken, post a comment to the PRD issue summarizing what happened this cycle. These comments are the **audit trail** — someone reading the issue history should understand exactly what happened without opening the diff.

```bash
gh issue comment <prd-number> --body "🤖 Auto-process cycle: ..."
```

**Post a comment when:**
- An issue was implemented via `/work-issue`
- Documentation was written and a PR was opened
- CI failures were investigated and fixed
- CodeRabbit feedback was addressed and pushed
- The PR was merged and the PRD transitioned to testing
- Test feedback was fixed and a new PR was opened

**Do NOT post a comment when:**
- No active PRD was found
- The PRD is in testing with no feedback issues (nothing to do)
- CI checks are still running/pending (just reporting status, no action taken)

**Comment length and detail:** Aim for 3–8 bullet points or sentences — enough context to understand what happened without reading the diff. A one-liner is never enough.

**For CodeRabbit rounds**, always list each comment individually with whether it was fixed or skipped and why:
```
🤖 Auto-process cycle: Addressed CodeRabbit round 2 (2 of 3 comments fixed):
- **Fixed** — Line 90 stale comment "No staff_rank set → crew": updated to "CrewMember < Officer"
  because we added `staff_rank` to the fixture last cycle but forgot to update the comment
- **Fixed** — Line 196 same stale comment pattern, same fix
- **Not fixed** — Suggestion to add abort-on-syncstart-failure: intentional design decision to
  warn-and-continue so a transient syncstart hiccup doesn't block the entire repair run
```

**For CI failures**, name the failing test(s) and describe the root cause and fix:
```
🤖 Auto-process cycle: Fixed CI failure — SyncMinecraftAccountTest "eligible staff user records
staff position set activity" was failing because the test created a user with staff_department
but no staff_rank; after our null-guard fix, that correctly returns 'none' and logs
minecraft_staff_position_removed. Added staff_rank: CrewMember to the fixture to restore the
intended scenario.
```

**For implementations**, summarize what was built:
```
🤖 Auto-process cycle: Implemented issue #342 — added MeetingPayout model and migration
(amount, user_id, meeting_id, paid_at), MeetingPayoutFactory, and SyncMeetingPayouts action
that calculates lumen payouts from check-in attendance. Tests cover eligibility rules.
```

**For merges**, confirm what passed:
```
🤖 Auto-process cycle: PR #369 passed CI (Pest + Coverage, Pint) and CodeRabbit review
(2 rounds, 5 total comments addressed) — merged to staging, PRD #365 transitioned to testing.
```

---

## Safety Rules

- **NEVER merge to or touch `main`.** All PRs target `staging`.
- **One step per invocation.** Do one thing, then exit.
- **Never skip tests.** All code changes must pass `./vendor/bin/pest`.
- **Respect `in-progress` labels.** Don't claim issues another agent is working on.
- **Push is allowed** in this skill (unlike `/work-issue`) because this is the orchestrator managing the full lifecycle.
- **Fully autonomous.** Do not prompt for confirmation. Do not present implementation plans and wait. Just execute.
