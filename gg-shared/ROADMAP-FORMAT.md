# ROADMAP.md format

`.gg/ROADMAP.md` is the project's **canonical dispatch state** — the small header every command reads
first to know where the project is, plus a log of the phases done and under way. There is no
per-chunk state machine here: the current phase's tasks live in `PROGRESS.md`.

## Structure

```md
# Roadmap — {Project name}

## State
- **state**: {scoping | building | shipped}
- **phase**: {N}            <!-- 0 = the initial product; 1, 2, … = refinement phases -->
- **stage**: {dev | launched}

## Phase log
{One line per phase, newest last. Phase 0 is the whole product; each later phase names the notes it
folded.}
- **Phase 0** — the initial product — {building | shipped {YYYY-MM-DD}}
- **Phase 1** — {the notes folded, e.g. "home redesign + dark mode"} — {building | shipped {YYYY-MM-DD}}

## Structural changelog
- {YYYY-MM-DD} — project kicked off by /gg:ideate (stage: dev).
```

## Rules

- **The header is the single source of truth for dispatch.** `state` / `phase` / `stage` route every
  command — `/gg:orient` reports them and each command's precondition checks them. Update them the
  moment they change; a stale header mis-routes the next clean session.
- **State vocabulary.** `scoping` (set at `/gg:ideate`'s close, where `/gg:discover` designs the
  phase) → `building` (in `/gg:next-task`) → `shipped` (phase done, product runnable and tried). The
  only back-edge is `shipped → scoping`: a new refinement phase begins when `/gg:discover` runs with
  pending notes. Before `.gg/` exists there is no state at all — `/gg:ideate` hasn't scaffolded yet.
- **`phase` counts cycles, not chunks.** Phase 0 is the initial `discover → next-task*` cycle; each
  refinement phase increments it. A phase's tasks live in `PROGRESS.md`, not here.
- **`stage` is flipped only by `/gg:orient`** (`STAGE.md`), on the user's explicit go; record the flip
  in the changelog.
- **The phase log is append-only narrative.** One line per phase, newest last; never rewrite a shipped
  phase's line. It's how a future session sees how the product grew.
- **External wall ≠ a state.** If `/gg:next-task` is stopped by something outside the session's
  control (a missing credential, a third party, a product decision only the user can make), it records
  the blocker and the unblock condition in `PROGRESS.md` ("Notes for the next session") and stops with
  a breadcrumb — it does not invent a parallel state. The user clears it and re-runs.
- **Changelog discipline.** Every structural change (kickoff, a new phase opened, a stage flip) gets
  one dated line.
- **No secrets** (`.gg/` is committed): record the *name* of an env var/credential, never its value.
