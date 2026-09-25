# Before Saying "Done"

Claim nothing you have not observed. This is the last gate.

## Requirement Closure

- [ ] Each acceptance criterion has a corresponding observable result.
- [ ] The original request is fully addressed — no silent scope cuts.
- [ ] Anything intentionally not done is listed explicitly to the user.
- [ ] Any assumption I had to make is stated (not buried).

## Evidence

- [ ] Tests run — the actual command and its real output, not "should pass".
- [ ] The specific new/changed behavior is demonstrated, not just the suite.
- [ ] The original bug reproduction now succeeds (bugfix cases).
- [ ] Lint/format/typecheck run with the project's own commands.
- [ ] Manual verification where automation can't reach (UI, CLI interaction, network).
- [ ] If something could not be verified here (no creds, no hardware, no network),
      I say that plainly instead of implying success.

## Robustness

- [ ] Failure paths tested: bad input, missing data, permission denied, timeout.
- [ ] No new warnings, errors, or log noise introduced.
- [ ] No performance cliff added for the common path (N+1 queries, sync I/O in a loop).
- [ ] Security check complete for touched surfaces (`references/security.md`).
- [ ] Concurrency/partial-failure reviewed if state is written.

## Code Health

- [ ] Final `git diff` reviewed in full; only intended changes present.
- [ ] Tests are deterministic — the suite passes twice in a row.
- [ ] No new dead code, no orphaned config, no stale docs left behind.
- [ ] Docs/comments updated if behavior or public API changed.
- [ ] Nothing outside the task's scope was modified.

## Handoff

- [ ] Reported: what changed, why, how it was verified (with output).
- [ ] Reported: what was deliberately not done, and residual risks.
- [ ] No claim of completion without evidence.
- [ ] If I discovered mid-task that my approach was wrong, I said so and corrected
      course rather than defending the original plan.
- [ ] No flattery, no padding — the diff and the output carry the message.
