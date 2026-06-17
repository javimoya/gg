# Changelog

All notable changes to `gg` are recorded here. Versions follow the `version` field in
`.claude-plugin/plugin.json`.

## 2.3.0

### Added
- **`/gg:orient --audit` — a read-only integrity check of the record.** A deep pass over the whole
  `.gg/` that flags drift the inline discipline doesn't re-validate: header-vs-artifacts contradictions,
  stale `A-NN` cross-refs, duplicated facts that drifted, an `AC-N` marked met without evidence, backlog
  hygiene, dangling blocks, and stray non-English prose. It reports each issue with the command that
  fixes it, **changes nothing** (no stage flip in this mode), and is meant for before a launch flip,
  after a migration, or on resuming a dormant project. It audits the record's *integrity* — never the
  product's *cuts*, which stay the inline self-accounting gate at phase close.

### Changed
- **The BLUEPRINT is now append-only.** `/gg:discover` designs the whole product in phase 0, and that
  design is then **frozen** — never edited. **Every refinement phase appends a dated `## Phase N`
  section** so the design ledger has no gaps, and the **depth scales**: a phase of only bugs/tweaks gets
  a one-line "no design change", while a phase that adds an entity/field/component or supersedes an
  earlier decision gets the full detail. Earlier content is never rewritten — the same move-don't-delete
  discipline as the `JOURNAL.md` and the assumptions ledger. This removes the silent design-drift that a
  living, hand-edited blueprint invites.
- **Link, don't duplicate, in the BLUEPRINT.** It restates no fact that lives authoritatively elsewhere
  — the test framework and run/verify commands belong to `RUNBOOK.md`, acceptance to `SPEC.md`, the
  "why" of a big call to an ADR — so a duplicated fact can't drift out of sync.

## 2.2.0

A real **backlog** with its own triage command. What used to be `NOTES.md` is now `BACKLOG.md`, and a
new `/gg:refine-backlog` lets you triage it one item at a time — instead of deciding everything inside
`/gg:discover`.

### Added
- **`/gg:refine-backlog` — a sixth command.** Between phases it walks each new backlog item one at a
  time and you give it a disposition: **next phase**, **later**, **future**, or **discard** (archived
  with a reason). It is **idempotent** — by default it walks only `## New`, so a triaged item is never
  shown again; `--later` / `--future` revisit those deferral tiers on purpose, and every run opens with
  the counts of all sections so nothing is invisible. It only triages — `/gg:discover` still designs.
- **`BACKLOG-ARCHIVE.md`.** Closed items leave the active backlog: **applied** items at phase close,
  **discarded** items when you drop them (with a recorded reason) — kept for the trace, never deleted.
- **Provenance + a `[bug]` marker on every item.** The `Captured` line records whether **you** raised
  it or the **agent** deferred it; a defect in shipped behavior is prefixed `[bug]`.

### Changed
- **`NOTES.md` → `BACKLOG.md`, sectioned by lifecycle.** The backlog is now `## New` / `## Next phase`
  / `## Later` / `## Future` instead of one `## Pending` list — an item's state *is* its section.
- **Triage left `/gg:discover`.** Discover no longer asks "which notes?" — it **consumes the
  `## Next phase` set** that `/gg:refine-backlog` already queued, and grills those items together.
- **A bug found while trying the shipped product is captured, not patched inline.** It flows through the
  normal `capture → refine-backlog → discover` cycle (so it gets a task, a test, and a `JOURNAL.md`
  entry), instead of an off-spec inline fix that left the record stale.
- **All `.gg/` content is written in English.** A new constitution rule pins the project's on-disk state
  to one language (verbatim user quotes excepted) so the agent's adherence stays sharp; it governs the
  `.gg/` prose only, never the project's own code or stack.

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
