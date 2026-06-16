---
description: Designs and scopes a phase of a gg project, then hands off to /gg:next-task. In phase 0 it grills the whole product into a BLUEPRINT (data model + architecture), a testable SPEC, recorded ASSUMPTIONS for everything not asked, and an ordered task list. In a refinement phase it first asks which pending notes to include (the triage gate), then grills the selected set together. Resumable. Run it right after /gg:ideate, or after a phase ships when there are pending notes.
model: inherit
disable-model-invocation: true
---

# /gg:discover — Design and scope a phase

You turn a vague scope — the whole product in phase 0, or a selected set of notes in a refinement
phase — into a **design + a testable contract + an ordered task list** that `/gg:next-task` can build
with no further questions. You work in the project directory (cwd); state lives in `<cwd>/.gg/`.

Shared protocols in `${CLAUDE_PLUGIN_ROOT}/gg-shared/`: `GRILLING.md`, `CONSTITUTION.md`,
`BLUEPRINT-FORMAT.md`, `ASSUMPTIONS-FORMAT.md`, `SPEC-FORMAT.md`, `PROGRESS-FORMAT.md`, `NOTES-FORMAT.md`,
`STAGE.md`, `CONTEXT-FORMAT.md`, `ADR-FORMAT.md`, `RUNBOOK-FORMAT.md`, `CLOSE-FORMAT.md`.

## 0. Precondition
Read `.gg/ROADMAP.md`'s header (`state` / `phase` / `stage`):
- **No `.gg/`** → no project yet (or ideation was cut before it scaffolded). **Stop**, route to `/gg:ideate`.
- **`state: scoping`** → proceed. Either phase 0 right after ideate, or **resuming** a discovery (the
  SPEC's "Open questions" is your queue).
- **`state: building`** → a phase is already designed and mid-build. Don't silently re-discover and
  clobber it: **stop** and route to `/gg:next-task` (re-design only if the user explicitly asks).
- **`state: shipped`** → a phase just shipped. Start the next phase **only if there are pending notes**
  in `.gg/NOTES.md ## Pending`. If there are none, **stop**: nothing to refine yet — the user should
  `/gg:capture` an idea first.

## 1. Orient + constitution
- Read `.gg/PRINCIPLES.md` (the bar: full product, decompose ≠ drop, the cut is the *unrecorded*
  assumption, dev ≠ launched).
- Read `.gg/VISION.md`, `.gg/CONTEXT.md`, `.gg/adr/`, the current `BLUEPRINT.md` / `SPEC.md` /
  `ASSUMPTIONS.md` (if they exist), `.gg/RUNBOOK.md`, and the `stage`.

## 2. Triage gate (refinement phases only)
If this is a refinement phase (`state: shipped` with pending notes), **before grilling** read
`.gg/NOTES.md ## Pending` and ask the user **which notes this phase includes** (`GRILLING.md` → "The
note triage gate"): all / a specific one / a recommended set (with one-line reasoning) / a pick. The
**selected set is phase N**: bump `phase`, set `state: scoping`, add its phase-log line. Unselected
notes stay pending. (In phase 0 there are no notes — the scope is the whole product.)

## 3. Grill (the `GRILLING.md` protocol) — and record defaults
One question at a time, with your recommended answer, exploring the code when the answer is there.

- **Phase 0 — design the whole product.** Grill the **load-bearing** decisions and write the design
  into `.gg/BLUEPRINT.md` (`BLUEPRINT-FORMAT.md`): the **whole data model / schema**, the architecture,
  the shared types. Designing it whole up front is what stops later phases from layering and migrating.
- **Phase N — grill the selected notes together** (a joint view, so their tasks come out coherent),
  **extending** the BLUEPRINT rather than re-opening frozen structures.
- **Record every default.** You can't ask everything. Ask the load-bearing question per area;
  everything else becomes a numbered `A-NN` in `.gg/ASSUMPTIONS.md` (`ASSUMPTIONS-FORMAT.md`) — the
  default, why, how to reverse it, blast-radius. **High-blast decisions are grilled, never defaulted.**
- **Stage-aware** (`STAGE.md`): in `dev`, do **not** grill migration / backward-compat / data-
  preservation questions — record the omission as an `A-NN` (the launch flip will consume it). In
  `launched`, those are real questions.
- Sharpen terms in `CONTEXT.md` inline; pin run/verify commands in `.gg/RUNBOOK.md` as they firm up;
  offer ADRs only on the three criteria.
- Keep unresolved questions in the SPEC's **"Open questions (working)"** list (most important first) —
  the phase stays `scoping` while it's non-empty, so a long discovery can be cut and resumed.

## 4. Write the SPEC + the task list
- Write/extend `.gg/SPEC.md` (`SPEC-FORMAT.md`): the acceptance criteria (each `AC-N` with evidence
  type + Verify, tagged by phase), the deliverable + "How to see it". Reference the BLUEPRINT; don't
  reduplicate the schema.
- Write the **ordered task list** into `.gg/PROGRESS.md` (`PROGRESS-FORMAT.md`): sized so each task
  fits one fresh `/gg:next-task` session, **foundational-first** (the integrating spine before
  features, so later tasks build on a working base). Tasks have no deliverable/tests of their own.

## 5. Sign-off (confirm questions done + defaults taken)
Before closing (`GRILLING.md` → "Confirm before you stop"): (1) name any areas still worth probing and
let the user decide whether to keep grilling; (2) **surface the high/medium-blast defaults** you
recorded, for veto. Only when "Open questions" is empty and the user is satisfied, set `state:
building` and update the ROADMAP phase-log line.

## Close — ritual + breadcrumb
Run the close ritual (`CLOSE-FORMAT.md`): persist BLUEPRINT / ASSUMPTIONS / SPEC / PROGRESS / ROADMAP,
append a `JOURNAL.md` entry, then the breadcrumb:
- **Done** (`building`): *"Phase {N} designed: {M} tasks in PROGRESS. Next: `/clear` then
  `/gg:next-task` for task 1."*
- **Cut mid-way** (`scoping`): *"Phase {N} discovery checkpointed; {K} open questions left, next is
  '{question}'. Continue with `/clear` + `/gg:discover`."*
