---
description: Triages the gg backlog one item at a time. By default walks each NEW item in .gg/BACKLOG.md — explains it (idea, [bug]/feature, who raised it, what it touches) — and asks you to send it to the next phase, keep it for later, park it for the future, or discard it (archived with a reason). Idempotent — already-triaged items are never re-presented; pass --later or --future to revisit those deferral tiers on purpose. Run it after a phase ships, before /gg:discover, so discover then designs exactly what you queued. Phase 0 has no backlog to refine.
model: inherit
disable-model-invocation: true
argument-hint: "[--later | --future]"
---

# /gg:refine-backlog — Triage the backlog

You groom the refinement backlog so the next phase is built from a deliberate, reviewed set. You
**triage — you do not design, grill, or build**: each item gets one of four dispositions (next phase /
later / future / discard) and that's it. `/gg:discover` designs whatever you queue here. You work in
the project directory (cwd); state lives in `<cwd>/.gg/`. Shared protocols in
`${CLAUDE_PLUGIN_ROOT}/gg-shared/`: `BACKLOG-FORMAT.md`, `CONSTITUTION.md`, `VISION-FORMAT.md`,
`CLOSE-FORMAT.md`.

`/gg:refine-backlog` walks **`## New`** by default; `--later` walks **`## Later`**, `--future` walks
**`## Future`** (revisit a deferral tier on purpose). The two deferral tiers: `## Later` = not now but
on the radar; `## Future` = someday / maybe, parked further out.

## 0. Precondition
Read `.gg/ROADMAP.md`'s header (`state` / `phase` / `stage`):
- **No `.gg/`** → no project yet. **Stop**, route to `/gg:ideate`.
- **`state: visioning` or `scoping`** → you're mid-`/gg:ideate` / `/gg:discover`; an idea right now
  belongs in the live grilling, and there's no backlog to triage yet. **Stop** and route there.
- **`state: building`** → a phase is mid-build. Triaging now opens nothing — the queued set is consumed
  by the **next** `/gg:discover`, not this build. **Default: stop** and route to `/gg:next-task`, unless
  the user explicitly wants to groom the backlog now.
- **`state: shipped`** → proceed. This is the between-phases grooming step.

If `.gg/BACKLOG.md` is missing, or the section this run walks is empty (`## New` by default, or the
`## Later` / `## Future` tier named by the flag): **stop** — nothing to triage. Tell the user to
`/gg:capture` an idea first.

## 1. Orient + summary
- Read `.gg/PRINCIPLES.md` (the bar; decompose-don't-drop; a discard is a **recorded** product
  decision, never a silent cut), `.gg/VISION.md` (the destination, to judge what's in or out of the
  product), and `.gg/BACKLOG.md`.
- **Open with a one-line summary** of the backlog by state — across **all** sections, so deferred
  items are never invisible: *"Backlog: {N} new · {M} queued for the next phase · {K} later · {F}
  future · (archive: {A} applied, {D} discarded)."* The user sees the whole shape before triaging.
- **Read the argument** `$ARGUMENTS` to pick which section to walk (gate on the literal value):
  **empty** → `## New` (§2); **`--later`** → `## Later`; **`--future`** → `## Future` (§3). The
  dispositions are the same wherever you walk.

## 2. Walk the NEW items, one at a time (idempotent)
For each item in `## New` (newest first) — **one at a time, wait for the answer before the next**:
- **Explain it**: the idea in the user's words; whether it's a `[bug]` or a feature; who raised it
  (`user` / `agent`); what it `Touches`; any `Relates` / `reverses: A-NN`. Give your **recommended
  disposition** with one line of why — a question without a recommendation is work pushed back onto the
  user (`GRILLING.md`).
- Take the disposition (move the item's `###` block):
  - **next phase** → `## Next phase`.
  - **later** → `## Later` (on the radar).
  - **future** → `## Future` (someday / maybe).
  - **discard** → `.gg/BACKLOG-ARCHIVE.md ## Discarded` with a one-line **Why** (the user's reason). A
    discard is a recorded decision — never a silent drop. (If the user frames it as "the product will
    never do this", that's a VISION boundary — offer to note it in `VISION.md`'s "It is not"; otherwise
    archiving is enough.)
- **Do not design, grill, or build.** This is disposition only. If the user starts shaping an item,
  redirect: the design happens in `/gg:discover` once the item is queued.
- Walk **only `## New`** (no flag). Already-triaged items (`Next phase` / `Later` / `Future` / archived)
  are **not** re-presented — this is what makes the command idempotent across clean sessions.

## 3. Revisit a deferral tier (only with a flag)
By default you walk only `## New` — you never nag about the deferral tiers. To reconsider one, the user
runs the command with a flag:
- **`/gg:refine-backlog --later`** → walk `## Later` (newest first), same four dispositions: promote to
  `## Next phase`, keep in `## Later`, push to `## Future`, or discard.
- **`/gg:refine-backlog --future`** → walk `## Future`, same dispositions (promote, pull back to
  `## Later`, keep, or discard).
This is what keeps a deferred item from becoming a black hole: it's never forced on you, but the
summary always shows the counts and a flag walks the tier whenever you want.

## Close — ritual + breadcrumb
No `state` change — triage doesn't open a phase (`/gg:discover` does). Run the close ritual
(`CLOSE-FORMAT.md`): persist `.gg/BACKLOG.md` and `.gg/BACKLOG-ARCHIVE.md`, append a `JOURNAL.md` entry
recording the dispositions taken (`State change: —`), then the breadcrumb:
- **Something queued**: *"Backlog refined: {X} queued for the next phase, {Y} later, {W} future, {Z}
  discarded. Next: `/clear` then `/gg:discover` to design the next phase."*
- **Nothing queued** (all later/discarded): *"Backlog refined: nothing queued for now. `/gg:capture`
  more, or come back later — no phase to open yet."*
