---
description: Designs and scopes a phase of a gg project, then hands off to /gg:next-task. In phase 0 it grills the whole product into a BLUEPRINT (data model + architecture), a testable SPEC, recorded ASSUMPTIONS for everything not asked, and an ordered task list. In a refinement phase it consumes the set you already queued with /gg:refine-backlog (BACKLOG ## Next phase) and grills those items together — a build phase (capabilities/bugs) or a research phase (a search: experiments with reported acceptance that closes honestly even on a negative result). Resumable. Run it right after /gg:ideate, or after a phase ships once ## Next phase is queued — via /gg:refine-backlog (a batch) or /gg:quick (one item).
model: inherit
disable-model-invocation: true
---

# /gg:discover — Design and scope a phase

You turn a vague scope — the whole product in phase 0, or the set queued by `/gg:refine-backlog` in a
refinement phase — into a **design + a testable contract + an ordered task list** that `/gg:next-task`
can build with no further questions. You work in the project directory (cwd); state lives in `<cwd>/.gg/`.

Shared protocols in `${CLAUDE_PLUGIN_ROOT}/gg-shared/`: `GRILLING.md`, `CONSTITUTION.md`,
`BLUEPRINT-FORMAT.md`, `ASSUMPTIONS-FORMAT.md`, `SPEC-FORMAT.md`, `PROGRESS-FORMAT.md`, `BACKLOG-FORMAT.md`,
`STAGE.md`, `CONTEXT-FORMAT.md`, `ADR-FORMAT.md`, `RUNBOOK-FORMAT.md`, `CLOSE-FORMAT.md`.

## 0. Precondition
Read `.gg/ROADMAP.md`'s header (`state` / `phase` / `stage`):
- **No `.gg/`** → no project yet. **Stop**, route to `/gg:ideate`.
- **`state: visioning`** → ideation was kicked off but cut before the vision was sharp; there's no
  VISION/SPEC to design from yet. **Stop**, route to `/gg:ideate` to finish it.
- **`state: scoping`** → proceed. Either phase 0 right after ideate, or **resuming** a discovery (the
  SPEC's "Open questions" is your queue).
- **`state: building`** → a phase is already designed and mid-build. Don't silently re-discover and
  clobber it: **stop** and route to `/gg:next-task` (re-design only if the user explicitly asks).
- **`state: shipped`** → a phase just shipped. Start the next phase **only if there are items queued**
  in `.gg/BACKLOG.md ## Next phase`. If that section is empty, **stop** and route to
  `/gg:refine-backlog` to triage the backlog first (or `/gg:capture` if the backlog is empty) — the
  set the next phase builds is chosen there, not here.

## 1. Orient + constitution
- Read `.gg/PRINCIPLES.md` (the bar: full product, decompose ≠ drop, the cut is the *unrecorded*
  assumption, dev ≠ launched).
- Read `.gg/VISION.md`, `.gg/CONTEXT.md`, `.gg/adr/`, the current `BLUEPRINT.md` / `SPEC.md` /
  `ASSUMPTIONS.md` (if they exist), `.gg/FINDINGS.md` (the observations a queued item may cite),
  `.gg/RUNBOOK.md`, and the `stage`.

## 2. Load the queued set (refinement phases only)
If this is a refinement phase (`state: shipped`), read `.gg/BACKLOG.md ## Next phase` — the set
`/gg:refine-backlog` already triaged (`GRILLING.md` → "The queued set" for why that set *is* phase N and
why `## New` / `## Later` items stay put). **Classify the phase's `kind`** (`ROADMAP-FORMAT.md`): a set of
`[exp]` experiments / an open question to investigate → **`research`**; capabilities / bugs / tweaks →
**`build`** (the default). Record the move: bump `phase`, set `state: scoping`, **set `kind`**, add its
phase-log line (a `research` phase names its question). Surface the `kind` for the user's veto at sign-off
(§5). The items stay in `## Next phase` while you build; they move to `BACKLOG-ARCHIVE.md ## Applied` at
phase close. (In phase 0 there is no backlog — the scope is the whole product, and phase 0 is always
`kind: build`.)

## 3. Grill (the `GRILLING.md` protocol) — and record defaults
One question at a time, with your recommended answer, exploring the code when the answer is there.

- **Phase 0 — design the whole product.** Grill the **load-bearing** decisions and write the design
  into `.gg/BLUEPRINT.md` (`BLUEPRINT-FORMAT.md`): the data model / schema, the architecture, the shared
  types — **settled whole up front** so later phases extend it instead of layering and migrating.
  Settling it whole means the **irreversible foundation and the seams** decided once; where a property
  space is genuinely not-yet-knowable (an open / empirical product), design an **extension point** — an
  open map / registry / plug-point — so a discovered property extends the model without a migration
  (`BLUEPRINT-FORMAT.md`; reserve extension points for what truly can't be enumerated, never to dodge a
  knowable schema). For such a product, phase 0 settles the foundation + the extensible model, **names the
  riskiest open question** (it lives in `VISION.md` "Unknowns / risks", usually mirrored by a
  `[discovered]` clause), and drives a thin vertical to a show — the open-ended *search* is later
  `research` phases, never crammed into phase 0's tasks or a self-fulfilling criterion.
- **Phase N — grill the queued items together** (a joint view, so their tasks come out coherent). The
  phase-0 BLUEPRINT is **frozen**; **append** a dated `## Phase N` section recording this phase's design
  impact (`BLUEPRINT-FORMAT.md`), never editing earlier content. **Depth scales**: a heterogeneous set
  of only bugs/tweaks gets a one-liner ("… — no design change"); a new entity/field/component or a
  supersession of an earlier decision gets the full detail.
- **A `research` phase designs the search, not a capability** (`kind: research`, set in §2). Grill *what
  to run and what to measure*: the experiment(s), the harness that runs them, the observable signal, and
  what result would answer the question. Its acceptance is **`reported`** (§4) and its deliverable is the
  measured answer; the appended BLUEPRINT section records what the experiment needs (often just the
  extension point a result will populate), depth-scaled like any phase.
- **Apply a vision-revising item.** If a queued `B-NN` is a **correction-from-evidence** — it cites an
  `F-NN` that contradicts a "done and perfect" clause (`CONSTITUTION.md` → "Boundaries vs. cuts") —
  **apply it**: edit the clause in place in `.gg/VISION.md`, log an append-only `R-NN` in its
  `## Revisions` citing that `F-NN` (`VISION-FORMAT.md`), and cascade the design impact into the appended
  BLUEPRINT `## Phase N` section. **Refuse it** if it has no citable `F-NN`, or the `F-NN` doesn't
  contradict the clause, or the real reason is "it was hard" — that's a cut, not a correction; take it
  back to the user.
- **Record every default.** You can't ask everything. Ask the load-bearing question per area;
  everything else becomes a numbered `A-NN` in `.gg/ASSUMPTIONS.md` (`ASSUMPTIONS-FORMAT.md`) — the
  default, why, how to reverse it, blast-radius. **High-blast decisions are grilled, never defaulted.**
- **Stage-aware** (`STAGE.md`): in `dev`, do **not** grill migration / backward-compat / data-
  preservation questions — record the omission as an `A-NN` (the launch flip will consume it). In
  `launched`, those are real questions.
- Sharpen terms in `CONTEXT.md` inline; pin run/verify commands in `.gg/RUNBOOK.md` as they firm up;
  offer ADRs only on the three criteria.
- Keep unresolved questions in the SPEC's **"Open questions (working)"** list (most important first) —
  the phase stays `scoping` while it's non-empty, so a long discovery can be cut and resumed.

## 4. Write the SPEC + the task list
- Write/extend `.gg/SPEC.md` (`SPEC-FORMAT.md`): the acceptance criteria (each `AC-N` with evidence
  type + Verify, tagged by phase), the **`## Shows` entry** (each show's one-line "How to see it" when the
  phase has any — the single home for that text, which `/gg:next-task` reads), and the deliverable +
  "How to see it". Reference the BLUEPRINT; don't reduplicate the schema. In a **`research`** phase the
  criteria are **`reported`** — each `AC-N` an open question whose `Verify` is the experiment plus the
  `F-NN` that will record its measured result (`SPEC-FORMAT.md`); `reported` appears only in a research
  phase, never as a capability AC in disguise.
- Write the **ordered task list** into `.gg/PROGRESS.md` (`PROGRESS-FORMAT.md`): sized so each task
  fits one fresh `/gg:next-task` session, **foundational-first** — the integrating spine before features,
  so later tasks build on a working base. "Spine" is a **thin vertical** through the whole stack, not
  each subsystem built to full breadth first: only what genuinely **can't be retrofitted** — the
  load-bearing decisions later work can't be re-sequenced onto — must precede the first show; everything
  else is thickened after it (below).
  - **Place the shows** (`kind: show`, `PROGRESS-FORMAT.md`) — a task that builds a watchable slice and
    stops for the user to look. A show goes **where the felt character meaningfully changes** — where a
    `[discovered]` clause (`VISION-FORMAT.md`) becomes more judgeable — never on a task count.
  - **The first show is mandatory iff the phase has a `[discovered]` clause in play**, and it is
    load-bearing: anchor it to the **riskiest** `[discovered]` clause — the thinnest slice at which the
    user could form a genuine opinion about it — and **drive the task order to reach it as early as the
    expensive-to-retrofit foundation allows** (a thin vertical first, then thicken; the whole design is
    settled up front in the BLUEPRINT, so thickening is sequencing, not rework). The only
    legitimate "why not earlier" is foundation that can't be retrofitted. Keep the slice honestly thin
    but **representative** — real spine, no stub (a slice that *looks* right but misleads is worse than
    none). **Each `[discovered]` clause the phase intends to close needs a show** at the point it becomes
    judgeable — the first (riskiest) mandatory and earliest, the rest where each becomes judgeable; a
    clause given no show can't be closed at phase close (no `F-NN` to cite — `next-task.md` §6), so leave
    it showless only if the phase deliberately defers it. A phase with **no** `[discovered]` clause (a
    bug-batch, a purely `[declared]` refinement) needs no forced show — its `AC-N` + the phase-close
    try-it bind it.
  - Tasks otherwise have **no** deliverable or tests of their own; **a `show` is the deliberate
    exception** — it carries a runnable slice + a "How to see it" (still no full suite — that stays at the
    phase close).

## 5. Sign-off (confirm questions done + defaults taken)
Before closing (`GRILLING.md` → "Confirm before you stop"): (1) for a refinement phase, **confirm the
phase's `kind`** (build / research) for the user's veto — *"I'm opening this as a {build|research} phase
because {the queued set is capabilities/bugs | it's an open question / experiments}"*; (2) name any areas
still worth probing and let the user decide whether to keep grilling; (3) **surface the
high/medium-blast defaults** you recorded, for veto; (4) when the phase has a `[discovered]` clause,
**surface where the first show lands and why it can't be earlier** — *"you won't see {the riskiest
discovered clause} until task {N}, because {the foundation that must exist first}; everything after
thickens it"* — for the user's veto. This is the control that stops the first (riskiest) look from
drifting to the end. Only when "Open questions" is empty and the user is satisfied, set `state: building`
and update the ROADMAP phase-log line.

## Close — ritual + breadcrumb
Run the close ritual (`CLOSE-FORMAT.md`): persist BLUEPRINT / ASSUMPTIONS / SPEC / PROGRESS / ROADMAP,
append a `JOURNAL.md` entry, then the breadcrumb:
- **Done** (`building`): *"Phase {N} designed: {M} tasks in PROGRESS. Next: `/clear` then
  `/gg:next-task` for task 1."*
- **Cut mid-way** (`scoping`): *"Phase {N} discovery checkpointed; {K} open questions left, next is
  '{question}'. Continue with `/clear` + `/gg:discover`."*
