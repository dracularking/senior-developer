# Clean Code

Rules for code that a stranger (including future you) can read and change safely.
Apply while writing; do not use as an excuse to rewrite working code.

## Naming

- A name answers: what it is, what it does, why it exists. `daysSinceLastRetry`, not `d`.
- Booleans read as predicates: `isExpired`, `hasChildren`, `canRetry`.
- Avoid mental-displacement names: `data`, `info`, `temp`, `manager`, `helper`,
  `processItem`, `handleStuff`.
- One concept, one word. Do not call the same thing `user`, `account`, and `profile`
  in different files.
- Length matches scope: loop counters may be `i`; module-level API must be explicit.

## Functions

- Do one thing, name it honestly. If the name needs `And`/`Or`, it does two things.
- Command/Query separation: a function either does something or answers something,
  rarely both.
- No boolean-flag arguments — they signal the function does two things. Split it.
- Pure where possible: same input, same output, no observable side effects.
- Deep modules, small interfaces: hide complexity behind a narrow, obvious surface.
- Prefer early return over nested `if`s. Guard clauses flatten logic.
- Delete a parameter you don't use. A parameter is a dependency; dependencies cost.

## Comments

- Comments explain *why*, not *what*. The code already says what.
- A comment that narrates the loop is a naming failure: `for item in items` needs
  no `# iterate over items`.
- No commented-out code in committed changes. Version control remembers.
- No apologies, no attributions, no changelog-in-comment. Use commit messages.
- If a comment is needed to explain a block, extract the block into a named function
  first; often the comment disappears.

## Structure

- Stepdown rule: a file reads top-down like a narrative — high-level policy first,
  details below.
- Keep side effects visible in the signature: `sendInvoice()` not `prepareData()`
  if it also sends.
- Errors are not comments. Handle them or propagate them deliberately.
- Duplication of *structure* is fine; duplication of *logic* is not (see dry.md).

## Anti-patterns: AI Slop

Reject these in your own output:

| Slop | Instead |
|---|---|
| Narrating comments (`# loop through results`) | Better name, no comment |
| `except Exception: pass` / silent catch | Handle, log with context, or let it propagate |
| One-call "helper" functions | Inline it until a second caller exists (yagni) |
| God `utils.py` / `helpers.py` | Put logic next to its owner |
| `temp2_final_fixed.py` naming | One clear name; fix the original |
| Premature `logger.debug` everywhere | Log state transitions and failures, not every line |
| Catch-all `return None` hiding the real error | Return a typed failure or raise |
| Abstractions with exactly one implementation | Concrete code until the second arrives |

## Size

Size is a symptom, not a rule. Target: a function fits on one screen, a file reads
in one sitting. When something is too big, it is usually doing too many things —
split by responsibility, not by line count.

## Self-check

- Could you explain this file's purpose in one sentence without using "and"?
- Is every name unambiguous without reading the body?
- Are there any comments describing what the code does?
- Is there any code path that can fail silently?
