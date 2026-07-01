# PROGRESS.md format

`.gg/PROGRESS.md` is the **task board and handoff** for the phase under way — the document that shows
every task of the current phase and its status, and lets you `/clear` and resume cleanly.
`/gg:next-task` reads it to know what's next, does exactly one task per run, then checkpoints here.
Terse is fine; unambiguous is mandatory.

## Structure

```md
# PROGRESS — {Project name} — Phase {N}

## Provenance
{Written by /gg:next-task before the first code change of the phase, then never rewritten (only
"Owned paths" grows). The owned paths are this phase's authoritative scope.}
- **Base commit**: {git HEAD when the phase's build started, or `unversioned` if not a git repo}
- **Pre-existing dirty paths**: {paths already modified/untracked when the build started — the user's,
  off-limits, never reverted. Empty / n.a. without a VCS.}
- **Owned paths**: {files this phase created or changed — grows as tasks proceed}

## Task board
| # | task | type | status |
|---|------|------|--------|
| 1 | {name} | — | done |
| 2 | {name} | — | done |
| 3 | {name} | show | in-progress |
| 4 | {name} | — | pending |

## Where to resume
- **Next**: task {N} — {file:line / the concrete next step}
- **Notes for the next session**: {gotchas; in-flight decisions; or "Blocked: {reason} / unblock when
  {observable condition}" if an external wall stopped you}

## Closed-task log
{One line per finished task (newest last) — enough to trust it's done, no more.}
- **Task 1 — {name}**: {what got built} · verified: {the focused check + its real result}
```

## Rules

- **The task board is the source of truth for "what's left in this phase."** One row per task,
  status ∈ `pending` / `in-progress` / `done`. It mirrors the plan `/gg:discover` produced.
- **`type: show` marks a try-it task** (`/gg:discover` sets it; placement rule in `/gg:discover` §4). A
  `show` builds a thin runnable slice;
  its one-line "How to see it" lives in the SPEC's `## Shows` entry (`SPEC-FORMAT.md`), not on the board,
  and it ends with a *look-at-this* stop (`/gg:next-task`); a normal build task is `—` and has no
  deliverable of its own. The full green suite is always phase-level — a `show` look is not a phase close.
- **`/gg:next-task` does exactly one task per run** and updates the board + "Where to resume" before
  stopping. That single handoff is why no separate wrap command is needed.
- **Provenance is written once, before any code change, and not rewritten.** Only "Owned paths" grows.
  "Pre-existing dirty paths" are never claimed as owned and never reverted (`CONSTITUTION.md` →
  "Safety and reversibility").
- **"Where to resume" is unambiguous** — a `file:line` or a concrete next step a stranger could act on.
  This is the whole point of the checkpoint.
- **Closed tasks collapse to one line.** One `- **Task {N} — {name}**: …` bullet per task — hard cap
  two lines, never a paragraph (the JOURNAL holds the narrative).
- **Board cells are names.** A row's `task` cell is a short name — 80 characters at most, citing the
  ids it builds (`B-NN` / `AC-N`) rather than restating them; descriptions and acceptance text live in
  the SPEC and the BLUEPRINT, not on the board.
- **No phase-level deliverable or full-suite claims here.** A task's "verified" is a *focused* check;
  the deliverable run and the full green suite are phase-level and recorded in the phase-close JOURNAL
  entry.
- **One phase per file — `/gg:discover` replaces it at phase open.** `PROGRESS.md` holds exactly the
  current phase's board; opening a new phase rewrites the file whole (`discover.md` §4). Prior boards
  are never stacked here under any heading — a prior phase's outcome lives in its phase-close JOURNAL
  entry.
