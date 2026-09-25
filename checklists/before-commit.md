# Before Commit

Evidence first, staging second.

## Diff Review

- [ ] `git diff` read hunk by hunk; every hunk traces to the requirement.
- [ ] No debug leftovers: `print`/`console.log`/`console.log(x)`/`debugger`,
      temporary flags, `XXX`, `TODO: fix later`, commented-out code.
- [ ] No unrelated files: other task's changes, editor backups (`.orig`, `.bak`,
      `*.tmp`), accidentally added binaries or generated output.
- [ ] No unrelated reformatting — the diff shows the change, not whitespace noise.
- [ ] No secrets, keys, tokens, credentials, or internal URLs in code, config, or tests.

## Self-Code-Review

- [ ] Read as if a hostile reviewer wrote it (`references/code-review.md`).
- [ ] Names are accurate; no comments describing what the code plainly does.
- [ ] Error paths handled or deliberately propagated — nothing fails silently.
- [ ] No dead code, no unused imports, no unused config/options.
- [ ] Nothing was refactored that the task did not require.
- [ ] If I found a real defect in my own work, I fixed it and reported it.

## Verification (must have real output)

- [ ] Targeted test for the change runs and passes.
- [ ] Regression test added (bugfix) or behavior test added (feature) — and it
      **fails without the fix** (or at least was confirmed to fail before the fix).
- [ ] Full test suite passes, or failures are pre-existing and explained.
- [ ] Linter and formatter pass with the project's own commands.
- [ ] Build/typecheck passes.
- [ ] Negative case verified: the original failure is actually gone.

## Commit Craft

- [ ] One logical change only — otherwise split.
- [ ] Message: imperative subject ≤72 chars stating what/why; body carries rationale
      and trade-offs.
- [ ] Only intended files staged (`git status` inspected).
- [ ] Pre-commit hooks passed; not bypassed.
- [ ] Nothing is force-pushed or history-rewritten on a shared branch.

## Report

- [ ] Evidence (command + output) is ready to show, not just claimed.
