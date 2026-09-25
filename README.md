# senior-developer

An Agent Skill that makes AI coding assistants work like a senior software
engineer — not a code generator.

It encodes **Clean Code + SOLID + KISS / DRY / YAGNI + TDD + root-cause debugging
+ code review + anti-hallucination** into one work protocol: verify before writing,
find the root cause before patching, prefer the simplest design that satisfies the
requirement, and prove the change works before saying "done".

## Install

### OpenCode

```powershell
# global (all projects)
npx skills add dracularking/senior-developer

# or manually clone into a skill directory
git clone https://github.com/dracularking/senior-developer.git `
  "$HOME\.config\opencode\skills\senior-developer"
```

Project-local: clone into `.opencode/skills/senior-developer` instead.

### Claude Code

```powershell
git clone https://github.com/dracularking/senior-developer.git `
  "$HOME\.claude\skills\senior-developer"
```

## Trigger

The skill activates on requests like:

- "review my code"
- "fix it properly" / "why is this failing"
- "write this properly" / "production quality"
- any non-trivial implementation, debugging, refactoring, or architecture task

## The Iron Rules

1. No code without understanding the requirement.
2. No edits without investigating the existing code.
3. No guessing APIs — tool-verified beats remembered.
4. Root cause, not symptom.
5. Simplest solution that satisfies the requirement.
6. No speculative design (YAGNI).
7. New behavior gets a test first (or at least a test).
8. Do not refactor unrelated code.
9. Every change is verified by evidence — command output, not "it should work".
10. Self-review before reporting.
11. Overturn your own bad plan when you find it.
12. Do not add complexity to look professional.

## Structure

```
senior-developer/
├── SKILL.md                 frontmatter, iron rules, 6-phase workflow, routing table
├── references/              loaded on demand, only when the task matches
│   ├── clean-code.md        naming, functions, comments, AI-slop detection
│   ├── solid.md             SRP / OCP / LSP / ISP / DIP, applied not preached
│   ├── kiss.md              complexity cost ledger
│   ├── dry.md               duplicated knowledge vs. duplicated text
│   ├── yagni.md             speculative requirements and their disguises
│   ├── architecture.md      boundaries, dependency direction, module layout
│   ├── testing.md           red-green-refactor, determinism, what to test
│   ├── debugging.md         reproduce → hypothesize → prove → fix the root
│   ├── security.md          threat model, boundary validation, authz, secrets
│   ├── git.md               commits, staging, history safety, revert
│   └── code-review.md       severity-ordered findings, self-review protocol
└── checklists/
    ├── before-coding.md     requirement · investigation · facts · design · safety
    ├── before-commit.md     diff review · self-review · verification · craft
    └── before-done.md       closure · evidence · robustness · handoff
```

## How It Works

`SKILL.md` is a router, not a wall of rules. It holds the twelve iron rules, a
six-phase workflow, and a table that maps a signal in the work to exactly one
reference file:

| Signal | Load |
|---|---|
| Long functions, dead code, slop | `clean-code.md` |
| Too many reasons to change | `solid.md` |
| "This is getting complicated" | `kiss.md` |
| Same logic in two places | `dry.md` |
| Building for a future that may never come | `yagni.md` |
| New module, service, folder layout | `architecture.md` |
| Writing or repairing tests | `testing.md` |
| Something is broken or flaky | `debugging.md` |
| Auth, input, secrets, paths, subprocesses | `security.md` |
| Commit, branch, revert | `git.md` |
| Reviewing code | `code-review.md` |

Nothing loads "just for context" — context is a budget.

## What It Prevents

| Failure mode | Countermeasure |
|---|---|
| Flattery — agreeing with a flawed proposal | Stress-test first, offer the alternative |
| Guessing an API or behavior from memory | Check the source/docs; run it |
| Symptom patching — retries, swallowed exceptions | Reproduce, explain the mechanism, fix the cause |
| Gold plating — abstractions nobody asked for | YAGNI + KISS cost ledger |
| Silent scope creep | Minimal surface, Boy Scout Rule only |
| Unverified "done" | Evidence standard + `before-done.md` |
| AI verbosity | Output discipline section |

## Design Notes

- **Skills are documentation, not enforcement.** This skill steers behavior; pair it
  with real gates (linters, typecheckers, CI, pre-commit hooks) for guarantees.
- **Composable.** Combine with a TDD skill or a diagram skill: this one owns *how
  the engineer works*, those own *specific techniques*.
- **Language-agnostic.** Examples and commands are given per project; the skill
  requires discovering the project's own test/lint/build commands rather than
  assuming them.

## License

MIT
