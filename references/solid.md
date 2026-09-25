# SOLID (Applied, Not Preached)

Five constraints on module shape. Use them to *diagnose* pain, not to preempt it.
Load when a class/module has too many reasons to change, or a change ripples
unexpectedly across files.

## S — Single Responsibility

One module, one reason to change. Test: if two different stakeholders would request
changes to this file, it holds two responsibilities.

- A class that formats dates *and* queries the database has two reasons to change.
- Split along the axis of *who asks for the change*, not along the axis of size.

## O — Open/Closed

Extend behavior without modifying stable code.

- Prefer adding a new branch/object over editing a well-tested core.
- Signals you're violating it: `switch`/`if-elif` chains on a type that keep growing;
  every new feature touches the same 5 files.
- Do NOT build a plugin system to satisfy this preemptively. Two occurrences of the
  pattern are enough evidence.

## L — Liskov Substitution

A subtype must be usable wherever its base type is expected, without callers
needing to check which one they got.

- Violations: overriding a method to throw `UnsupportedOperation`, weakening
  preconditions, returning null when the base returns a value.
- Fix: often the hierarchy is wrong. Prefer composition, or model the difference
  explicitly (e.g. a `ReadOnly` interface) instead of faking inheritance.

## I — Interface Segregation

Don't force clients to depend on methods they don't use.

- Fat interfaces that every implementer stubs out with `raise NotImplementedError`
  are the tell.
- Split by client need. Two small interfaces beat one medium one.

## D — Dependency Inversion

High-level policy must not depend on low-level detail. Both depend on abstractions.

- Business logic importing `requests`/`fs`/`datetime.now()` directly is a hidden
  dependency that blocks testing.
- Inject the boundary: pass a client, a clock, a repository. Let the caller choose
  the concrete implementation.
- Practical form: `create_order(repo, clock, notifier)` beats a module that reaches
  out to a global DB connection.

## How To Use SOLID In Practice

1. Do not refactor "to be SOLID". Wait for a real change that hurts.
2. Name the pain concretely: "adding a payment provider required editing 6 files."
3. Make the smallest structural change that removes that specific pain.
4. Verify with tests before and after.

## Smells That Map To SOLID

| Smell | Offending principle |
|---|---|
| Class with `and` in its comment/purpose | SRP |
| Growing `switch` over a closed enum of types | OCP |
| `if isinstance(...)` scattered across callers | LSP / OCP |
| Interface with 20 methods, implementers use 4 | ISP |
| Business module imports a concrete driver/SDK | DIP |

Related: `references/kiss.md` (do not over-structure), `references/yagni.md`
(do not build the extension points early).
