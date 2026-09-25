# Debugging — Root Cause Only

A fix that removes the symptom while preserving the cause is a new bug.
Load whenever something is broken, flaky, or behaving inexplicably.

## The Loop

1. **Reproduce.** Get a minimal, repeatable case. If you cannot reproduce it, you
   cannot verify the fix. Record the exact command, input, and observed output.
   Flaky? Vary timing, order, concurrency, data until it becomes reliable.
2. **Observe, don't assume.** Instrument: logs, a debugger, a failing assertion that
   prints actual state. Locate the *first* point where reality diverges from
   expectation — not the last one that complains.
3. **Hypothesize.** State a falsifiable cause: "the cache returns the stale entry
   because invalidation keys on id but write keys on sku." Not "maybe the cache".
4. **Test the hypothesis.** One experiment per hypothesis. Confirm by making the
   predicted change and watching the effect (e.g. disabling the cache removes the bug).
5. **Fix at the root.** Change the mechanism that produced the failure.
6. **Prove it.** Repro now passes; full suite still passes; add the regression test.
7. **Ask why it escaped.** Missing test, missing type, missing validation, missing
   log line? Cheap fix here prevents the recurrence.

## First Questions

- What changed? `git log`/`git diff` on the last commits touching this path is the
  fastest lead. Recently-added code is the prime suspect.
- Does it fail everywhere or only in one environment/config/data set? That difference
  *is* the clue.
- What is the actual error message, verbatim? Read it fully — no paraphrasing.
- Can you make it fail earlier and louder? The earliest reliable failure point is
  usually the true one.

## Anti-Patterns (Symptom Patching)

Do not ship any of these as "the fix":

- Wrapping in `try/except` / swallowing the error so the crash disappears.
- Adding a retry around a deterministic failure.
- Adding a null/undefined check where a null means an upstream invariant broke.
- Broadening a type, widening a timeout, or bumping a limit to make it pass.
- Patching the call site to work around a broken callee.
- "It stopped failing" after changing two things at once — you learned nothing.
- Deleting or weakening the failing test.

Each is acceptable **only** if explicitly presented as a temporary mitigation with
the root-cause work identified, and the user chose it.

## Concurrency And State

Suspect shared mutable state, ordering, and time when the failure is intermittent:
cache coherence, race between read and write, missing await, unflushed buffer,
process restart losing in-memory state, timezone/DST boundaries.

## When Stuck

- Explain the system out loud (rubber duck), line by line, and check each assumption
  against the source.
- Binary-search the input or the commit range.
- Reduce to a minimal reproduction — usually reveals the cause by itself.
- Check the dependency's actual source/version rather than its documentation.
- Still stuck? Say so. Report what you ruled out, the evidence, and the next two
  experiments. Do not guess and ship.

## Definition Of Fixed

- Root cause identified and stated in one sentence.
- Regression test added that fails without the fix.
- Original reproduction now succeeds.
- No unrelated behavior changed.
