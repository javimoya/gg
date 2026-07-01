---
description: Builds the current phase of a gg project, one task at a time. Each run implements exactly the next task to the full bar (with tests), verifies it internally, checkpoints PROGRESS, and stops — you /clear and run it again for the next task. On the last task it closes the phase — green full suite, runnable deliverable, and a phase-close JOURNAL entry — the decisive try-it. You also see watchable slices early at the phase's show tasks — the first placed as early as the riskiest discovered target allows — so a wrong target surfaces fast. Pass --gate to pause for a go before coding. Run it after /gg:discover, and again while tasks remain.
model: inherit
disable-model-invocation: true
argument-hint: "[--gate]"
---

# /gg:next-task — Build the next task

You implement a phase's SPEC **to the highest bar**, **exactly one task per run**. You work in the
project directory (cwd); state lives in `<cwd>/.gg/`. Protocols in `${CLAUDE_PLUGIN_ROOT}/gg-shared/`;
read and edit `.gg/` per `LEDGERS.md` — by anchor, never a grown file whole.

## 0. Precondition (don't build the wrong thing)
Read `.gg/ROADMAP.md`'s header (`state` / `phase` / `stage`) — the `## State` block + `## Phase log`,
never the changelog (`LEDGERS.md`):
- **No `.gg/`** → **stop**, route to `/gg:ideate`.
- **`state: visioning`** → ideation isn't finished (no SPEC / task list yet). **Stop**, route to `/gg:ideate`.
- **`state: scoping`** → there's no SPEC / task list yet. **Stop** and route to `/gg:discover`.
- **`state: building`** → proceed; read `.gg/PROGRESS.md` and work the next task. If the task board has
  **no pending/in-progress task left**, finish it via §6 (phase close).
- **`state: shipped`** → the phase is done; the product is ready to try. **Stop — don't build here.**
  Anything the try-it surfaces — a new idea **or a bug in what shipped** — is **captured, not fixed on
  the spot**: jot it (inline, per `CAPTURE.md`; mark a defect `[bug]`), then `/gg:refine-backlog`
  triages it and `/gg:discover` designs the fix as the next phase (`/gg:orient` will route you). For a
  **single** small one you've decided to do now, `/gg:quick` records it and fast-tracks it to
  `/gg:discover` — same cycle (still a task, a test, a JOURNAL entry), just skipping the triage step.

## 1. Constitution + context load
- Read `.gg/PRINCIPLES.md` and internalize it (full bar; effort is never a reason to cut; decompose ≠
  drop; the user tries the product at the phase's **shows** and at the phase close — not task by task;
  dev ≠ launched).
- Read whole (bounded by design): `.gg/PROGRESS.md` (the task board — which task is next),
  `.gg/RUNBOOK.md`, `.gg/CONTEXT.md`, plus the `stage` and the `kind` (`build` or `research` — a
  research phase closes on a `reported` measurement, §6).
- Read by anchor (`LEDGERS.md` — a grown ledger is never read whole): the SPEC's `## Goal`, this
  phase's `AC-N` (the `(phase {N})` tag), `## Shows`, and the deliverable; the BLUEPRINT's `## Shape`
  + this phase's `## Phase {N}` section(s), pulling the phase-0 design sections the task touches;
  `ASSUMPTIONS.md ## Open`; the ADRs whose slug touches the task (`ls .gg/adr/`). For a research
  phase, also the `F-NN` blocks this phase's criteria cite (`.gg/FINDINGS.md` is created lazily, so a
  research phase's first task may run before it exists). Everything else on demand, by anchor.

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
- **If the task's `type` is `show`** (`PROGRESS-FORMAT.md`): its definition of done is *it runs and is
  watchable* via its "How to see it" (the SPEC's `## Shows` entry, `SPEC-FORMAT.md`) — built at full bar
  (thin but real spine, **no stub**). It
  carries no full suite (that stays phase-level), but the slice itself must actually work; the look
  happens at §5.
- **Stage-aware** (`STAGE.md`): in `dev`, no migrations / backward-compat / preservation tests —
  recreate & reseed freely; the stage-independent safety floor (name the rollback, get a yes before
  destroying data that exists on disk — `STAGE.md` → "Stage-independent") still binds.
- **Anti-cut reframe**: tempted to stub / defer / leave a `TODO`? Apply the constitution's test. Honest
  moves only — a later **task** in this phase (record it in PROGRESS), or take it to the user as a
  boundary. Never a silent drop.
- **New scope mid-task** (often the user raises it): **jot it** to `.gg/BACKLOG.md ## New` per
  `CAPTURE.md` (jot-only, no grilling) — it's handled in a *later* phase — and **return to the current task**.
  Never derail the build.
- **Record ADRs** for architectural/surprising decisions (the three criteria of `ADR-FORMAT.md`).
- **External wall** (a missing credential, a third party, a product decision only the user can make):
  record it in PROGRESS ("Notes for the next session": `Blocked: {reason} / unblock when {observable
  condition}`) and stop — don't fake or cut.

## 4. Verify internally + checkpoint
- Run the **focused** test/check the task needs and record its real result. (For a normal task, *you*
  verify and the user doesn't; the user's looks are the phase's **shows** and the phase close — see
  §5/§6 — not every task in between.)
- Update `.gg/PROGRESS.md`: mark the task `done`, add touched files to **owned paths**, write the
  one-line closed-task log and the **"Where to resume"** for the next task.

## 5. Not the last task → stop
If pending tasks remain, **stop here** (one task per run). Leave the tree clean and known-good. Run the
close ritual (`CLOSE-FORMAT.md`) and the breadcrumb: *"Phase {N}: task {T}/{M} done. Next: `/clear` +
`/gg:next-task` (task {T+1} — {where})."* (If the user told you to wrap up mid-task, carve the
remainder into a new follow-up task first, then stop — the same checkpoint.)
- **If the task just done is a `show`** (a designed look, *not* the phase close): also **run its "How to
  see it" yourself** (the SPEC's `## Shows` entry), then make the breadcrumb a *look-at-this* — invite the
  user to run it and react now. Route whatever the look surfaces per `CAPTURE.md`: **record the user's
  verdict on the show's `[discovered]` target as an `F-NN`** in `FINDINGS.md` (that verdict is the show's
  reason for being — even "this feels right" is the citable observation a phase close needs); any further
  observation → another `F-NN`, a wanted change or bug → `BACKLOG.md ## New` (`B-NN`) — **never** patch it
  inline or fold it into the current build. **Then branch the breadcrumb on what the look revealed.**
  **If the reactions mean the remaining tasks must change before the phase can continue** (the approach is
  wrong, or new/changed work must land *before* the next pending task): route to **`/gg:discover` to
  re-scope this phase in place** (`discover.md` §0 / §2 / §4) — it folds in the `F-NN` you just recorded
  **and** the `## New` reactions, keeps the done tasks done, and redesigns only the pending portion (it
  does **not** open a new phase). **Not `/gg:refine-backlog`** — these reactions belong to *this* phase,
  not a future one (`refine-backlog.md` §0 can't queue into `## Next phase` mid-build). Breadcrumb:
  *"Phase {N}: task {T}/{M} done — show: try it → {how to see it}. The look changed the plan; once you
  confirm, `/clear` + `/gg:discover` to re-scope phase {N} in place (folds F-NN + the `## New` reactions,
  keeps tasks 1–{T} done)."* **Otherwise** — the look is good, or the only captures are minor scope for a
  *later* phase — continue: *"Phase {N}: task {T}/{M} done — show: try it → {how to see it}. React and
  I'll capture it; then `/clear` + `/gg:next-task` (task {T+1})."*

## 6. Last task → close the phase
When the last task is done — **the phase's decisive try-it point**, where the *whole* phase is judged
green and runnable (the earlier looks, if any, were the phase's shows):
- **If that last task is itself a `show`** (`/gg:discover` may place a `[discovered]` clause's look last):
  fold its look into this close — run its "How to see it", invite the user to react, and record their
  verdict on its `[discovered]` target as an `F-NN` (the §5 show ritual) as part of this decisive try-it,
  so a show placed last is closed here, not skipped.
- Build the **deliverable**; **run the SPEC's "How to see it" yourself** and record the real result. In a
  **`research`** phase (`ROADMAP-FORMAT.md`) "How to see it" runs the **experiment**: record its measured
  result as an `F-NN` (`FINDINGS-FORMAT.md`) and **close each `reported` `AC-N` by citing that `F-NN`** —
  `yes` / `no` / `inconclusive` are all honest, green closes; a negative result is the finding, not a
  failure.
- Run the **RUNBOOK full suite**: it must be **green**; record the delta against the baseline. (A research
  phase's experiment harness is real code under that suite — no stub; only its *result* is `reported`,
  not pass/fail.)
- **Self-accounting gate** (`CONSTITUTION.md`): list everything simplified / deferred / defaulted; turn
  each into a note or justify it. **A research phase's negative or inconclusive `reported` result is an
  honest finding, never a cut to account for** — what *would* be a cut is faking a target it didn't reach
  or trimming the experiment. Ask the **VISION-conformance** question (does the product now meet
  "done and perfect", or what remains?). A **`[discovered]`** clause (`VISION-FORMAT.md`) is counted met
  **only** if a cited `F-NN` — an actual try-it observation — backs it; never infer it from the green
  suite, and if it was never looked at it is *not yet met* (record the gap, don't claim it).
- Move the phase's items from `.gg/BACKLOG.md ## Next phase` to `.gg/BACKLOG-ARCHIVE.md ## Applied`
  (with where each landed); move any default an applied item reversed to
  `ASSUMPTIONS-ARCHIVE.md ## Overridden`. Then **sweep `ASSUMPTIONS.md ## Open`**
  (`ASSUMPTIONS-FORMAT.md`): every default this phase consumed — reversing it now would be a change to
  shipped behavior (a `B-NN`), not a costless re-decision — moves whole to
  `ASSUMPTIONS-ARCHIVE.md ## Consumed`; a default still governing unbuilt work, and every
  stage-deferral (`STAGE.md`), stays `## Open`.
- Write the **phase-close `JOURNAL.md` entry** (`JOURNAL-FORMAT.md`): built / how-to-verify + real
  result / acceptance-evidence table / baseline→close tests / items applied / VISION conformance.
- Set `state: shipped`; update the ROADMAP phase-log line to `shipped {date}`.
- Breadcrumb: *"Phase {N} shipped — try it: {how to see it}. Then `/gg:capture` anything to change
  (or `/gg:quick` to fast-track one small fix), and `/clear` + `/gg:discover` for the next phase."*

## --gate (optional)
The user invoked `/gg:next-task $ARGUMENTS` — gate on that literal value. **If `$ARGUMENTS` contains
`--gate`**, after §1 show the acceptance criteria + the task you're about to do and **wait for an
explicit go** before any code. **If it's empty**, running `/gg:next-task` is itself the go — proceed.
