---
name: pr-reviewer
description: Perform a thorough code review of a pull request — checks correctness, security, test coverage, and style. Use when the user says "review this PR", "review PR #N", "do a code review", or asks for feedback on a pull request.
---

# PR Reviewer

Reviews a pull request end-to-end and produces structured, actionable feedback.

## Input

The user provides one of:
- **PR number** — e.g. "review PR #42"
- **PR URL** — e.g. "review https://github.com/org/repo/pull/42"
- **Current branch** (implicit) — if no PR is specified, infer from the current branch

## Algorithm

### 1. Locate the PR

- If a PR number or URL was given, use `gh pr view <number>` to fetch metadata.
- Otherwise, run `gh pr view` (no args) to find the PR for the current branch.
- If no PR is found, tell the user and stop.

### 2. Gather context

Run these commands in parallel:
```
gh pr view <number> --json title,body,author,baseRefName,headRefName,additions,deletions,changedFiles,commits,labels,reviewRequests
gh pr diff <number>
gh pr checks <number>
```
Also read any linked issues from the PR description, and run `git log <base>..<head> --oneline` for commit history.

### 3. Understand the intent

Read the PR title, description, and linked issues to understand **what problem is being solved** and **why**. This frames every subsequent comment: a change that looks odd in isolation may be intentional given the stated goal.

### 4. Review the diff

Read the full diff and evaluate each changed file for:

| Category | What to check |
|----------|--------------|
| **Correctness** | Logic errors, off-by-ones, incorrect assumptions, unhandled edge cases, race conditions |
| **Security** | Injection vulnerabilities (SQL, command, XSS), auth/authz gaps, secrets in code, insecure deserialization, OWASP Top 10 |
| **Tests** | New behavior is tested; edge cases and error paths are covered; tests are not just happy-path |
| **Readability** | Names are clear, functions are focused, abstractions are appropriate, comments explain *why* not *what* |
| **Consistency** | Matches existing patterns, naming conventions, and style in the repo |
| **Error handling** | Errors are surfaced correctly, not silently swallowed |
| **Performance** | No obvious N+1 queries, unnecessary allocations, or blocking calls in hot paths |
| **Breaking changes** | API contracts, database migrations, config schema changes are backwards-compatible or clearly flagged |

### 5. Check CI status

Report whether all checks pass. For failing checks, summarize what failed.

### 6. Produce the review

Structure the output as follows:

---

## PR Review: <title> (#<number>)

**Author:** <author> | **Base:** `<base>` ← `<head>` | **Size:** +<additions>/-<deletions> across <N> files

### Summary
2–4 sentences: what the PR does, whether the approach is sound, and the overall assessment (Approve / Request Changes / Needs Discussion).

### Issues

List findings grouped by severity. Omit sections with no findings.

#### Blocking
> Must be resolved before merge. Correctness bugs, security vulnerabilities, missing critical tests.

- `path/to/file.py:42` — **[Bug]** Description of the problem and why it matters. Suggested fix or direction.

#### Non-blocking
> Worth fixing now but not a merge blocker. Code quality, style, minor edge cases.

- `path/to/file.py:88` — **[Style]** Description.

#### Nits
> Optional cleanup. Small naming, formatting, or comment improvements.

- `path/to/file.go:15` — Consider renaming `x` to `requestTimeout` for clarity.

### Praise
Call out one or two things done particularly well. Skip if there is nothing notable.

### CI Status
- ✅ / ❌ — summary of check results

---

## Output rules

- Be specific: include file path and line number for every finding.
- Be constructive: explain *why* something is a problem, not just *that* it is.
- Don't invent issues — only flag something if you can clearly articulate the problem.
- Keep nits genuinely optional; don't pad the review.
- Do not auto-submit the review via `gh pr review --submit` unless the user explicitly asks.
