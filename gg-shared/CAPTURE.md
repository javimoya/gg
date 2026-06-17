# Capturing an idea into the backlog (shared)

This is how a new idea, change, refinement, **or a bug in the shipped product** gets recorded **the
moment it surfaces** — whether you thought of it or the user raised it. The thread never lives only in your head, in prose, or in
Claude's native memory. It is the lightest form of the constitution's **"decompose, don't drop"**
(`CONSTITUTION.md`): capture **jots and reconciles — it does not grill**.

Two entry points share this protocol:
- **Standalone** — `/gg:capture`, run once a product already exists (during `/gg:next-task` or between
  phases).
- **Inline** — an idea surfaces *while `/gg:next-task` is running*; you jot it without leaving the
  session, then return to the task you were on.

## The iron rule
- **Never Claude native memory.** No `MEMORY.md`, no `~/.claude/**/memory/`. `.gg/` is the only memory.
- **Never a silent drop**, and never "I'll remember it later" as the only record. A thread parked
  outside `.gg/` is a cut.

## What capture does (jot + light reconciliation — no grilling)
1. **Write the idea to `.gg/BACKLOG.md ## New`** (create the file if missing) per `BACKLOG-FORMAT.md`:
   the idea in the user's words, the date, who raised it (`user`/`agent`), and the area it touches if
   known. If it's a **defect in already-shipped behavior**, prefix the title `[bug]` and record what's
   broken vs. what's expected (in the user's words) — it is still only jotted, triaged later by
   `/gg:refine-backlog` and fixed via `/gg:discover`, never patched on the spot.
2. **Reconcile against the active backlog** (New + Next phase + Later) — the "little more" beyond a
   blind append:
   - if it refines or duplicates an existing item → offer to **fold it in** (merge the nuance);
   - if it contradicts one → **surface the contradiction** and let the user reconcile;
   - otherwise → add it standalone.
   Record the relationship on the item's `Relates` line so the backlog stays coherent.
3. **If it reverses a recorded default**, point the item at that `A-NN` (`reverses: A-NN`); the
   assumption moves to `ASSUMPTIONS.md ## Overridden` only when the item is later applied.

This is **not** triage or design. Which items a phase includes is decided by `/gg:refine-backlog`
(triage, one item at a time), and how they're shaped by `/gg:discover` (design). Capture only records
and relates.

## When capture is NOT the move
- **No product yet** (no `.gg/` or `state: visioning` — mid-`/gg:ideate` — or `state: scoping` —
  mid-`/gg:discover`): an idea right now belongs **in the live grilling**, not the backlog. Redirect
  the user there.
- **It's a cut you were tempted to make** in the current task — that's the constitution's anti-cut
  reframe (do it right, or a later task of this phase), not a note.
- **It's a bug in the task you're building right now** — fix it as part of doing this task to the bar
  (the same anti-cut reframe), don't capture it. Capture a `[bug]` only for a defect in **already-
  shipped** behavior (or another area), to be fixed in a later phase.

## Don't derail
Capturing is a quick interjection. Inline, you **return to the task you were on** — you are not
switching to build the new thing now. New scope is always handled in a *later* phase.
