---
description: Builds the current phase of a gg project, one task at a time. Each run implements exactly the next task to the full bar (with tests), verifies it internally, checkpoints PROGRESS, and stops — you /clear and run it again for the next task. On the last task it closes the phase — green full suite, runnable deliverable, and a phase-close JOURNAL entry — and only then do you try the product. Pass --gate to pause for a go before coding. Run it after /gg:discover, and again while tasks remain.
model: inherit
disable-model-invocation: true
argument-hint: "[--gate]"
---

# /gg:next-task — Build the next task

You implement a phase's SPEC **to the highest bar**, **exactly one task per run**. You work in the
project directory (cwd); state lives in `<cwd>/.gg/`. Protocols in `${CLAUDE_PLUGIN_ROOT}/gg-shared/`.

## 0. Precondition (don't build the wrong thing)
Read `.gg/ROADMAP.md`'s header (`state` / `phase` / `stage`):
- **No `.gg/`** → **stop**, route to `/gg:ideate`.
- **`state: visioning`** → ideation isn't finished (no SPEC / task list yet). **Stop**, route to `/gg:ideate`.
- **`state: scoping`** → there's no SPEC / task list yet. **Stop** and route to `/gg:discover`.
- **`state: building`** → proceed; read `.gg/PROGRESS.md` and work the next task. If the task board has
  **no pending/in-progress task left**, finish it via §6 (phase close).
- **`state: shipped`** → the phase is done; the product is ready to try. **Stop — don't build here.**
  Anything the try-it surfaces — a new idea **or a bug in what shipped** — is **captured, not fixed on
  the spot**: jot it (inline, per `CAPTURE.md`; mark a defect `[bug]`), then `/gg:discover` triages and
  fixes it as the next phase (`/gg:orient` will route you). Patching a shipped defect inline — off the
  board, off the SPEC — is the misstep that leaves the record stale; everything, bugs included, goes
  through the cycle so it gets a task, a test, and a JOURNAL entry.

## 1. Constitution + context load
- Read `.gg/PRINCIPLES.md` and internalize it (full bar; effort is never a reason to cut; decompose ≠
  drop; the user tries the product only at phase close; dev ≠ launched).
- Read `.gg/PROGRESS.md` (the task board — which task is next), `.gg/SPEC.md` (acceptance criteria),
  `.gg/BLUEPRINT.md` (the design), `.gg/ASSUMPTIONS.md`, `.gg/RUNBOOK.md`, `.gg/CONTEXT.md`, the ADRs,
  and the `stage`.

## 2. Durable start (first run of the phase only)
If `.gg/PROGRESS.md` has no provenance yet (this is the phase's first task):
1. **Record provenance** (`PROGRESS-FORMAT.md`): the base commit (`git rev-parse HEAD`, or
   `unversioned`), the pre-existing dirty paths (`git status --porcelain` — the user's, off-limits),
   an empty owned-paths list. **Never commit** to set this baseline.
2. **Run the RUNBOOK full suite** and record the baseline (N pass / M fail + names). If already red,
   say so and decide with the user before building on it.
3. Persist; the phase log already reads `building` from discover.

## 3. Implement exactly the next task to the bar
- Build **only the next task** complete and robust; cover the edge cases; **write tests** mapping to
  the acceptance criteria.
- **Stage-aware** (`STAGE.md`): in `dev`, no migrations / backward-compat / preservation tests —
  recreate & reseed freely. In **both** stages, **name the rollback and get a yes before destroying
  data that exists on disk** (recreating a *populated* store counts).
- **Anti-cut reframe**: tempted to stub / defer / leave a `TODO`? Apply the constitution's test. Honest
  moves only — a later **task** in this phase (record it in PROGRESS), or take it to the user as a
  boundary. Never a silent drop.
- **New scope mid-task** (often the user raises it): **jot it** to `.gg/NOTES.md` per `CAPTURE.md`
  (jot-only, no grilling) — it's handled in a *later* phase — and **return to the current task**.
  Never derail the build.
- **Record ADRs** for architectural/surprising decisions (the three criteria of `ADR-FORMAT.md`).
- **External wall** (a missing credential, a third party, a product decision only the user can make):
  record it in PROGRESS ("Notes for the next session": `Blocked: {reason} / unblock when {observable
  condition}`) and stop — don't fake or cut.

## 4. Verify internally + checkpoint
- Run the **focused** test/check the task needs and record its real result. (No user try-it here —
  that's phase close only; between tasks, *you* verify, the user doesn't.)
- Update `.gg/PROGRESS.md`: mark the task `done`, add touched files to **owned paths**, write the
  one-line closed-task log and the **"Where to resume"** for the next task.

## 5. Not the last task → stop
If pending tasks remain, **stop here** (one task per run). Leave the tree clean and known-good. Run the
close ritual (`CLOSE-FORMAT.md`) and the breadcrumb: *"Phase {N}: task {N}/{M} done. Next: `/clear` +
`/gg:next-task` (task {N+1} — {where})."* (If the user told you to wrap up mid-task, carve the
remainder into a new follow-up task first, then stop — the same checkpoint.)

## 6. Last task → close the phase
When the last task is done — **this is the only point the user tries the product**:
- Build the **deliverable**; **run the SPEC's "How to see it" yourself** and record the real result.
- Run the **RUNBOOK full suite**: it must be **green**; record the delta against the baseline.
- **Self-accounting gate** (`CONSTITUTION.md`): list everything simplified / deferred / defaulted; turn
  each into a note or justify it. Ask the **VISION-conformance** question (does the product now meet
  "done and perfect", or what remains?).
- Move the phase's applied notes to `.gg/NOTES.md ## Applied`; mark any default a note reversed as
  `ASSUMPTIONS.md ## Overridden`.
- Write the **phase-close `JOURNAL.md` entry** (`JOURNAL-FORMAT.md`): built / how-to-verify + real
  result / acceptance-evidence table / baseline→close tests / notes applied / VISION conformance.
- Set `state: shipped`; update the ROADMAP phase-log line to `shipped {date}`.
- Breadcrumb: *"Phase {N} shipped — try it: {how to see it}. Then `/gg:capture` anything to change, and
  `/clear` + `/gg:discover` for the next phase."*

## --gate (optional)
With `--gate`, after §1 show the acceptance criteria + the task you're about to do and **wait for an
explicit go** before any code. Without it, running `/gg:next-task` is the go.
