---
description: Jots an idea into the gg backlog (.gg/NOTES.md) the moment it surfaces, so it isn't lost to Claude's memory or a future you'll forget. It is deliberately light — it records the idea and lightly reconciles it against the pending backlog (folds a refinement into an existing note, flags a contradiction) but does NOT grill or design; the shaping happens later in /gg:discover's triage. Use it once a product exists (during /gg:next-task or between phases); it refuses while you're mid-ideate or mid-discover (raise it in the grilling instead).
model: inherit
disable-model-invocation: true
argument-hint: "[the idea]"
---

# /gg:capture — Jot an idea into the backlog

You record an idea that surfaced **on the fly** into `.gg/NOTES.md`, so it's never lost to Claude's
native memory. You do **no grilling, no design, no implementation** — you jot and lightly reconcile,
then get out of the way. You work in the project directory (cwd); state lives in `<cwd>/.gg/`. This is
the standalone entry to the shared protocol (`${CLAUDE_PLUGIN_ROOT}/gg-shared/CAPTURE.md`); the inline
entry — an idea during `/gg:next-task` — applies the same protocol without leaving the session.

## 0. Precondition
Read `.gg/ROADMAP.md`'s header:
- **No `.gg/`** → no project yet. **Stop**: route to `/gg:ideate`.
- **`state: visioning`** → you're mid-`/gg:ideate`. **Stop**: an idea right now belongs **in the live
  ideation grilling**, not the backlog — route the user there.
- **`state: scoping`** → you're mid-`/gg:discover`. **Stop** and tell the user to **raise it in the
  current grilling** — that's where scope is decided, not the backlog.
- **`state: building` or `shipped`** → proceed (a product exists; this is the right time to jot).

## 1. Jot + lightly reconcile (`CAPTURE.md`)
- Read `.gg/NOTES.md ## Pending` (create the file lazily if missing).
- Write the idea per `NOTES-FORMAT.md`: the idea in the user's words, the date, the area it touches.
- **Reconcile** against the backlog — the only "smarts", and never grilling: if it refines/duplicates a
  pending note, offer to **fold it in**; if it **contradicts** one, surface that and let the user
  reconcile; otherwise add it standalone. Record the relationship on the note's `Relates` line. If it
  **reverses a recorded default**, point it at that `A-NN`.
- **Never** write to Claude's native memory; **never** mutate the ROADMAP or open a phase (that's
  `/gg:discover`'s triage); **never** derail — if you're inline in a build, return to your task.

## Close
No state change, no `JOURNAL.md` entry — the dated note in `.gg/NOTES.md` is the record. End with a
one-line confirmation, not the full ritual:
- *"Noted in the backlog ({short title}). It'll be triaged when you next `/gg:discover`. Carry on."*
