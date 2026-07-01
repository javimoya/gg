# ROADMAP.md format

`.gg/ROADMAP.md` is the project's **canonical dispatch state** — the small header every command reads
first to know where the project is, plus a log of the phases done and under way. There is no
per-chunk state machine here: the current phase's tasks live in `PROGRESS.md`.

## Structure

```md
# Roadmap — {Project name}

## State
- **state**: {visioning | scoping | building | shipped}
- **phase**: {N}            <!-- 0 = the initial product; 1, 2, … = refinement phases -->
- **stage**: {dev | launched}
- **kind**: {build | research}   <!-- the current phase's flavor: build to a known spec, or a search -->

## Phase log
{One line per phase, newest last. Phase 0 is the whole product; each later phase names the backlog
items it folded — or, for a research phase, the question it pursued.}
- **Phase 0** — the initial product — {visioning | scoping | building | shipped {YYYY-MM-DD}}
- **Phase 1** — {the backlog items folded, e.g. "home redesign + dark mode"} — {scoping | building | shipped {YYYY-MM-DD}}
- **Phase 2** — research: {the open question pursued} — {scoping | building | shipped {YYYY-MM-DD}}

## Structural changelog
- {YYYY-MM-DD} — project kicked off by /gg:ideate (stage: dev).
```

## Rules

- **The header is the single source of truth for dispatch.** `state` / `phase` / `stage` route every
  command — `/gg:orient` reports them and each command's precondition checks them. (`kind` is the current
  phase's *flavor*, not a routing gate — `/gg:discover` sets it, `/gg:next-task` consumes it, `/gg:orient`
  reports it; see below.) Update them the moment they change; a stale header mis-routes the next clean
  session.
- **State vocabulary.** `visioning` (set the moment `/gg:ideate` scaffolds `.gg/`, before any grilling
  — the vision isn't sharp yet) → `scoping` (set at `/gg:ideate`'s close once the vision is sharp; this
  is where `/gg:discover` designs the phase) → `building` (in `/gg:next-task`) → `shipped` (phase done,
  product runnable and tried). The only back-edge is `shipped → scoping`: a new refinement phase begins
  when `/gg:discover` runs with items queued by `/gg:refine-backlog`. **Each state is resumable** by re-running its owning
  command on a clean session: `visioning` → `/gg:ideate` (it continues the grilling — it does **not**
  treat the project as already kicked off); `scoping` → `/gg:discover`; `building` → `/gg:next-task`.
  Before `.gg/` exists there is no state at all. `/gg:ideate` writes the header (`state: visioning`,
  `phase: 0`, `stage: dev`, `kind: build`) and the kickoff changelog line at scaffold, then promotes
  `visioning → scoping` at its close. Only phase 0 passes through `visioning`; refinement phases open at
  `scoping`.
- **`phase` counts cycles, not chunks.** Phase 0 is the initial `discover → next-task*` cycle; each
  refinement phase increments it. A phase's tasks live in `PROGRESS.md`, not here.
- **`kind` is the current phase's flavor — `build` or `research`.** A **`build`** phase builds capability
  to a known spec (the default; **phase 0 is always `build`** — it lays the foundation a search later runs
  on). A **`research`** phase *is a search*: a question/hypothesis → an experiment → an observed result (a
  finding, `FINDINGS-FORMAT.md`) → the next step decided *from* that result; its acceptance is `reported`
  (`SPEC-FORMAT.md`) and it can close honestly on a negative result. `/gg:discover` sets `kind` when it
  opens a phase — from what the phase pursues (an open question / `[exp]` experiments → `research`;
  capabilities / bugs → `build`), **surfaced for the user's veto at sign-off**. A `research` phase uses the
  **same** `scoping → building → shipped` states as any other — *no new state*; only its acceptance shape
  differs.
- **`stage` is flipped only by `/gg:orient`** (`STAGE.md`), on the user's explicit go; record the flip
  in the changelog.
- **The phase log is append-only narrative.** One line per phase, newest last; never rewrite a shipped
  phase's line. It's how a future session sees how the product grew.
- **External wall ≠ a state.** If `/gg:next-task` is stopped by something outside the session's
  control (a missing credential, a third party, a product decision only the user can make), it records
  the blocker and the unblock condition in `PROGRESS.md` ("Notes for the next session") and stops with
  a breadcrumb — it does not invent a parallel state. The user clears it and re-runs.
- **Changelog discipline — a closed set.** One dated line per structural event: kickoff, a new phase
  opened, an in-place re-scope (`discover.md` §2), a stage flip. Nothing else qualifies — never a line
  per task, per session, or per shipped phase (the phase log's line already records the ship). If in
  doubt, it isn't structural. The changelog is write-only outside `/gg:orient --audit` (`LEDGERS.md`).
- **No secrets** (`.gg/` is committed): record the *name* of an env var/credential, never its value.
