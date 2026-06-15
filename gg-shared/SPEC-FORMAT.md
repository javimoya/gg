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
  - **Evidence**: {automated | manual | visual | performance | security}
  - **Verify**: {the exact command/steps — for `automated`, the test name/path in the RUNBOOK suite}
- **AC-2** (phase 0) — …

## Deliverable + "How to see it"
- **Deliverable**: {the runnable/observable product the user tries at the end of a phase}
- **How to see it**: {the real command or steps the user runs, and what to observe}

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
  *inferred* — no runnable check — isn't a criterion; sharpen it. IDs never change once assigned.
- **Phase N extends the contract, it doesn't fork it.** A refinement phase adds new `AC-N` (tagged
  with its phase) or revises existing ones in place; the SPEC always describes the *current* product.
- **The deliverable is runnable/observable, and "How to see it" is a real command or concrete steps**
  — never a vague description. It is what the user tries at a phase close (the only try-it point).
- **References the blueprint; never reduplicates the schema.** WHAT and the acceptance live here; the
  design (data model, architecture) lives in `BLUEPRINT.md`. One source of truth each.
- **An assumption is not an AC.** Defaults `/gg:discover` took live in `ASSUMPTIONS.md`; they decide
  *what to build*. An `AC-N` is still closed only by real, reproduced evidence — never "should work".
- **"Open questions" is a resumable checkpoint, not the contract.** While `/gg:discover` grills,
  answered decisions are written into the sections above and unresolved ones stay here, most-important
  first. The phase stays `scoping` while this list is non-empty; discover empties it before moving to
  `building`. This is what lets a long discovery be cut and resumed.
