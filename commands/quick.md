---
description: Express lane for a single small change you've already decided to do now — a bug, a tweak. Records it straight into the backlog's ## Next phase with a stable B-NN (skipping the /gg:refine-backlog triage), then runs /gg:discover to design just that one item as its own micro-phase and hands off to /gg:next-task to build it. It does NOT skip the bar — the item is recorded, designed, and built with tests like any phase; only the triage deliberation is skipped, because choosing to run /gg:quick IS the triage. Runs only once a product is shipped and nothing else is queued for the next phase; otherwise it degrades to a plain /gg:capture so the idea is never lost. For a batch of changes, use /gg:capture then /gg:refine-backlog instead.
model: inherit
disable-model-invocation: true
argument-hint: "[the small change to build now]"
---

# /gg:quick — Capture and design one small thing now (the express lane)

You compress the loop for **one small change you've already decided to do** — a bug, a tweak — into a
single step: record it, skip the triage deliberation, and design it on the spot. You do **not** skip the
bar. The item still gets a stable `B-NN`, a design (`/gg:discover`), and a build with tests
(`/gg:next-task`) like any phase — **only `/gg:refine-backlog`'s triage is skipped, because choosing to
run `/gg:quick` *is* the triage** (you decided "do this now"). You work in the project directory (cwd);
state lives in `<cwd>/.gg/`. Protocols in `${CLAUDE_PLUGIN_ROOT}/gg-shared/`: `CAPTURE.md`,
`BACKLOG-FORMAT.md`, `CONSTITUTION.md`, `STAGE.md` — and the `/gg:discover` design protocol
(`discover.md`), which this command runs unchanged.

**For more than one item, this is the wrong command** — use `/gg:capture` (jot each) then
`/gg:refine-backlog` (triage the batch into a coherent phase). The express lane is deliberately
**one item, one micro-phase**.

## 0. Precondition (gate before fast-tracking)
Read `.gg/ROADMAP.md`'s header (`state` / `phase` / `stage`). The express lane runs **only** at
`state: shipped` with an **empty `.gg/BACKLOG.md ## Next phase`** — that is what lets the recorded item
*be* the whole micro-phase, so `/gg:discover` and `/gg:next-task` run with no change to their contract.
Any other state **degrades to a plain `/gg:capture`** (`CAPTURE.md`) so the idea is never lost:
- **No `.gg/`** → no project yet. **Stop**, route to `/gg:ideate`.
- **`state: visioning`** → mid-`/gg:ideate`, no product to change. **Stop**, route to `/gg:ideate`.
- **`state: scoping`** → mid-`/gg:discover`. **Stop** and tell the user to **raise it in the current
  grilling** — that's where scope is decided.
- **`state: building`** → a phase is mid-build; opening a micro-phase now would clobber it. **Jot the
  item to `## New`** per `CAPTURE.md` (which means: a defect in the task being built *now* is a fix, not a
  capture; capture and mark `[bug]` only a defect in already-shipped behavior or another area), then
  **stop** — finish the current phase (`/gg:next-task`), and it's there to `/gg:quick` or triage
  afterward. **Don't derail the build.**
- **`state: shipped`, but `## Next phase` is NOT empty** → a set is already queued for the next phase;
  designing one item in isolation would archive that set wrongly at phase close. **Jot the item to
  `## New`** per `CAPTURE.md`, then **stop**: route to `/gg:discover` to build the already-queued set —
  the just-jotted item waits in `## New` for the next `/gg:refine-backlog`.
- **`state: shipped` and `## Next phase` is empty** → the express lane (§1–§2).

## 1. Record the item straight to `## Next phase` (the express auto-triage)
Apply `CAPTURE.md`'s jot + light reconcile, with **one deviation** — the item (or, if the reconcile folds
it into an existing item, that survivor keeping its `B-NN`) lands in **`## Next phase`**, not `## New`. The
input is `$ARGUMENTS` if passed, else the idea the user just raised. Create `.gg/BACKLOG.md` with a
`# Backlog — {name}` header and both `## New` + `## Next phase` sections if it doesn't exist yet (the common
right-after-phase-0-ship case). Field shape, the `[bug]` prefix, the next stable `B-NN`, and `reverses: A-NN`
are all per `BACKLOG-FORMAT.md` / `CAPTURE.md` — unchanged. Because §0's precondition kept `## Next phase`
empty, it ends holding **exactly this one item**; that placement *is* the skip of `/gg:refine-backlog` (the
item is already triaged to "do next").

## 2. Design it now — run `/gg:discover` for the one-item micro-phase
Run the **`/gg:discover` protocol unchanged** (`discover.md`). With `state: shipped` and `## Next phase`
holding exactly your item, discover opens it as the next phase and designs **only it** — depth scales
(`discover.md` §3): a bug or tweak gets a one-line BLUEPRINT note ("… — no design change"), a new
entity/field/component gets the full detail. Discover writes the SPEC + the ordered task list, records
any defaults as `A-NN`, sets `state: building`, runs its close ritual (`CLOSE-FORMAT.md`), and ends with
its breadcrumb to `/gg:next-task`. The build happens in a **fresh session** — the express lane stops at
design, because **one task per fresh `/gg:next-task` session** stays the rule (`CONSTITUTION.md` →
Context discipline). The item closes out to `BACKLOG-ARCHIVE.md ## Applied` at phase close, like any
other.

## Close — breadcrumb
`/gg:quick` has **no separate close**: `/gg:discover`'s close ritual + breadcrumb end the express run.
Frame it as the express item, e.g.: *"Quick item {B-NN} designed as phase {N}: {M} tasks. `/clear` then
`/gg:next-task` to build it."* If §0 **degraded**, run no ritual and end with a one-line note matching
what actually happened:
- **Jotted** (`building`, or `shipped` with a set already queued): *"A phase is mid-build (or a set is
  already queued); noted {B-NN} in `## New`. {Finish via `/gg:next-task`, or `/gg:discover` the queued
  set}."*
- **Routed away, nothing recorded** (no `.gg/`, `visioning`, `scoping`): *"No product to change here —
  {route to `/gg:ideate`, or raise it in the current grilling}."*
