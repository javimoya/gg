---
description: Kicks off a new gg project from a vague idea. Runs divergent brainstorming then convergent grilling to pin a sharp VISION, scaffolds the .gg/ directory (PRINCIPLES, VISION, ROADMAP) right away in the visioning state, and sets the project stage (defaulting to dev, confirmed during ideation). It is the first step; the design and the build come next via /gg:discover then /gg:next-task. Because it scaffolds immediately, an ideation cut before the vision was sharp is resumable — re-running it continues the grilling. Once the vision is sharp (the project moves to scoping) it does nothing and routes you to /gg:orient.
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
Read `.gg/ROADMAP.md`'s header — route on `state`, not merely on whether `.gg/` exists:
- **No `.gg/`** → this is a new project. Proceed to §1 (scaffold).
- **`state: visioning`** → a prior ideation was cut **mid-grilling**, before the vision was sharp.
  **Resume it**: skip §1 (the scaffold is already done — do **not** re-create anything or re-log the
  kickoff), re-read `.gg/PRINCIPLES.md`, the partial `.gg/VISION.md`, `.gg/CONTEXT.md`, and (if present)
  the latest `.gg/JOURNAL.md` entry — its `Next` names where ideation stopped; if `JOURNAL.md` is absent
  the cut predated the first close. Then **continue grilling from §2**. Do **not** route away.
- **`state: scoping` or later** → the project is already kicked off (ideation finished). `/gg:ideate`
  runs **once**. **Stop** and route to `/gg:orient` (it'll say where you are and what's next). A new
  idea now goes to `/gg:capture`; the next phase is started by `/gg:discover`.

## 1. Scaffold + read the constitution
- Create `.gg/` — **write the ROADMAP header first**, so any surviving `.gg/` always carries a `state`:
  - `mkdir -p .gg`
  - Create `.gg/ROADMAP.md` per `${CLAUDE_PLUGIN_ROOT}/gg-shared/ROADMAP-FORMAT.md` and **write its
    header immediately**, before anything else: `state: visioning`, `phase: 0`, `stage: dev` (provisional
    default — confirmed in §3), the phase-log line *"Phase 0 — the initial product — visioning"*, and the
    dated `## Structural changelog` line *"project kicked off by /gg:ideate (stage: dev)"*. Writing the
    header first is what makes a cut-short ideation detectable and resumable (`state: visioning` on disk).
  - Copy `${CLAUDE_PLUGIN_ROOT}/gg-shared/CONSTITUTION.md` → `.gg/PRINCIPLES.md` (verbatim — the
    constitution).
  - Create `.gg/VISION.md` (skeleton) following `${CLAUDE_PLUGIN_ROOT}/gg-shared/VISION-FORMAT.md`.
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
- **Confirm the stage.** §1 wrote `stage: dev` provisionally; a new project has no real users
  (`STAGE.md`). Confirm with the pointed question — *"is this already launched to real users whose data
  must survive? (deployed ≠ launched)"* — defaulting to `dev` (normally a no-op). Record it in
  `VISION.md`'s `## Stage`; if the answer is `launched`, set `stage: launched` in the ROADMAP header
  and reflect it in the kickoff changelog line.

## 4. Promote the ROADMAP header (only once the vision is sharp)
The header already exists from §1 (`state: visioning`). Once the vision is sharp and the user is
satisfied (the `GRILLING.md` "confirm before you stop" checkpoint), **promote it**: set `state: scoping`
and rewrite the Phase 0 phase-log word in place from `visioning` to `scoping`. Leave `phase: 0`, the
`stage`, and the kickoff changelog line exactly as §1 wrote them — don't re-add them. There is no
per-phase table — a "phase" is a whole `discover → next-task*` cycle; the product's structure is
designed in `/gg:discover`'s BLUEPRINT. If the vision is **not** yet sharp, leave `state: visioning` and
take the cut-short close below.

## Close — present, then ritual + breadcrumb
**Vision sharp** (promoted to `scoping` in §4):
1. **Present the VISION** to the user — the destination in their own words, the "done and perfect" bar,
   the stage. Invite any last adjustment.
2. **Run the close ritual** (`CLOSE-FORMAT.md`): persist VISION/ROADMAP, append the first `JOURNAL.md`
   entry (`State change`: `visioning → scoping`), then the breadcrumb:
   *"Vision set (stage: dev). Next: `/clear` then `/gg:discover` to design and scope the whole product
   (phase 0)."*

**Cut mid-grilling** (vision not yet sharp): leave `state: visioning` on disk, run the same close ritual
(`CLOSE-FORMAT.md`) to persist the partial VISION/CONTEXT, append a `JOURNAL.md` entry (`State change`:
`— (still visioning)`; `Next`: the open vision area to reopen), then the resume breadcrumb:
*"Ideation checkpointed; vision not yet sharp. Resume with `/clear` then `/gg:ideate`."*
