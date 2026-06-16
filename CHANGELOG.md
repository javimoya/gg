# Changelog

All notable changes to `gg` are recorded here. Versions follow the `version` field in
`.claude-plugin/plugin.json`.

## 2.1.0

`/gg:ideate` is now **resumable**: `visioning` is a real, first-class state, not just a documented name.

### Changed
- **`visioning` is the project's first on-disk state.** `/gg:ideate` writes the `ROADMAP.md` header
  (`state: visioning`) the moment it scaffolds `.gg/`, *before* grilling — so an ideation cut before the
  vision is sharp is now **resumable**: re-run `/gg:ideate` and it continues the grilling from where it
  stopped instead of treating the project as already kicked off. The vision-sharp close promotes
  `visioning → scoping`.
- **Every command routes on `visioning`.** `/gg:discover`, `/gg:next-task`, and `/gg:capture` send a
  mid-ideation project back to `/gg:ideate`; `/gg:orient` reports "ideation in progress" and skips the
  stage toggle (there's no product to stage yet).
- **State vocabulary documented.** `ROADMAP-FORMAT.md` now spells out all four states —
  `visioning → scoping → building → shipped` — and that each one is resumable by re-running its owning
  command (`/gg:ideate`, `/gg:discover`, `/gg:next-task` respectively).

## 2.0.0

The first public release of `gg` — a Claude Code workflow plugin that builds a product in **phase 0**
and then refines it **phase by phase**, keeping all project state on disk in `.gg/`.

### The model
- **A "phase" is one `discover → next-task*` cycle.** Phase 0 builds the whole product end to end;
  phase 1, 2, … each fold in a selected set of captured notes. Within a phase, work is split into
  **tasks**.
- **Five commands**: `/gg:ideate` (once → a sharp VISION), `/gg:discover` (design the whole product
  into a BLUEPRINT + a testable SPEC + recorded ASSUMPTIONS + an ordered task list; in a refinement
  phase, a triage gate picks which notes to include, then grills them together), `/gg:next-task`
  (build exactly the next task, verify it internally, checkpoint, stop — the last task closes the
  phase and is the only point you try the product), `/gg:capture` (jot an idea into the backlog with
  light reconciliation; no grilling), and `/gg:orient` (read-only GPS + the dev/launched stage
  toggle).
- **Strict command preconditions.** Each command refuses out-of-order calls and routes you to the
  right one.

### Highlights
- **Discover-all-up-front + `BLUEPRINT.md`.** The whole data model/architecture is designed once, so
  later phases *extend* it instead of re-opening a frozen structure and writing migrations for it.
- **Recorded assumptions (`ASSUMPTIONS.md`).** Grilling asks the load-bearing questions and logs every
  other choice as a numbered, reversible default — *the cut is the unrecorded assumption*. High-blast
  decisions are always grilled; the discover sign-off surfaces defaults for veto.
- **A `dev` / `launched` stage.** While a product is in development, the system skips migrations,
  backward-compatibility, and data-preservation work; you flip to `launched` (via `/gg:orient`) when
  real users' data must survive (deployed ≠ launched), which seeds launch-readiness notes. Your own
  on-disk data is protected in both stages.
- **One task per `next-task` run; you try the product only at a phase close.** `PROGRESS.md` is both
  the task board and the handoff. Verification is the green RUNBOOK suite at the phase close, the
  runnable deliverable, and a self-accounting + VISION-conformance gate (there is no separate audit).
- **Boundary vs. deferral, made unambiguous.** A *boundary* is what the finished product will never
  include (→ `VISION.md`); a *deferral* is in-scope work pushed to a later phase (→ a note in
  `.gg/NOTES.md`, the only record re-read at the next discover). The constitution and the grilling
  protocol guard against mislabeling one as the other.

### On disk (`.gg/`)
`PRINCIPLES.md` · `VISION.md` · `ROADMAP.md` (state · phase · stage + phase log) · `BLUEPRINT.md` ·
`ASSUMPTIONS.md` · `SPEC.md` · `PROGRESS.md` · `NOTES.md` · `RUNBOOK.md` · `CONTEXT.md` ·
`JOURNAL.md` · `adr/`.
