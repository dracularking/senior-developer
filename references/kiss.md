# KISS — Keep It Simple, Stupid

Simplest thing that works *today*. Complexity is a loan with interest; take it only
when the requirement pays it back.

## What "Simple" Means

- Fewer moving parts: fewer modules, fewer configs, fewer abstractions, fewer deps.
- Readable control flow: straight-line code beats clever code.
- Obvious to a reader who knows the language but not the project.
- Cheap to delete. Simple code has no tangle of callers.

## Decision Procedure

Before adding any structure, ask in order:

1. Does the current code fail a *stated* requirement? If no → stop.
2. Can this be solved by naming things better or extracting one function? If yes → do that.
3. Does it need a new dependency, framework, or pattern? If yes → prove the
   in-process, dependency-free version is insufficient.
4. Is the extra structure for a hypothetical future? → `references/yagni.md`.

## Complexity Red Flags

- **Speculative configuration**: an option nobody sets, a strategy enum with one member.
- **Premature generalization**: `AbstractBaseFactoryProvider` for one concrete case.
- **Layering for its own sake**: a service that only forwards to another service.
- **Indirection without payoff**: interfaces with a single implementation, wrappers
  that add no behavior.
- **Cleverness**: metaprogramming, custom DSLs, recursion where a loop suffices,
  short-circuit tricks that require a comment.
- **Deep nesting**: 4+ levels of `if`/`for`/`try`. Flatten with guard clauses or
  extraction.
- **Distributed complexity**: solving with 3 services/queues what a function call does.

## The Cost Ledger

When you *do* need complexity, name its price out loud:

> "Adding the abstraction costs 60 lines and one more concept, and buys us
> the ability to add a second backend without touching callers. Requirement X
> needs that within this milestone."

If you cannot complete that sentence, keep it simple.

## KISS vs Related Rules

- KISS ≠ fewer lines. Sometimes the simple version is longer but flatter.
- KISS ≠ no design. A clear boundary between two subsystems *is* simplicity.
- KISS is not an excuse to skip tests or validation (see `references/testing.md`,
  `references/security.md`).

## Self-check

- Could a new teammate modify this without asking you a question?
- What is the first thing you would delete if forced?
- How many concepts must a reader hold in their head to follow this function?
