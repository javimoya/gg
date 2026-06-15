# Capturing an idea into the backlog (shared)

This is how a new idea, change, or refinement gets recorded **the moment it surfaces** — whether you
thought of it or the user raised it. The thread never lives only in your head, in prose, or in
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
1. **Write the idea to `.gg/NOTES.md ## Pending`** (create the file if missing) per `NOTES-FORMAT.md`:
   the idea in the user's words, the date, and the area it touches if known.
2. **Reconcile against the existing backlog** — the "little more" beyond a blind append:
   - if it refines or duplicates a pending note → offer to **fold it in** (merge the nuance);
   - if it contradicts a pending note → **surface the contradiction** and let the user reconcile;
   - otherwise → add it standalone.
   Record the relationship on the note's `Relates` line so the backlog stays coherent.
3. **If it reverses a recorded default**, point the note at that `A-NN` (`reverses: A-NN`); the
   assumption moves to `ASSUMPTIONS.md ## Overridden` only when the note is later applied.

This is **not** triage or design. Which notes a phase includes, and how they're shaped, is decided
later by `/gg:discover` (the triage gate + grilling). Capture only records and relates.

## When capture is NOT the move
- **No product yet** (`state ∈ {visioning, scoping}` — you're mid-`/gg:ideate` or mid-`/gg:discover`):
  an idea right now belongs **in the live grilling**, not the backlog. Redirect the user there.
- **It's a cut you were tempted to make** in the current task — that's the constitution's anti-cut
  reframe (do it right, or a later task of this phase), not a note.

## Don't derail
Capturing is a quick interjection. Inline, you **return to the task you were on** — you are not
switching to build the new thing now. New scope is always handled in a *later* phase.
