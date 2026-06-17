# BACKLOG.md format

`.gg/BACKLOG.md` is the project's **active refinement backlog** — the unprocessed items (ideas, bugs,
and scope deferred from `/gg:ideate` / `/gg:discover` / `/gg:next-task`) recorded once a product exists,
waiting to be triaged into a future phase. It stays **lean**: closed items (applied or discarded) move
out to `.gg/BACKLOG-ARCHIVE.md`, so what's left is only live work.

Three commands act on it: `/gg:capture` (and the agent's inline jot) **adds** items — always to
`## New`; `/gg:refine-backlog` **triages** each new item one at a time (→ next phase / later /
discarded); and `/gg:discover` **consumes** the `## Next phase` set when it opens a refinement phase.
There is no in-discover triage question anymore — `/gg:refine-backlog` owns triage.

## The item lifecycle

```text
new ──(refine-backlog)──► next-phase ──(discover designs it; phase ships)──► applied   (→ ARCHIVE)
 │
 ├──(refine-backlog)──► later         (on the radar; revisit with --later)
 ├──(refine-backlog)──► future        (someday / maybe; revisit with --future)
 └──(refine-backlog)──► discarded                                          (→ ARCHIVE, with a reason)
```

An item's **state is its section** — `## New`, `## Next phase`, `## Later`, or `## Future`. A freshly
recorded item is always `## New` — that is what makes `/gg:refine-backlog` **idempotent**: by default it
walks only `## New`, so an item you already triaged is never presented again. The two deferral tiers are
revisited on purpose (`--later` / `--future`), and every run's opening summary shows the counts of all
sections, so nothing is ever invisible.

## Structure — `.gg/BACKLOG.md` (active)

```md
# Backlog — {Project name}

## New
{Recorded, NOT yet triaged. Newest first. `/gg:capture` and the agent's inline jot write here. One
`###` block per item. Free-form — NOT typed add/change/remove. A defect in already-shipped behavior is
prefixed `[bug]` in its title (the one optional marker).}

### {short title}   <!-- prefix with "[bug] " if it's a defect in shipped behavior -->
- **Captured**: {YYYY-MM-DD} — {user | agent}, via {command, e.g. /gg:capture or /gg:next-task task 4}
- **Idea**: {the idea/change/bug, in the user's words, plus any why; for a `[bug]`, what's broken vs. expected}
- **Touches**: {the area(s) of the product it affects, if known}
- **Relates**: {optional, set by capture's reconciliation — "refines: {item}" / "contradicts: {item}" / "reverses: A-NN"}

## Next phase
{Triaged by `/gg:refine-backlog` as "do next". `/gg:discover` consumes this set, grills the items
together, and on phase close they move to the archive's `## Applied`. Same `###` block shape as New.}

## Later
{Triaged as "not now, but on the radar". Revisit with `/gg:refine-backlog --later`. Same `###` block shape.}

## Future
{Triaged as "someday / maybe" — parked further out. Revisit with `/gg:refine-backlog --future`. Same
`###` block shape.}
```

## Structure — `.gg/BACKLOG-ARCHIVE.md` (closed, history)

```md
# Backlog archive — {Project name}

## Applied
{Built and shipped. Moved here whole on phase close — the trace stays visible, never deleted.}

### {short title} — applied phase {N} ({YYYY-MM-DD})
- **Landed**: {AC-N / the tasks / where it went}

## Discarded
{Explicitly dropped in `/gg:refine-backlog` — a recorded decision, not a silent cut.}

### {short title} — discarded ({YYYY-MM-DD})
- **Why**: {the user's reason}
```

## Rules

- **State = section.** A new item is triaged to `## Next phase`, `## Later`, or `## Future`, or is
  discarded (archived). `/gg:capture` and the agent only ever write `## New`; only `/gg:refine-backlog`
  moves items between sections (and to the archive's `## Discarded`); only a phase close
  (`/gg:next-task` §6) moves `## Next phase` items to the archive's `## Applied`.
- **Free-form, not categorized.** An item is an idea in the user's words; gg does not force an
  add/change/remove type. The one optional marker is `[bug]` in the title, for a defect in
  already-shipped behavior — broken is not a new idea, so triage can prioritize it; for a `[bug]` the
  **Idea** line names what's broken vs. what's expected. Everything else stays untyped.
- **Idempotent triage, with deferral tiers.** `/gg:refine-backlog` walks only `## New` by default, so a
  triaged item is never re-presented. The deferral tiers are revisited on purpose: `--later` walks
  `## Later` (on the radar), `--future` walks `## Future` (someday / maybe). Every run opens with the
  counts across all sections, so a deferred item is never invisible — never a black hole.
- **Capture reconciles against the active backlog** (`CAPTURE.md`): a new item may **fold into** an
  existing one, be flagged as **contradicting** one, or stand alone — recorded in `Relates`. Reconcile
  over the whole active backlog (New + Next phase + Later), never the archive; capture never triages.
- **An item that reverses a recorded default** points at the `A-NN` it overturns (`reverses: A-NN`);
  that assumption moves to `ASSUMPTIONS.md ## Overridden` only when the item is later applied.
- **Moved, not deleted.** Applied and discarded items go to `.gg/BACKLOG-ARCHIVE.md`; nothing is
  erased — same discipline as the assumptions ledger and the ROADMAP changelog.
- **Created lazily.** `BACKLOG.md` at the first capture/jot (with `# Backlog — {name}` and `## New`);
  `BACKLOG-ARCHIVE.md` at the first applied/discarded item. No secrets — record the *name* of an env
  var, never its value.
