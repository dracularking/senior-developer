# DRY — Don't Repeat Yourself

**D**on't **R**epeat **Y**ourself: *every piece of knowledge has a single,
authoritative representation* — not "never type the same string twice".

DRY is about duplicated **knowledge/decision**, not duplicated **text**.

## What Counts As A Duplication Worth Removing

- The same validation rule enforced in two places (they will drift).
- The same business rule encoded in an app and a script.
- The same magic constant/format/date-layout scattered across files.
- Copy-pasted logic where a bug fix must be applied in N places.
- Parallel type/schema definitions that must stay in sync by hand.

## What Does NOT Count

- Two similar-looking functions with different reasons to change and different
  rates of change. Forcing them together creates **coupling** — the wrong kind of
  sharing.
- Independent test fixtures that read better when explicit.
- Two branches of an `if` that happen to share a line today.
- Standard-library or framework behavior you "duplicated" by writing 3 lines.

> Duplication is cheaper than the wrong abstraction. — Sandi Metz

## Extraction Test

Only unify when ALL hold:

1. The two pieces express the same decision (not merely look alike).
2. They will change together, forever.
3. The unified name is not vaguer than the originals (`process()` is not a name).
4. Callers remain readable after the merge.

If you fail any, leave the duplication and add a comment only if drift is a real risk.

## How To Remove It Well

- Extract at the right level: the *rule*, not the *symptom*.
  - Bad: `do_thing_common_v2()` wrapping two unrelated paths.
  - Good: one `validate_email()` called by both paths.
- Parameterize data, don't fork code. Differences become arguments, not copies.
- Single source of truth: constants, schemas, and config live in one place and are
  imported/derived, never re-typed.
- Generated code counts as one source if generation is in the pipeline and
  reproducible.

## DRY vs KISS vs YAGNI

| Situation | Rule |
|---|---|
| Same rule in 2 places, changes together | DRY — extract |
| 2 similar functions, evolve independently | KISS — leave them |
| Building a shared layer for a future 3rd caller | YAGNI — don't |

## Self-check

- If this rule changes tomorrow, how many files do I edit?
- Am I extracting a *concept* or just shortening lines?
- Does the new shared name still describe what it does?
