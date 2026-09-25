# Security

Default-deny, validate at the boundary, fail closed. Load before touching
authentication, authorization, input handling, file paths, subprocesses, secrets,
deserialization, crypto, or anything that talks to a network on behalf of a user.

## Threat Model First

Two questions, answered before coding:

1. **What is untrusted?** User input, file uploads, URLs, headers, environment
   variables, third-party APIs, DB rows written by older code, JSON from a queue.
2. **What must not happen?** Data leaks across tenants, privilege escalation,
   arbitrary file/process access, money moved twice, irreversible destruction.

Design the control for the actual threat. Do not add crypto-shaped decoration.

## Boundary Rules

- **Validate, then use.** Allow-list at the boundary: format, length, range, enum.
  Never rely on later code to be careful.
- **Encode for the sink.** Output encoding is per-context (HTML, SQL, shell, URL,
  log). Parameterized queries always; string-built SQL never.
- **Path containment.** Join untrusted segments only after normalizing and verifying
  the resolved path stays inside the intended root (`..`, absolute paths, symlinks,
  Windows drive/UNC tricks).
- **Command execution.** No shell for untrusted input; pass argv arrays. If a shell
  is unavoidable, treat every byte as hostile.
- **Deserialization.** Never unpickle/load/eval untrusted data. Prefer explicit
  schemas with strict types and unknown-field rejection.
- **SSRF.** Do not fetch attacker-supplied URLs from a privileged network position
  without allow-listing scheme and destination.
- **Replay/idempotency.** Money-moving and state-changing endpoints need a
  uniqueness or idempotency key.

## AuthN / AuthZ

- **Authenticate** the caller; **authorize** the action on that specific resource.
  Check authorization server-side on every request — hidden UI is not access control.
- Deny by default: unknown actor gets nothing.
- Object-level check: "can this user access *this* id?" (IDOR is the classic miss).
- Sessions/tokens: short-lived where possible, signed with a reviewed algorithm,
  never "none"/user-controlled alg, secrets from env/secret manager — never source.

## Secrets And Data

- No secrets in code, logs, error messages, URLs, or client bundles. Rotate on
  exposure; add a scanner/`gitleaks` step if the repo has none.
- Log identifiers, not payloads: no passwords, tokens, card numbers, or full PII.
- Minimize what you store; encrypt in transit (TLS) and at rest where required.
- Error responses to clients are generic; detail goes to the server log.

## Dependencies

- Prefer the standard library and well-maintained packages; check the dependency
  before adding it. Pin versions; review lockfile diffs.
- Run the existing audit tool (`npm audit`, `pip-audit`, `cargo audit`) if present.
- New dependency = new attack surface + maintenance cost; justify it.

## Failure Modes

- Fail closed. A authz check that errors must deny, not allow.
- `except: pass` around security checks is a vulnerability, not hygiene.
- Don't roll your own crypto, token formats, or password hashing — use reviewed
  primitives (argon2/bcrypt/scrypt, HMAC/JWT libs, OS randomness).

## Reporting A Finding

Lead with it, severity first: what's exploitable, by whom, with what impact,
`file:line`, the minimal proof, and the recommended fix. If the codebase already
has a pattern for this, follow it.
