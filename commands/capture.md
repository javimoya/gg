---
description: Jots whatever just surfaced — a future idea/change/bug into the gg backlog (.gg/BACKLOG.md ## New), or a past-tense observation of what the running product did into .gg/FINDINGS.md (F-NN) — the moment it surfaces, so it isn't lost to Claude's memory or a future you'll forget. It is deliberately light — it records the thing and lightly reconciles it against the active backlog (folds a refinement into an existing item, flags a contradiction) but does NOT grill, triage, or design; triage happens later in /gg:refine-backlog and the design in /gg:discover. Pass --next to queue the item straight into .gg/BACKLOG.md ## Next phase (self-triage to "do next", skipping ## New and /gg:refine-backlog) instead of ## New — it still does not design or build it (that's /gg:discover, or /gg:quick which also designs). Use it once a product exists (during /gg:next-task or between phases); it refuses while you're mid-ideate or mid-discover (raise it in the grilling instead).
model: inherit
disable-model-invocation: true
argument-hint: "[--next] [the idea]"
---

# /gg:capture — Jot an idea into the backlog

You record an idea that surfaced **on the fly** into `.gg/BACKLOG.md ## New`, so it's never lost to
Claude's native memory. You do **no grilling, no design, no implementation** — you jot and lightly reconcile,
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
- **`state: building`** → proceed; jot to `## New`. **`--next` degrades here**: a phase is mid-build, so
  `## Next phase` holds the set being built and archives whole to `## Applied` at phase close
  (`next-task.md` §6) — queuing an undesigned item there would get it falsely archived. So `--next` falls
  back to a plain `## New` jot; say so. The idea is never lost.
- **`state: shipped`** → proceed (a product exists — the right time to jot). A bare jot writes `## New`;
  **`--next`** writes the item straight to `## Next phase` (§1), the self-triage to "do next" that skips
  `/gg:refine-backlog`. Unlike `/gg:quick` it does **not** also design it, and there is **no
  empty-`## Next phase` requirement** — `--next` just queues, so you can assemble a multi-item next phase
  by jotting several.

## 1. Jot + lightly reconcile (`CAPTURE.md`)
Apply the `CAPTURE.md` protocol — jot the thing, then lightly reconcile it; no grilling, no design. The
input is `$ARGUMENTS` if the user passed it as an argument; otherwise it's what they just raised in
conversation (inline during a build, or after a bare `/gg:capture`). **Route by tense first** (`CAPTURE.md`
→ "Idea or observation?"): a *future-tense* idea / change / bug goes to `.gg/BACKLOG.md ## New` per
`BACKLOG-FORMAT.md` (next stable `B-NN`, reconciled against the active backlog — fold in / flag a
contradiction / stand alone); a *past-tense observation* of what the running product did goes to
`.gg/FINDINGS.md` per `FINDINGS-FORMAT.md` (next stable `F-NN`). Create either file lazily if missing.

**`--next` (only at `state: shipped`, only for a backlog item):** the `$ARGUMENTS` flag `--next` is not
part of the idea — strip it. When present, the future-tense item lands in `## Next phase` instead of
`## New` (still its next stable `B-NN`, still the same light reconcile). If the reconcile **folds** the new
wording into an existing **`## New`** item, don't leave a stranded copy: merge the nuance into that survivor, record the
relation (`refines: …`), and **move the survivor — keeping its `B-NN` — into `## Next phase`** (same as
`/gg:quick` §1, minus the design step). A fold that instead matches an already-**deferred** item
(`## Later` / `## Future`) is surfaced and left where triage put it — promoting a deferred item is
`/gg:refine-backlog`'s call (`BACKLOG-FORMAT.md`). `--next` is meaningless for a *past-tense observation* — a finding
has no phase — so ignore it and route to `.gg/FINDINGS.md` as usual.

**Never** write to Claude's native memory; **never** mutate the ROADMAP, triage, or open a phase (triage
is `/gg:refine-backlog`'s job, design is `/gg:discover`'s); **never** derail — if you're inline in a
build, return to your task.

## Close
No state change, no `JOURNAL.md` entry — the dated item is the record (`.gg/BACKLOG.md` — `## New`, or
`## Next phase` with `--next` — for an idea/change/bug, `.gg/FINDINGS.md` for a past-tense observation).
End with a one-line confirmation that names where it landed, not the full ritual:
- backlog item (`## New`): *"Noted in the backlog ({B-NN} — {short title}). It'll be triaged when you next `/gg:refine-backlog`. Carry on."*
- backlog item with `--next` (`## Next phase`): *"Queued for the next phase ({B-NN} — {short title}) — already triaged to \"do next\", so it skips `/gg:refine-backlog`. `/gg:discover` will design it (with anything else queued there). Carry on."*
- `--next` degraded (mid-build): *"A phase is mid-build, so I jotted {B-NN} in `## New` rather than `## Next phase`; it's there to triage once this phase ships. Carry on."*
- finding: *"Recorded as a finding in `.gg/FINDINGS.md` ({F-NN} — {short title}); it's on the record, not triaged. Carry on."* (name any `B-NN` it also spawned.)
