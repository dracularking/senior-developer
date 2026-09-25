# YAGNI — You Aren't Gonna Need It

Do not build something until it is required *now*. Tomorrow's imagined requirement
is not a requirement.

## The Rule

> When, and only when, you actually need it, do you write it.

No speculative parameters, no extension points for hypothetical backends, no config
for a deployment model that doesn't exist, no base class with one child.

## Common Disguises

- "We'll need to swap the database later." → You have one database. Add a seam when
  the second one is funded.
- "Let's make it configurable." → Who configures it? What are the legal values?
  If you can't name the operator and the value, hardcode the current one.
- "Let's add a base class so future subclasses are easy." → Write the concrete class.
- "I'll add a cache/queue/layer just in case." → Measure first. Premature
  infrastructure is the most expensive YAGNI violation.
- "The API might return X." → Handle the documented contract; add a branch when you
  observe X in the wild.
- "This will be reused across projects." → YAGNI applies double to reuse that
  nobody requested.

## Signals You Are Already Over-Building

- More than one layer of indirection between caller and effect.
- More than one implementation of an interface.
- More than one option in a config object nobody set.
- More than one constructor/factory for the same object.
- Tests asserting that a *hook* exists rather than that behavior is correct.
- Code you cannot delete without asking someone outside the project.

## What YAGNI Does NOT Excuse

- **Validation and error handling** for inputs that can genuinely arrive malformed.
- **Security controls** required by the actual threat model (see `references/security.md`).
- **The test** that proves today's behavior (iron rule 7).
- **Clear naming and structure** — readability is not speculation.

## The Defer Loop

1. Implement the simplest thing that satisfies the stated requirement.
2. Leave the code *easy to change*, not *pre-changed*.
3. When the second concrete use appears, refactor then — with tests green.
4. Record the decision in one line (comment or commit message) if the omission is
   surprising: `// single provider for now; extract when a 2nd lands`.

## Self-check

- Which specific future requirement is this code for? Can you name it?
- If that requirement never arrives, is this code dead weight?
- Could you delete this in 5 minutes without breaking anything?
