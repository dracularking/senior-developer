# Before Coding

Run this before writing or modifying any code. If a box cannot be checked, stop
and resolve it — most rework starts here.

## Requirement

- [ ] I can state the goal in one sentence without using "and".
- [ ] Acceptance criteria are explicit (what must be observably true when done).
- [ ] Ambiguities that would change the design were asked — in one batch, not dribbled out.
- [ ] Scope is bounded: I know what is explicitly **out** of scope.
- [ ] The user's proposal, if any, has been stress-tested. If it has a flaw, I said so
      before implementing it (flattery is a defect).

## Codebase Investigation

- [ ] I read the file(s) I will change, plus their callers.
- [ ] I know how this project's code is organized (module layout, naming, error style).
- [ ] I found prior art: an existing helper, pattern, or earlier attempt for this problem.
- [ ] I know the exact commands for: build, test, lint, format.
- [ ] I checked whether a similar change was made before (`git log` on the path).

## Facts, Not Memory

- [ ] Every API/flag/parameter I plan to use was verified against the actual source,
      docs, or a running example — not recalled.
- [ ] Versions confirmed (language, framework, key dependencies) if behavior differs
      between versions.
- [ ] No assumption of behavior I have not observed.

## Design

- [ ] The approach is the simplest one that meets the stated criteria.
- [ ] Any added structure (layer, config, abstraction, dependency) is tied to a
      **current** requirement, not a hypothetical one.
- [ ] The riskiest assumption is identified, and I know how to test it early.
- [ ] Impact radius known: which files change, which behaviors could regress.
- [ ] For a bug: I have a reproduction, and the regression test plan is defined.

## Safety

- [ ] Auth, input, paths, subprocesses, secrets, or deserialization involved →
      `references/security.md` loaded.
- [ ] Cross-module change → `references/architecture.md` loaded.
- [ ] I know how to revert this change (it is isolated and reversible).
