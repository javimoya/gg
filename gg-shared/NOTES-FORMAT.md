# NOTES.md format

`.gg/NOTES.md` is the project's **refinement backlog** — the ideas, and bugs, you capture (with
`/gg:capture` or inline during `/gg:next-task`) once a product exists, waiting for a future phase. `/gg:discover`
triages it at the start of a refinement phase: you pick which notes the phase includes, it grills them
together, and on phase close they move to `## Applied`.

## Structure

```md
# NOTES — {Project name}

## Pending
{Captured, not yet built. Newest first. One `###` block per note. Free-form ideas — NOT typed
add/change/remove. A note that is a **defect in already-shipped behavior** is prefixed `[bug]` in its
title — the one optional marker, so triage can see what's broken apart from a new idea.}

### {short title}
- **Captured**: {YYYY-MM-DD}, {who raised it / which session}
- **Idea**: {the change/addition/removal/fix, in the user's words, plus any why}
- **Touches**: {the area(s) of the product it affects, if known — helps group at triage}
- **Relates**: {optional, set by capture's reconciliation — "refines: {note}" / "contradicts: {note}"
  / "reverses: A-NN"}

## Applied
{Built and shipped. Moved here whole on phase close — the trace stays visible, never deleted.}

### {short title} — applied phase {N} ({YYYY-MM-DD})
- **Landed**: {AC-N / the tasks / where it went}
```

## Rules

- **Free-form, not categorized.** A note is an idea in the user's words; gg does not force an
  add/change/remove type. The shaping happens later in `/gg:discover`, not at capture. The **one
  optional marker** is `[bug]` in the title, for a defect in already-shipped behavior — broken is not a
  new idea, so triage can prioritize it; for a `[bug]` the **Idea** line names what's broken vs. what's
  expected, in the user's words. Everything else stays untyped.
- **Capture reconciles against the backlog** (`CAPTURE.md`): a new idea may **fold into** an existing
  pending note, be flagged as **contradicting** one (and reconciled with the user), or stand alone —
  recorded in `Relates`. The point is a coherent backlog, not a blind append; but capture never grills.
- **`/gg:discover` consumes by selection.** At a refinement phase it reads `## Pending`, the user picks
  the set for this phase, discover grills them together, and on phase close the selected notes move to
  `## Applied` with where they landed. Unselected notes stay pending (never dropped).
- **A note that reverses a recorded default** points at the `A-NN` it overturns (`reverses: A-NN`);
  when the note is applied, that assumption moves to `ASSUMPTIONS.md ## Overridden`.
- **Moved, not deleted.** Applied notes go to `## Applied`; nothing is erased — same discipline as the
  ROADMAP changelog and the assumptions ledger.
- **Created lazily** at the first capture, with the `# NOTES — {name}` header. No secrets — record the
  *name* of an env var, never its value.
