---
description: Kicks off a new gg project from a vague idea. Runs divergent brainstorming then convergent grilling to pin a sharp VISION, scaffolds the .gg/ directory (PRINCIPLES, VISION, ROADMAP), and sets the project stage to dev. It is the first step and runs once per project; the design and the build come next via /gg:discover then /gg:next-task. If a project already exists it does nothing and routes you to /gg:orient.
model: inherit
disable-model-invocation: true
---

# /gg:ideate — Kick off a new project

You turn a vague idea into a **sharp VISION**, writing it into `.gg/` so later clean sessions can
continue. You work in the project directory (cwd); state lives in `<cwd>/.gg/`. You produce the
**destination, not the design** — `/gg:discover` designs the whole product next.

Shared protocols live in `${CLAUDE_PLUGIN_ROOT}/gg-shared/` — read each when a step references it:
`CONSTITUTION.md`, `GRILLING.md`, `VISION-FORMAT.md`, `ROADMAP-FORMAT.md`, `CONTEXT-FORMAT.md`,
`STAGE.md`, `CLOSE-FORMAT.md`.

## 0. Precondition (only at the very start)
Read `.gg/ROADMAP.md`:
- **No `.gg/`** → this is a new project. Proceed.
- **`.gg/` already exists** → the project is already kicked off. `/gg:ideate` runs **once**. **Stop**
  and route to `/gg:orient` (it'll say where you are and what's next). A new idea now goes to
  `/gg:capture`; the next phase is started by `/gg:discover`.

## 1. Scaffold + read the constitution
- Create `.gg/`:
  - `mkdir -p .gg`
  - Copy `${CLAUDE_PLUGIN_ROOT}/gg-shared/CONSTITUTION.md` → `.gg/PRINCIPLES.md` (verbatim — the
    constitution).
  - Create `.gg/VISION.md` and `.gg/ROADMAP.md` following `${CLAUDE_PLUGIN_ROOT}/gg-shared/VISION-FORMAT.md`
    and `ROADMAP-FORMAT.md`.
  - `CONTEXT.md`, `BLUEPRINT.md`, `ASSUMPTIONS.md`, `SPEC.md`, `PROGRESS.md`, `RUNBOOK.md`, `NOTES.md`,
    `adr/`, and `JOURNAL.md` are created **lazily** later (most in `/gg:discover` / `/gg:next-task`;
    `JOURNAL.md` at this first close).
- Read `.gg/PRINCIPLES.md` and internalize it: the final product is complete to the agreed bar;
  effort is never a reason to cut; nothing is dropped silently; "close, not perfect, then refine" is
  legitimate only because gaps are recorded (assumptions/notes).

## 2. Diverge, then converge (the `GRILLING.md` protocol)
- **Diverge first**: the idea is vague, so widen the solution space hard — options, alternatives,
  prior art, and lateral ideas the user hasn't named (angles from adjacent products and other
  domains), plus risks and "have you considered X?".
- **Then converge**: grill **one question at a time, with your recommended answer**, to pin the
  *destination* — the problem, who it's for, what it is and is not, the "done and perfect" bar, the
  non-negotiables. Sharpen fuzzy terms into `CONTEXT.md` inline. (The full product *design* — schema,
  architecture, the task list — is `/gg:discover`'s job; stay at the vision level here.)
- A new idea raised now belongs **in this grilling**, not in `/gg:capture` (capture is for once a
  product exists).
- **Confirm before you stop** (`GRILLING.md`): propose areas still worth exploring and let the user
  decide when the vision is sharp enough.

## 3. Write the VISION + set the stage
- Fill `.gg/VISION.md` per `VISION-FORMAT.md`: problem/opportunity, for whom, what it is / is not,
  constraints and accepted tradeoffs (boundaries, not cuts), **"done and perfect"** (the real bar,
  not an MVP), non-negotiables, unknowns.
- **Set the stage to `dev`.** A new project has no real users (`STAGE.md`). Confirm with the pointed
  question — *"is this already launched to real users whose data must survive? (deployed ≠ launched)"*
  — defaulting to `dev`. Record it in `VISION.md`'s `## Stage` and the ROADMAP header.

## 4. Initialize the ROADMAP header
Fill `.gg/ROADMAP.md` per `ROADMAP-FORMAT.md`: `state: scoping`, `phase: 0`, `stage: dev`; a phase-log
line *"Phase 0 — the initial product — scoping"*; and a dated `## Structural changelog` line *"project
kicked off by /gg:ideate (stage: dev)"*. There is no per-phase table — a "phase" is a whole
`discover → next-task*` cycle; the product's structure is designed in `/gg:discover`'s BLUEPRINT.

## Close — present, then ritual + breadcrumb
1. **Present the VISION** to the user — the destination in their own words, the "done and perfect" bar,
   the stage. Invite any last adjustment.
2. **Run the close ritual** (`CLOSE-FORMAT.md`): persist VISION/ROADMAP, append the first `JOURNAL.md`
   entry, then the breadcrumb:
   *"Vision set (stage: dev). Next: `/clear` then `/gg:discover` to design and scope the whole product
   (phase 0)."*
