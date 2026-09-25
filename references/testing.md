# Testing

Tests are the evidence that a change works and the safety net that lets the next
change be cheap. Load when writing new behavior, fixing a bug, or touching existing
tests.

## Test First Where It Matters

- New feature: write the failing test before the code (red), then make it pass
  (green), then clean up (refactor). If the design is unclear, a test written first
  forces you to specify the interface.
- Bugfix: the regression test comes *before* the fix. Confirm it fails for the right
  reason, then fix, then confirm it passes. A fix with no reproducing test is a bet.
- Trivial changes (comment, typo, literal already covered): a test adds nothing.
  Say so instead of writing theater.

## What To Test

Test **observable behavior through the public surface**, not internals.

- Cover the happy path, the meaningful edge cases, and the failure mode the
  requirement calls out.
- Assert on outcomes the user/caller cares about. Asserting a private field or a
  call order makes the test a change barrier.
- One behavior per test, named for the behavior: `test_expired_token_is_rejected`,
  not `test_token_2`.
- If you cannot test it, that is a design problem: hidden globals, concrete
  infrastructure imports, `now()` read directly. Inject the seam (see solid.md DIP).

## Test Quality

| Bad | Good |
|---|---|
| Asserts nothing | Asserts a specific, meaningful outcome |
| Depends on execution order or shared mutable state | Independent; builds its own fixtures |
| Sleeps to "wait" for async work | Awaits a condition or a signal |
| Tests the mock's configuration | Tests behavior with a real or faithful double |
| Silent on failure | Fails with a message that names the actual mismatch |
| 200-line test | One scenario, obvious arrange-act-assert |

- **Determinism**: no real clock, no real network, no random ports if avoidable.
  Flaky tests are worse than no tests — they train people to ignore red.
- **Fast feedback**: unit tests in milliseconds; reserve integration/e2e for what
  genuinely needs the wiring.
- **Triangulate**: one test that would fail if the logic were wrong, not one that
  passes for any output.

## Scope Discipline

- Test the layer where the bug lives. Don't write an e2e test for a pure function.
- Do not test third-party behavior you don't own.
- Do not assert on incidental text (log wording, exact timestamps) unless the text
  *is* the feature.

## Repairing Tests

When a test fails after your change:

1. Determine whether the *code* or the *expectation* is wrong. Don't reflexively
   edit the assertion — that converts a regression into a blessing.
2. If the expectation is legitimately outdated, update it and explain why in the
   commit message.
3. If the test was passing for the wrong reason, fix the test's quality too.

## Evidence Standard

"Tests pass" is only a claim. Report the actual command and its output
(`pytest -q`, `go test ./...`, `npm test`). If some tests were skipped or are
flaky, disclose it.
