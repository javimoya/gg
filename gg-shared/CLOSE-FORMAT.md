# Close ritual (shared)

The end-of-session ritual every **working** skill (`/gg:ideate`, `/gg:discover`, `/gg:next-task`,
`/gg:refine-backlog`, and `/gg:orient` when it flips the stage) runs before it stops — whether the
session finished its goal or is being cut mid-way. `/gg:orient`'s read-only report does not run it, and
`/gg:capture` / `/gg:quick` run no ritual of their own — the express `/gg:quick` reaches this ritual
through the `/gg:discover` it invokes (degrading to a plain `/gg:capture` jot otherwise). Running the
same ritual everywhere keeps the ROADMAP, the JOURNAL, and the breadcrumb from drifting apart between
clean sessions.

## The ritual (in order)

1. **Persist the skill's own artifacts.** Whatever this skill owns is written and consistent on disk:
   the ROADMAP header, and (as applicable) `VISION.md`, `BLUEPRINT.md`,
   `ASSUMPTIONS.md` / `ASSUMPTIONS-ARCHIVE.md`, `SPEC.md`, `PROGRESS.md`, `RUNBOOK.md`,
   `BACKLOG.md` / `BACKLOG-ARCHIVE.md`. A cut session persists its partial
   state too (discover's "Open questions"; next-task's "Where to resume"; refine-backlog's dispositions).
2. **Update the ROADMAP header; changelog only on a structural event.** `state` / `phase` / `stage`
   reflect reality. The `## Structural changelog` takes one dated line per structural event **only** —
   kickoff · a phase opened · an in-place re-scope · a stage flip — and nothing else: **a task close is
   never a changelog line** (the board and the JOURNAL hold it), and a phase ship updates its phase-log
   line, not the changelog (`ROADMAP-FORMAT.md`).
3. **Append a JOURNAL entry.** Add one entry to `.gg/JOURNAL.md` per `JOURNAL-FORMAT.md` (create it on
   the first close). Append-only — never edit a prior entry. A **phase close** uses the richer
   phase-close variant (what was built, how to verify + real result, acceptance evidence,
   baseline→final tests).
4. **Emit the breadcrumb.** End with the skill's one-line breadcrumb: *where you are + what's next +
   which command to continue with*. The JOURNAL's `Next` must match it.

## Commit (the user's call)

Committing is **not** part of the ritual. gg's default is to **never commit, push, or take any
outward action on its own** — leave the tree as is and let the breadcrumb stand. Commit **only when
the user explicitly asks**; if they do, honor any commit conventions in the project's own `CLAUDE.md`
and use a short imperative message after step 4. Never auto-commit or push.

## Cut sessions (stopping early)

If the session is stopping before its goal (the user said "wrap up", or context is filling): finish
the smallest safe unit, persist the partial state (step 1), set the ROADMAP header coherently, and
make the breadcrumb and the JOURNAL `Next` name the **exact** resume point — the next task, or the
next open question. For `/gg:next-task` this is just its normal per-task checkpoint; there is no
separate wrap command. Cutting early and handing off cleanly always beats pushing a degraded session.
