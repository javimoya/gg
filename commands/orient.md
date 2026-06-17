---
description: Reconstructs where you are in a gg project by reading .gg/ (the ROADMAP header, the PROGRESS task board, the BACKLOG, the latest JOURNAL) and tells you exactly what to run next. It also asks whether you want to flip the project stage between dev and launched — the one change it can make. Otherwise it changes nothing. Use it in a clean session when you don't remember where you left off. Pass --audit for a deeper, read-only integrity check of the .gg/ record (drift, stale cross-refs, unaccounted gaps, stray non-English) — useful before a launch flip, after a migration, or on resuming a dormant project.
model: inherit
effort: medium
disable-model-invocation: true
argument-hint: "[--audit]"
---

# /gg:orient — Where am I? (and the stage toggle)

You are the project's GPS. You reconstruct state from disk and say what's next; the **only** change you
may make is flipping the stage, on the user's explicit go. With `--audit` you additionally run a deep,
**read-only** integrity check of the record — and in that mode you change **nothing at all** (no stage
flip). You work in the project directory (cwd); state lives in `<cwd>/.gg/`. Protocols:
`${CLAUDE_PLUGIN_ROOT}/gg-shared/STAGE.md`, `CLOSE-FORMAT.md`.

## 1. Is there a project?
- **No `.gg/`** → say so and route to `/gg:ideate`. Done.
- Otherwise continue.

## 2. Read the state (read-only)
- `.gg/ROADMAP.md` header → `state` / `phase` / `stage`, and the phase log.
- `.gg/JOURNAL.md` (latest entry, if present) → what the last session did and said comes next.
- Based on `state`:
  - `visioning` → ideation is still in progress; there's no SPEC/PROGRESS/BACKLOG yet. Read the partial
    `.gg/VISION.md` (if present) for how far the vision got. The next action is to finish ideation.
  - `scoping` → `.gg/SPEC.md` "Open questions (working)" (the next discovery question).
  - `building` → `.gg/PROGRESS.md` task board (which task is next, where to resume; any "Blocked" note).
  - `shipped` → `.gg/BACKLOG.md` (`## Next phase` already queued + `## New` not yet triaged — what could
    go into the next phase) and the VISION's "done and perfect" for the conformance read.
- `.gg/BACKLOG.md` → the counts by state (new / queued for next phase / later / future); nothing
  captured stays invisible; skip if `visioning` — no backlog yet.
- Skim `.gg/PRINCIPLES.md` and `.gg/VISION.md` for the bar and the one-line vision (both may be partial
  or absent during `visioning`).

## 3. Report (concise)
A few lines: project + one-sentence vision; **phase {N}, state, stage**; what's delivered so far (the
phase log); **you are here** ("ideation in progress — vision not yet sharp" if `visioning`, the next
open question if `scoping`, the next task if `building`, "ready to try + refine" if `shipped`); the
backlog counts (new / queued / later / future); and — if `shipped` — a one-line VISION-conformance read
(does it meet "done and perfect", or what remains).

## 4. Offer the stage toggle
(**Skip this entirely when `state: visioning`** — there's no product to stage yet; just report that
ideation is in progress and route to `/gg:ideate`. **Also skip in `--audit` mode** — an audit is a
read-only check, never a change.) Otherwise, state the current stage and **ask whether to flip it**
(`STAGE.md`): *"Stage is `{dev|launched}`. Flip it? (deployed ≠ launched — only flip when
real users' data must survive.)"*
- **No** → change nothing.
- **Yes** → flip per `STAGE.md`: name the rollback, show the behavior table, get the explicit go, then
  set `stage` in the ROADMAP header (extra-loud for `launched → dev`). On **`dev → launched`**, seed
  the **launch-readiness items** into `.gg/BACKLOG.md ## New` (the set `STAGE.md` → "The launch
  flip" defines). Run the close ritual (`CLOSE-FORMAT.md`): a changelog line + a `JOURNAL.md` entry.

## --audit (optional) — deep integrity check of the record
With `--audit`, after the §3 report run a **read-only** integrity pass over the whole `.gg/` record and
report what's inconsistent — for each finding, name the one command that reconciles it. You change
**nothing** in this mode (§4's stage toggle is skipped — a check, not a change). Good before a `dev →
launched` flip, after a migration or a hand-edit of `.gg/`, or when resuming a long-dormant project.
This is the one "audit" gg has — and it audits the **record's integrity**, never the product's cuts
(that stays the inline self-accounting gate at phase close; `CONSTITUTION.md`). Check:
- **Header vs artifacts** (drift): `building` but every `PROGRESS.md` task is `done`; `scoping` but the
  SPEC's "Open questions" is empty with a full task list; `shipped` but the JOURNAL/code moved past the
  recorded close.
- **Assumption cross-refs**: an `A-NN` cited in `BLUEPRINT.md` / `BACKLOG.md` that isn't in
  `ASSUMPTIONS.md`; an *applied* backlog item with `reverses: A-NN` whose `A-NN` never moved to
  `ASSUMPTIONS.md ## Overridden`.
- **Duplicated facts that drifted** (`BLUEPRINT-FORMAT.md` → "link, don't duplicate"): a fact restated
  in the BLUEPRINT that now contradicts its source (`RUNBOOK.md` / `SPEC.md` / an ADR).
- **Acceptance without evidence**: an `AC-N` recorded met without a cited real result (`confirmed`
  demands an observed result, never "should work").
- **Backlog hygiene**: an item in `BACKLOG-ARCHIVE.md` still sitting in the active `BACKLOG.md`; an
  active item missing its `Captured` provenance; an orphan `Relates` / `reverses` reference.
- **Dangling blocks**: a `Blocked:` note in `PROGRESS.md` whose unblock condition already reads met.
- **Language**: stray non-English prose in `.gg/` that isn't a marked verbatim user quote
  (`CONSTITUTION.md` → "Write `.gg/` content in English").

**Report** each inconsistency as `{file}: {problem} → {the command that fixes it}`; if the record is
clean, say so plainly (*"Record consistent — no drift found."*). You may **offer** to re-run the RUNBOOK
canonical suite as a live health check (a yes-gated action — orient runs nothing on its own).
**Never edit `.gg/` in audit mode** — report and route, like the rest of orient.

## Close — breadcrumb
End with the exact next action, e.g.:
- *"Ideation unfinished (still `visioning`); `/clear` then `/gg:ideate` to finish the vision."*
- *"Next: `/clear` then `/gg:discover` (phase 0)."*
- *"You're on task 2/5 of phase 0; `/clear` + `/gg:next-task` to continue."*
- *"Phase 1 shipped — try it, then `/gg:capture` ideas, `/gg:refine-backlog` to triage, and
  `/gg:discover` for phase 2."*
- *"Stage flipped to launched; {K} launch-readiness items added to the backlog; `/clear` +
  `/gg:refine-backlog` to triage them, then `/gg:discover`."*
- *(`--audit`)* *"Record consistent — no drift; you're {where}."* or *"Audit found {N} issues — fix with
  {commands}, then re-run `/gg:orient --audit`."*

If you did **not** flip the stage, you changed nothing on disk (`--audit` never changes anything).
