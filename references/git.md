# Git Discipline

History is a tool for the next reader. Keep it useful, safe, and reversible.

## Commits

- One logical change per commit. A commit that fixes a bug AND reformats the file
  cannot be reviewed or reverted cleanly.
- Subject: imperative, specific, ≤ ~72 chars — what changed, not "update file".
  `Fix race in order sync by locking before read` beats `fix bug`.
- Body: the *why*, the trade-off, the alternatives rejected. Link the issue.
- Never commit: secrets, credentials, generated junk, `node_modules`, local config,
  lockfile churn you didn't intend, commented-out code.
- Never `--no-verify` to skip hooks; fix the reason they fired.

## Before You Stage

Run `checklists/before-commit.md`. Specifically confirm:

- `git diff` reviewed hunk by hunk — no stray debug prints, `console.log`,
  commented code, `.orig`/`.bak`, or files from another task.
- Only intended files staged. Review `git status` for untracked surprises.
- The linter/formatter the project uses has been run (don't reformat unrelated files).

## Branches And Scope

- Branch names describe intent: `fix/invoice-rounding`, not `dev2`.
- One task per branch. If unrelated work crept in, split it before the PR.
- Branch from the correct base; rebase or merge per the team's convention — learn
  which this repo uses rather than guessing.

## Rewriting History

- **Never** force-push a shared branch, and never rewrite a branch someone already
  reviewed without telling them.
- Rebase your own unpushed commits freely; squash noisy WIP before review.
- Do not amend a pushed commit unless the team allows it.

## Reverting And Recovering

- Prefer `git revert` (a new commit that undoes) on any shared history.
- Before any destructive operation (`reset --hard`, `clean -fd`, forced push), note
  the current HEAD (`git rev-parse HEAD`) so recovery is possible.
- "It works on my machine" → check `git status`, stashes, and whether the fix is
  actually committed.

## Review-Ready Diffs

- Keep diffs small enough to review in minutes.
- Avoid mixing whitespace/reformat churn with logic changes — it buries the real diff
  (`git diff -w` helps, but don't rely on reviewers knowing that).
- If a change is intentionally large (migration, rename), say so up front and split
  it into mechanical + behavioral commits.

## What To Report To The User

When your work involves git operations, state the exact commands you ran and their
result. Do not commit, push, tag, or open a PR unless explicitly asked.
