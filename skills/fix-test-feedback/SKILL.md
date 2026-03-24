---
name: fix-test-feedback
description: Address open test-feedback issues by making small fixes on the PRD branch. Escalates complex issues with a "Refinement Needed" label. Use when user wants to fix test feedback, address tester-reported bugs, or when invoked by /auto-process-prd during the testing phase.
---

# Fix Test Feedback

Address all open `test-feedback` issues by making small, targeted fixes on the current PRD branch. Invoked by `/auto-process-prd` during the testing phase, or manually.

**CRITICAL: NEVER touch the `main` branch.**

## Process

### 1. Receive context

The caller (usually `/auto-process-prd`) provides a PRD issue number. Fetch the PRD for context:

```bash
gh issue view <prd-number>
```

Note the PRD branch name and what the PRD's changes involve — this helps assess whether feedback is related.

### 2. Gather all test-feedback issues

```bash
gh issue list --label "test-feedback" --state open --limit 50 --json number,title,body
```

If none exist, report "No test-feedback issues found" and exit.

### 3. Triage each issue

For each test-feedback issue, read the full issue body:

```bash
gh issue view <number>
```

Assess the issue against these criteria:

**Fix it** if:
- It's a small, contained change (single file or a few lines across 1-2 files)
- The bug is clear and reproducible from the description
- The fix doesn't require architectural or structural changes

**Escalate it** if:
- It touches multiple files or requires structural changes
- It's ambiguous — the problem isn't clear enough to confidently fix
- It reads like a feature request rather than a bug
- It requires a design decision or user input

### 4. Escalate complex issues

For issues that don't qualify as small fixes:

```bash
gh issue edit <number> --remove-label "test-feedback" --add-label "Refinement Needed"
gh issue comment <number> --body "This issue requires more than a small fix — flagged for refinement. It may involve structural changes or needs clarification before implementation."
```

Move on to the next issue.

### 5. Fix small issues

For each fixable issue:

1. **Claim it:**
   ```bash
   gh issue edit <number> --add-label "in-progress"
   ```

2. **Ensure you're on the PRD branch:**
   ```bash
   git checkout prd/<short-name>
   git pull origin prd/<short-name>
   ```

3. **Read the relevant code** — understand the area before changing it.

4. **Make the fix** — keep it minimal and focused. Don't refactor surrounding code.

5. **Run tests:**
   ```bash
   ./vendor/bin/pest
   ```

6. **Commit** with a reference to close the issue:
   ```bash
   git commit -m "fix: <description of what was fixed>

   Fixes #<number>

   Co-Authored-By: Claude Opus 4.6 (1M context) <noreply@anthropic.com>"
   ```

7. **Close the issue:**
   ```bash
   gh issue edit <number> --remove-label "in-progress" --remove-label "test-feedback"
   gh issue close <number> --reason completed
   ```

8. Move on to the next issue.

### 6. Summary

After processing all issues, report:

```
Test Feedback Summary
---------------------
Fixed:
  - #101: Button alignment on dashboard
  - #103: Typo in welcome message

Escalated (Refinement Needed):
  - #102: Rework notification preferences UI
  - #104: Add bulk export feature

No action needed:
  (none)
```

## Safety Rules

- **NEVER touch `main`.** All work happens on the PRD branch.
- **One commit per issue.** Each fix gets its own commit referencing the issue number.
- **Don't push.** The caller (`/auto-process-prd`) handles pushing and PR creation.
- **Keep fixes minimal.** If you're touching more than 2 files, reconsider whether this should be escalated.
- **Run tests after every fix.** Never move on with failing tests.
- **When in doubt, escalate.** A wrongly-escalated issue costs less than a bad fix.
