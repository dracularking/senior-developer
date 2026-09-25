# Architecture

Structural decisions: where code goes, what depends on what, and how a change
propagates. Load when a task crosses module/service boundaries, adds a module,
or you must decide a folder layout.

## First Principle: Architecture Is Cost Allocation

You are not drawing a diagram; you are deciding *where future change will be
cheap and where it will be expensive*. Every boundary makes one kind of change
easy and another hard. Choose based on which changes you expect.

## Dependency Direction

- Dependencies point inward toward stable policy, never outward toward volatile detail.
- Domain/business rules must not import frameworks, drivers, SDKs, HTTP, or the DB.
- The outer layer may know the inner layer; the reverse is a dependency inversion
  violation (see `references/solid.md` DIP).
- Watch for: cycles between modules, `import` at module top-level that drags in
  the world, "shared" modules everyone imports (a hidden cycle magnet).

## Boundaries

Draw a boundary where at least one of these is true:

- Different rate of change (UI churnes, billing doesn't).
- Different reason to change (different stakeholder or requirement source).
- Different failure or scaling characteristics.
- A hard external dependency you want isolated and mockable.
- Security/trust difference (user input vs. trusted core).

Do NOT draw a boundary because "clean architecture says so".

## Module Layout Heuristics

- **Feature-first** (`orders/`, `billing/`) for application code: things that change
  together live together.
- **Layer-first** (`models/`, `views/`, `services/`) only when layers genuinely have
  independent lifecycles — otherwise you get shotgun surgery (one feature, N folders).
- Put code next to its only caller until a second caller appears.
- A `utils/`/`common/`/`helpers/` grab-bag is a boundary failure; relocate on touch.
- Keep the entrypoint thin: parse, validate, delegate, respond. No business logic.

## Coupling And Cohesion

- High cohesion: functions in a module serve one purpose and change together.
- Low coupling: you can change a module without editing another one's source.
- Measure coupling by *change propagation*: how many files does one requirement touch?

## Choosing Between Processes

Before adding a service, queue, or worker, exhaust:

1. Same-process call.
2. Same-process async/background task.
3. Separate process.
4. Separate service + network + ops burden.

Each step buys isolation at the price of latency, partial failure, deployment, and
observability. Requirements, not résumé-building, decide.

## Evolution

- Architecture is discovered under load, not designed at zero. Start flat and
  modular; split when measurements or change frequency demand it.
- Preserve an easy path for the likely change. A wrong-but-reversible decision is
  often better than a right-but-costly one.
- When refactoring structure, keep behavior identical and prove it with tests
  before and after (`references/testing.md`).

## Checklist Before Adding A Layer

- [ ] What concrete requirement does this layer satisfy?
- [ ] What becomes *harder* because it exists?
- [ ] Can a test still exercise the core logic without booting this layer?
- [ ] Is there a way to delete it in one commit?
