# SPEC.md format

`.gg/SPEC.md` is the project's **living contract** — the testable acceptance criteria and the
deliverable, produced and extended by `/gg:discover`. `/gg:next-task` implements to it. One file for
the whole product (not one per phase): each phase adds or revises criteria. The design they slice
lives in `BLUEPRINT.md`; the task list that builds them lives in `PROGRESS.md`.

## Structure

```md
# SPEC — {Project name}

## Goal
{What the product delivers and for whom. A few sentences; the full destination is in VISION.md.}

## Acceptance criteria (testable)
{Each criterion carries a stable ID, the Given/When/Then, the evidence type, the exact check, and the
phase that introduced it. IDs are stable for the life of the project.}
- **AC-1** (phase 0) — {Given … when … then …}
  - **Evidence**: {automated | manual | visual | performance | security | reported}
  - **Verify**: {the exact command/steps — for `automated`, the test name/path in the RUNBOOK suite; for
    `reported`, the experiment to run and the `F-NN` finding that records its measured result}
- **AC-2** (phase 0) — …

## Shows
{The CURRENT phase's look points only — each a watchable slice the user reacts to, with its one-line
"How to see it" (`type: show` in `PROGRESS.md`; placement rule in `/gg:discover` §4). The single home
for that text, read by `/gg:next-task`. Transient: `/gg:discover` rewrites the section whole at phase
open, and removes it when the phase has no shows; one bullet per show.}
- {the show} — {its one-line "How to see it"}

## Deliverable + "How to see it"
- **Deliverable**: {the runnable/observable product the user tries at the phase close — the decisive try-it}
- **How to see it**: {the real command or steps the user runs, and what to observe}
- **Try (this phase)**: {the phase's load-bearing flows, one line each, citing the `AC-N`/`B-NN` it
  exercises — rewritten whole at phase open, like `## Shows`}

## Design
{One line pointing at `BLUEPRINT.md` — the schema/architecture this contract slices. Don't reduplicate
it here.}

## Open questions (working)
{Transient grilling queue — present only while `state` is `scoping`. One bullet per unresolved
question, most important first. Emptied before discover moves the project to `building`.}
- [ ] {next open question}
```

## Rules

- **Acceptance criteria have stable IDs, are atomic, name their evidence, and tag their phase.** Each
  `AC-N` is one Given/When/Then with an **evidence type** (`automated` preferred; others only when
  genuinely not automatable) and an exact, reproducible **Verify**. A criterion that can only be
  *inferred* — no runnable check — isn't a criterion; sharpen it. (`reported`, below, is not the
  exception it looks like: it *has* a runnable experiment — only its *answer* is open, not whether there
  is a check.) IDs never change once assigned; the next `AC-N` is one past the highest assigned anywhere
  in this SPEC (one file for the whole product, across every phase) — never reused, even when a criterion
  is later revised.
- **A user-triggered write path is verified through the real route.** Its `automated` evidence drives
  the request end-to-end — request → guard → persistence → read-back — never only isolated units; a
  unit-green path whose route was never driven is unverified (the guard that rejects what the units
  accept is exactly what unit tests miss).
- **`reported` is for a `research` phase's open question — a measure, not a capability.** A `research`
  phase (`ROADMAP-FORMAT.md`) closes on what it *learned*, so its acceptance is an **open empirical
  question** ("under {conditions}, does {X} yield {Y}?") whose answer isn't known at spec time. A
  `reported` `AC-N` is **closed by a cited `F-NN`** (`FINDINGS-FORMAT.md`) recording the experiment's
  measured result — and **`yes` / `no` / `inconclusive` are all honest, green closes**: a negative result
  ("explored {X}; it does **not** yield {Y}; here is the evidence") is the finding, not a failure. Four
  guardrails keep it from becoming a cut:
  - **Measure-question only, never a disguised capability.** If the clause can be written as "the product
    *does* {X}" (a capability present or absent), it is **not** `reported` — make it
    `automated`/`manual`/`visual`/… The same dodge as a `[discovered]` tag on a checkable clause
    (`VISION-FORMAT.md`).
  - **Still real, reproduced evidence.** It cites the actual experiment run and its `F-NN`, never an
    inferred "should work". `reported` is a *different shape* of evidence, never *less* of it.
  - **`research` phases only.** A `build` phase's `AC-N` stays a capability criterion with pass/fail
    evidence; `reported` may not appear in a `build` phase (that would license shipping less under a build
    banner).
  - **The answer is the deliverable, not a target.** The phase ships on the recorded answer; it never
    pre-writes the result it must hit.
- **Phase N extends the contract, it doesn't fork it.** A refinement phase adds new `AC-N` (tagged
  with its phase) or revises existing ones in place; the SPEC always describes the *current* product.
  **When a phase removes or replaces shipped behavior, it supersedes the criteria that described it**:
  collapse each affected criterion to a single line —
  `**AC-48** (phase 5) — superseded (phase {N}): {what removed or replaced it}` — the id stays retired
  and scannable, the Given/When/Then goes. A criterion left describing removed behavior is a
  contradiction in the contract, not history.
- **`## Shows` is transient — current phase only.** `/gg:discover` rewrites the section whole when it
  opens a phase, and removes it when the phase has no shows; a prior phase's shows never accumulate
  here (a past show's record is its `F-NN` verdict).
- **The close certifies the try list.** The deliverable carries the phase's load-bearing flows as a
  short **Try** list (each citing its `AC-N`/`B-NN`); the user's close verdict certifies *that list*,
  and the close's `F-NN` cites it (`next-task.md` §6). Transient like `## Shows` — `/gg:discover`
  rewrites it whole at phase open. A flow too risky to leave to a visual pass — a user-triggered
  write, a heavy job — belongs on the list by name.
- **The deliverable is runnable/observable, and "How to see it" is a real command or concrete steps**
  — never a vague description. It is what the user tries at a phase close (the decisive try-it point; the
  shows are the earlier looks). For a **`research` phase** (`ROADMAP-FORMAT.md`) the deliverable is the
  *measured answer with its cited evidence* — the experiment runs and its result is recorded as an `F-NN`
  — and "How to see it" is the command that reproduces that experiment.
- **References the blueprint; never reduplicates the schema.** WHAT and the acceptance live here; the
  design (data model, architecture) lives in `BLUEPRINT.md`. One source of truth each.
- **An assumption is not an AC.** Defaults `/gg:discover` took live in `ASSUMPTIONS.md`; they decide
  *what to build*. An `AC-N` is still closed only by real, reproduced evidence — never "should work".
- **"Open questions" is a resumable checkpoint, not the contract.** While `/gg:discover` grills,
  answered decisions are written into the sections above and unresolved ones stay here, most-important
  first. The phase stays `scoping` while this list is non-empty; discover empties it before moving to
  `building`. This is what lets a long discovery be cut and resumed.
