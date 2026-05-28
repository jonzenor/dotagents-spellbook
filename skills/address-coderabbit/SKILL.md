---
name: address-coderabbit
description: Walk through every unresolved CodeRabbit review thread on a PR, critically evaluate the feedback, apply real fixes, reply to each thread with the action taken (or a clear reason for dismissal), mark each thread as resolved, then commit and push the changes.
argument-hint: "<pr-number>"
---

# Address CodeRabbit Feedback

This skill cleans up the unresolved CodeRabbit review threads on a GitHub PR. It is **not** an auto-accept loop — CodeRabbit produces a lot of noise alongside real findings, and the value of this skill is in the critical evaluation step. Apply fixes that catch real bugs, security issues, or policy violations; dismiss style nits, premature abstractions, defensive-coding suggestions for impossible cases, and anything that conflicts with project conventions in `CLAUDE.md` / `.ai/context/apex-conventions.md`.

## Argument

A single GitHub PR number, with or without the `#` prefix (e.g. `5304` or `#5304`).

If no PR number is provided, ask the user for one. If you are on a branch with an open PR, `gh pr view --json number` returns it.

## Step 1: Gather context

1. Resolve owner/repo from `gh repo view --json owner,name --jq '.owner.login + "/" + .name'`.
2. Confirm the PR exists and capture its base branch + head ref:

   ```bash
   gh pr view <pr-number> --json number,title,baseRefName,headRefName,state
   ```

3. Make sure you are on the PR's head branch locally. If not, ask the user to switch — do not silently check out a different branch.
4. Make sure the working tree is clean (`git status`). Any uncommitted changes from before the skill ran can confuse the commit step later. If the tree isn't clean, ask the user how to proceed.

## Step 2: Pull all unresolved CodeRabbit review threads

CodeRabbit publishes its findings as GitHub **review threads** (line-anchored comments on a PR review). Each thread has an `isResolved` flag, a `path`, a `line`, and one or more comments. Replies append to the same thread. The thread is what gets resolved, not individual comments.

Fetch every unresolved thread that contains at least one comment authored by CodeRabbit. CodeRabbit's login is `coderabbitai` (the GitHub App's bot — the comment author will be `coderabbitai[bot]` in REST responses and `coderabbitai` in some GraphQL contexts; match by the prefix `coderabbitai`).

Use this GraphQL query — paginate if `reviewThreads.pageInfo.hasNextPage` is true:

```bash
gh api graphql -f query='
query($owner: String!, $repo: String!, $pr: Int!, $after: String) {
  repository(owner: $owner, name: $repo) {
    pullRequest(number: $pr) {
      reviewThreads(first: 50, after: $after) {
        pageInfo { hasNextPage endCursor }
        nodes {
          id
          isResolved
          isOutdated
          path
          line
          originalLine
          comments(first: 50) {
            nodes {
              id
              databaseId
              author { login }
              body
              createdAt
            }
          }
        }
      }
    }
  }
}' -F owner=<owner> -F repo=<repo> -F pr=<pr-number>
```

Filter the results to threads where:
- `isResolved` is `false`, AND
- at least one `comments.nodes[].author.login` matches `coderabbitai` (case-insensitive, with or without `[bot]` suffix).

`isOutdated` threads (the line they reference no longer exists in the current diff) are usually noise — read them, but lean toward resolving as outdated unless they describe a structural issue still present elsewhere.

## Step 3: Critically evaluate each thread

Read each thread end-to-end before deciding. CodeRabbit comments fall into roughly these buckets — your decision should reflect which bucket the comment belongs in:

**Usually fix**
- Real correctness bugs (null deref, off-by-one, wrong field reference, wrong operator)
- SOQL injection, FLS/CRUD misses, sharing-mode violations
- Catching exceptions that should propagate (or missing catch where the operation legitimately can fail)
- Test gaps the PRD or CLAUDE.md explicitly requires
- API version mismatches between `*.cls` and `*-meta.xml`
- Hardcoded record IDs, `SeeAllData=true`, direct DML/SOQL where UoW/Selector is required by `.ai/context/apex-conventions.md`
- Trigger logic outside of `TriggerService` subclasses

**Usually dismiss with a reason**
- Stylistic preferences that don't match this codebase (e.g. "consider extracting a helper" when the helper has one caller; "add JavaDoc" beyond the project's header convention)
- Speculative defensive coding for cases that can't happen (`if (uow == null) throw new IllegalStateException(...)` when the caller is in this file and never passes null)
- Suggestions to add tests for platform behavior (SOQL filters returning expected rows, picklist enforcement, framework guarantees) — see the "Don't test the platform" rule in `.ai/context/apex-conventions.md`
- Premature abstraction ("extract an interface here") when there is no second implementation in sight
- Renames that thrash existing identifiers without behavior change
- Suggestions to "use ApplicationLogger" when the file already uses `AutomationLogger` correctly, or vice versa
- Suggestions that contradict `CLAUDE.md` / `.ai/context/apex-conventions.md`

**Always think twice before dismissing**
- Anything tagged as a security finding
- Anything in an authentication, authorization, JIT, or token path
- Anything involving currency, donation amounts, or financial fields
- Anything that touches a managed package's namespace (`npsp__`, `ChargentOrders__`, `pmdm__`, `abacus__`) — those are easy to get subtly wrong

For each thread, decide one of: **fix**, **dismiss-with-reason**, **defer-to-user**. Defer-to-user is for findings that are real but require a product decision you can't make (e.g. "this changes the public contract of the service — is that intended?"). Defer-to-user threads stay unresolved; you ask the user before resolving.

## Step 4: Apply fixes

For threads in the **fix** bucket:

1. Apply the fix using `Edit` / `Write` — minimal, focused changes.
2. Do not bundle unrelated changes into a single fix. One thread → one logical change.
3. After all fixes are applied, run any obvious local checks (e.g. `git diff` review). If the project has a fast local validator, run it. Don't run full test suites yet — that's for the commit step.
4. If a fix turns out to be larger than expected (touches >3 files, requires new tests, changes a public API), pause and surface it to the user before continuing.

## Step 5: Reply to each thread and resolve it

For each thread, post one reply that states the action taken, then resolve the thread.

**Reply** — use the REST API. The `comment-id` is the `databaseId` of the first CodeRabbit comment in the thread:

```bash
gh api -X POST \
  repos/<owner>/<repo>/pulls/<pr-number>/comments/<comment-id>/replies \
  -f body="<reply text>"
```

**Resolve** — use the GraphQL `resolveReviewThread` mutation with the thread `id`:

```bash
gh api graphql -f query='
mutation($threadId: ID!) {
  resolveReviewThread(input: {threadId: $threadId}) {
    thread { id isResolved }
  }
}' -F threadId=<thread-node-id>
```

**Reply wording**

Reply bodies should be short and concrete. State *what* and *why*, not "thanks for the feedback." Examples:

- Fix applied: `Fixed in <commit-sha-or-"next commit">. Replaced inline SOQL with new Selector method per .ai/context/apex-conventions.md §SOQL.`
- Dismissed: `Not changing. The PRD explicitly treats missing claims as "consent not given" (see #5303 Implementation Decisions), so the early-return guard would defeat the documented behavior.`
- Dismissed (style): `Not changing. The codebase convention in CLAUDE.md is field-based DI for handler-layer selectors; switching this one site to factory wiring would be inconsistent without a wholesale migration.`
- Dismissed (test-the-platform): `Not changing. This would assert SOQL's WHERE-clause behavior, which is platform-guaranteed (see .ai/context/apex-conventions.md "Don't test the platform").`

Do not write apologetic or hedging replies. The reviewer is a bot; the reply is documentation for the next human to read the thread.

## Step 6: Handle defer-to-user threads

After processing all decidable threads, list the **defer-to-user** threads to the user as a numbered list with: file:line, the CodeRabbit excerpt, and your assessment of what makes it a product decision rather than a clear fix-or-dismiss. Wait for the user's call before continuing. Do not resolve these threads on the user's behalf.

## Step 7: Commit and push

Once every thread has been handled (resolved or explicitly deferred):

1. Confirm the working tree has the expected fixes staged. `git status` and `git diff --stat`.
2. Stage specific files (no `git add -A`).
3. Commit with a HEREDOC message. Use the conventional-commits prefix from the `commit` skill — `fix(...)` for bug-style fixes, `refactor(...)` for code-quality fixes, `style(...)` for formatting, `test(...)` for test-only changes. If several thread fixes don't share a clean type, default to `fix(coderabbit): address PR #<n> review feedback`.

   ```bash
   git commit -m "$(cat <<'EOF'
   fix(<scope>): address CodeRabbit review on PR #<n>

   <one or two sentence summary of what changed>

   <Story-ID if present, extracted from branch name or PR title>
   EOF
   )"
   ```

4. Push to the remote. The branch already has an upstream from earlier work, so a plain `git push` is correct.
5. Never force-push, never amend, never `--no-verify` unless the user explicitly asks.

If the commit's pre-commit hook fails (Prettier, lint, etc.), fix the underlying issue, re-stage, and create a **new** commit. Do not `--amend` — that hides the hook-failure recovery.

## Step 8: Report back

End with a short summary to the user:

- Number of threads processed
- Number fixed, number dismissed, number deferred
- Commit SHA pushed (if any)
- Anything that warrants their attention (e.g. "CodeRabbit flagged the SMS picklist value choice — I dismissed because the PRD specifies 'Not Given', but you may want to confirm with the marketing compliance team")

## Notes on the CodeRabbit CLI

The `commit` skill mentions a local `coderabbit` CLI. That tool runs CodeRabbit against your **uncommitted** local diff. It is **not** the right tool for this skill — this skill operates on the PR's existing review threads, which only exist on the server side. Use `gh api` (REST and GraphQL) as described above.
