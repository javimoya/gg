---
description: Triages the gg backlog as one reviewed report, then a single decision — not a per-item walk. By default it reads every NEW item in .gg/BACKLOG.md and presents one report — each item with its idea, whether it's a [bug], who raised it, what it touches, and the agent's recommended disposition (next phase / later / future / discard). Then it asks ONE question — accept the recommendations, send only the bugs to the next phase, send everything, or decide item by item — and applies it in a single pass. Every item carries a stable B-NN id so you can reference it. Idempotent — already-triaged items are never re-presented; pass --later or --future to review those deferral tiers on purpose. Run it after a phase ships, before /gg:discover, so discover designs exactly what you queued. Phase 0 has no backlog to refine.
model: inherit
disable-model-invocation: true
argument-hint: "[--later | --future]"
---

# /gg:refine-backlog — Triage the backlog

You groom the refinement backlog so the next phase is built from a deliberate, reviewed set. You
**triage — you do not design, grill, or build**: you read the section, give **every item a recommended
disposition in one report** (next phase / later / future / discard), then take **one decision** from the
user and apply it in a single pass. `/gg:discover` designs whatever you queue here. You work in the
project directory (cwd); state lives in `<cwd>/.gg/`. Shared protocols in
`${CLAUDE_PLUGIN_ROOT}/gg-shared/`: `BACKLOG-FORMAT.md`, `CONSTITUTION.md`, `VISION-FORMAT.md`,
`CLOSE-FORMAT.md`, `LEDGERS.md`.

`/gg:refine-backlog` reports **`## New`** by default; `--later` reports **`## Later`**, `--future`
reports **`## Future`** (review a deferral tier on purpose). The two deferral tiers: `## Later` = not now
but on the radar; `## Future` = someday / maybe, parked further out.

## 0. Precondition
Read `.gg/ROADMAP.md`'s header (`state` / `phase` / `stage`) — the `## State` block + `## Phase log`,
never the changelog (`LEDGERS.md`):
- **No `.gg/`** → no project yet. **Stop**, route to `/gg:ideate`.
- **`state: visioning` or `scoping`** → you're mid-`/gg:ideate` / `/gg:discover`; an idea right now
  belongs in the live grilling, and there's no backlog to triage yet. **Stop** and route there.
- **`state: building`** → a phase is mid-build. Triaging now opens nothing — the queued set is consumed
  by the **next** `/gg:discover`, not this build. **Default: stop** and route to `/gg:next-task`, unless
  the user explicitly wants to groom the backlog now. Even while grooming a build, **never move items into
  `## Next phase`** — that section holds the in-flight set and archives whole to `## Applied` at phase close
  (`next-task.md` §6), so a freshly queued item would be falsely archived as applied; triage only to
  `## Later` / `## Future` / discard, and leave next-phase promotion for after the phase ships.
- **`state: shipped`** → proceed. This is the between-phases grooming step.

If `.gg/BACKLOG.md` is missing, or the section this run reports is empty (`## New` by default, or the
`## Later` / `## Future` tier named by the flag): **stop** — nothing to triage. Before stopping, if other
tiers hold items, name them and the `--later` / `--future` flag that reviews them (so deferred work is
never left invisible); otherwise tell the user to `/gg:capture` an idea first.

## 1. Orient + summary
- Read `.gg/VISION.md` (the destination, to judge what's in or out of the product) and
  `.gg/BACKLOG.md`. No `PRINCIPLES.md` load here — the constitutional rules triage needs are already
  stated in this spec (recommend discard sparingly; decompose, don't drop; a discard is a **recorded**
  product decision, never a silent cut).
- **Open with a one-line summary** of the backlog by state — across **all** sections, so deferred items
  are never invisible: *"Backlog: {N} new · {M} queued for the next phase · {K} later · {F} future ·
  (archive: {A} applied, {D} discarded)."* The user sees the whole shape before triaging.
- **Read the argument** `$ARGUMENTS` to pick which section to report (gate on the literal value):
  **empty** → `## New`; **`--later`** → `## Later`; **`--future`** → `## Future`. The dispositions are
  the same wherever you report; only which tier counts as "stay put" differs (§2).
- **Note the ids.** Every item already carries a stable `B-NN`, assigned at `/gg:capture`
  (`BACKLOG-FORMAT.md`). They are how the user references items in the decision (§3) — you read them,
  you do not assign or change them.

## 2. Build the triage report (one report — not a walk)
Read the whole reported section and present **one report**, newest first, one entry per item — so the
user sees the full set at once instead of being asked one at a time:
- **Each entry**: its `B-NN`; the title (prefixed `[bug] ` for a shipped-behavior defect, `[exp] ` for
  the next experiment to run); who raised it (`user` / `agent`) and when; the idea in the user's words;
  what it `Touches`; any `Relates` / `reverses: A-NN`; and if it cites an `F-NN` that contradicts a "done
  and perfect" clause, flag it as a **correction-from-evidence** the next `/gg:discover` will apply to the
  VISION (`CONSTITUTION.md` → "Boundaries vs. cuts").
- **`[exp]` items map onto the normal dispositions** — no separate science vocabulary: next phase = run
  it (a `## Next phase` of `[exp]` items / an open question opens a **`research`** phase at
  `/gg:discover`, `ROADMAP-FORMAT.md`); later / future = defer the line; discard = abandon it (a recorded
  decision, with the reason).
- **Your recommended disposition + one line of why** — a report without a recommendation per item is
  work pushed back onto the user (`GRILLING.md`). From `## New`: next phase / later / future / discard.
  From `## Later`: promote to next phase / keep in later / push to future / discard. From `## Future`:
  promote / pull back to later / keep / discard.
- **Recommend discard sparingly.** A discard drops the item from the product — recommend it only when
  the item plainly falls outside `VISION.md` or duplicates a kept item; when unsure, recommend a
  deferral tier, never discard (decompose, don't drop — `CONSTITUTION.md`). For any item you do
  recommend discarding, **flag it distinctly and show the proposed one-line reason**, so accepting the
  report is an informed, recorded decision — never a silent drop.
- **Do not design, grill, or build.** This is disposition reasoning only — `/gg:discover` shapes
  whatever gets queued. If the user starts shaping an item, redirect there.

## 3. One decision — then apply in a single pass
Ask **one** question (the harness question tool; leave the door open to a free-form answer — the best
answer often isn't on the menu). Lead with the recommendation:
1. **Accept the recommendations** *(recommended)* — apply each item's recommended disposition exactly. A
   recommended discard uses the reason shown in the report (your acceptance is the recorded decision).
2. **Only the bugs → next phase** *(offer this only when the reported set has at least one `[bug]`)* —
   move the `[bug]` items to `## Next phase` and **leave every other item exactly where it is**; that
   ends the run. For when you want the next phase to be bug-fixes only and to keep deliberating on the rest.
3. **Everything → next phase** — move every reported item to `## Next phase`.
4. **Decide item by item** — the user names the exceptions by id (e.g. *"B-07 and B-06 → next phase, B-05
   → later, discard B-04"*); every item not named follows its recommended disposition. Ask for the
   one-line reason on any discard the user adds.

Then **apply the chosen dispositions in one batch** — move each item's `###` block to its target section,
**keeping its `B-NN`**; a discard moves to `.gg/BACKLOG-ARCHIVE.md ## Discarded` with its one-line
**Why**. A discard is a recorded decision, never a silent drop. (If the user frames an item as "the
product will never do this", that's a `VISION.md` boundary — offer to note it in "It is not"; otherwise
archiving is enough.)

## Close — ritual + breadcrumb
No `state` change — triage doesn't open a phase (`/gg:discover` does). Run the close ritual
(`CLOSE-FORMAT.md`): persist `.gg/BACKLOG.md` and `.gg/BACKLOG-ARCHIVE.md`, append a `JOURNAL.md` entry
recording the dispositions taken (cite the `B-NN`s; `State change: —`), then the breadcrumb:
- **Something queued**: *"Backlog refined: {X} queued for the next phase, {Y} later, {W} future, {Z}
  discarded. Next: `/clear` then `/gg:discover` to design the next phase."*
- **Nothing queued** (all later/discarded): *"Backlog refined: nothing queued for now. `/gg:capture`
  more, or come back later — no phase to open yet."*
