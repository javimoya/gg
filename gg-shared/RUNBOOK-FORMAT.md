# RUNBOOK.md format

`.gg/RUNBOOK.md` pins **how this project is run and verified**, so every clean session executes the
*same* commands instead of re-guessing them. `/gg:next-task` runs the full suite from here for its
phase baseline and again at phase close; if each session invented its own command, "green" would mean
different things. Created **lazily** — first pinned in `/gg:discover` or `/gg:next-task` when a real
run/test command exists — and kept current as the stack solidifies.

## Structure

```md
# RUNBOOK — {Project name}

## Prerequisites & setup
{Toolchain/runtime versions, install/bootstrap steps (e.g. `npm ci`, `uv sync`, `make setup`).}

## Environment
{Env vars the project needs — by NAME only, with what each is for. Never a value.}

## Full suite (the canonical one)
{The single command that runs the complete test suite. This exact command is the phase baseline and
the phase-close check. e.g. `npm test` / `pytest -q` / `cargo test --all`.}

## Focused tests
{How to run a subset while iterating — by path, file, or name filter.}

## Lint / static analysis
{Format, lint, type-check, any static gate.}

## Build
{How to produce the build artifact, if any.}

## Deliverable — "How to see it"
{The real command(s)/steps a user runs to exercise the current deliverable and what to observe.
Mirrors the SPEC's "How to see it".}

## Destructive paths & external effects
{Commands that touch the network, write data, deploy, send, or are otherwise irreversible — flagged
so a session never runs them blindly. In `dev`, reset/seed scripts (e.g. `scripts/seed.py` — drops &
recreates tables) are first-class dev tools; at the launch flip they are reclassified destructive and
gated (`STAGE.md`).}
```

## Rules

- **One canonical full-suite command.** "Full suite" must resolve to a single, copy-pasteable command;
  the phase baseline and the phase-close check run *that* one — never two different ones.
- **Env vars by name, never by value.** `.gg/` is committed (`CONSTITUTION.md` → "Safety and
  reversibility").
- **Destructive and outward steps are flagged, not hidden — and the list is stage-aware.** In `dev` a
  recreate/reseed is routine; in `launched` it's a gated, rollback-named operation (`STAGE.md`).
- **It stays current.** A RUNBOOK that names a command the project no longer has is worse than none.
  Update it in the same session the stack changes; if a section doesn't apply yet, say "n/a".
- **Reproducible, not narrative.** Every entry is a command or concrete steps a stranger could run.
