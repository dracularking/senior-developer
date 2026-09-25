# Code Review

Apply to your own diff before presenting it, and to the user's code when asked.
Findings first, ordered by severity, each with `file:line`. No praise padding.

## Order Of Concern

1. **Correctness** — does it do what the requirement says, for all inputs that
   matter? Off-by-one, null/empty, boundary, error paths, concurrent access.
2. **Security** — see `references/security.md`. Authz on every resource access,
   injection, secrets, unsafe deserialization.
3. **Data integrity** — migrations, idempotency, partial failure, rollback,
   durability. Anything that can corrupt or double-write is critical.
4. **Design** — right abstraction level, coupling, dependency direction. Does it
   add a layer the requirement didn't ask for (yagni)?
5. **Tests** — do they exist, do they actually fail when the logic is wrong, are
   they deterministic?
6. **Maintainability** — naming, structure, dead code, misleading comments.
7. **Style/nits** — only if the project has no formatter to enforce them.

## Severity Labels

- **[critical]** Exploitable, data-losing, or breaks the stated requirement. Block.
- **[major]** Will cause bugs or high maintenance cost soon. Should block.
- **[minor]** Real but low impact. Fix if cheap.
- **[nit]** Preference. Never blocks; often best left unsaid.
- **[question]** "Did you consider …?" — legitimate uncertainty, not a veto.
- **[praise]** Rare and specific. Default to omitting.

## What A Good Comment Contains

- The problem, not just the symptom: "line 42 trusts `user.supplier_id`; any
  authenticated user can pass another supplier's id (IDOR)."
- Why it matters: consequence, likelihood, blast radius.
- Where: `path:line`.
- A concrete direction: "scope the lookup by `session.org_id`" or a link to the
  relevant test.

Bad: `extract this into a helper`. Good: `this 60-line block both parses and
writes; extracting `parseInvoice()` lets the writer be tested without a file
system.`

## Self-Review Protocol (Before Reporting Done)

1. Read the diff as a stranger. Is every hunk justified by the requirement?
2. Check the diff for accidental content: debug output, TODOs you won't finish,
   dead code, unrelated formatting, commented-out lines.
3. Confirm each changed path has evidence (`checklists/before-done.md`).
4. Ask: "What is the most likely way this fails in production?" Answer it.
5. Ask: "Is there a simpler version of this diff?" If yes, produce it.
6. If you find a real defect in your own work, fix it and say you found it.
   Rule 11 — overturn your own plan.

## Receiving Review

- Technical scrutiny is not an insult. Evaluate each comment on its merits.
- If you disagree, give evidence (source, docs, a repro), not deference.
- Do not blindly implement a suggestion you believe is wrong — see the rebuttal
  discipline in `SKILL.md`.
