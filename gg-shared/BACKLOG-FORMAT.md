# BACKLOG.md format

`.gg/BACKLOG.md` is the project's **active refinement backlog** — the unprocessed items (ideas, bugs,
and scope deferred from `/gg:ideate` / `/gg:discover` / `/gg:next-task`) recorded once a product exists,
waiting to be triaged into a future phase. It stays **lean**: closed items (applied or discarded) move
out to `.gg/BACKLOG-ARCHIVE.md`, so what's left is only live work.

Four commands act on it: `/gg:capture` (and the agent's inline jot) **adds** items — always to
`## New`, assigning each a stable `B-NN` id; `/gg:refine-backlog` **triages** — it reports the items
with a recommended disposition each and applies the user's one decision in a single pass (→ next phase /
later / future / discarded); `/gg:discover` **consumes** the `## Next phase` set when it opens a
refinement phase; and `/gg:quick` (the express lane) **records a single item straight to `## Next phase`**
— skipping `## New` and the triage step — then designs it immediately. There is no in-discover triage
question anymore — `/gg:refine-backlog` owns triage.

## The item lifecycle

```text
new ──(refine-backlog)──► next-phase ──(discover designs it; phase ships)──► applied   (→ ARCHIVE)
 │                        ▲
 │      (quick) ──────────┘  records a single item straight to next-phase (the express lane)
 ├──(refine-backlog)──► later         (on the radar; revisit with --later)
 ├──(refine-backlog)──► future        (someday / maybe; revisit with --future)
 └──(refine-backlog)──► discarded                                          (→ ARCHIVE, with a reason)
```

An item's **state is its section** — `## New`, `## Next phase`, `## Later`, or `## Future`. A freshly
recorded item is always `## New` — that is what makes `/gg:refine-backlog` **idempotent**: by default it
reports only `## New`, so an item you already triaged is never presented again. The two deferral tiers
are revisited on purpose (`--later` / `--future`), and every run's opening summary shows the counts of
all sections, so nothing is ever invisible. The item's `B-NN` id is **stable** — it travels with the
item across every section and into the archive, and is never reused (see Rules).

## Structure — `.gg/BACKLOG.md` (active)

```md
# Backlog — {Project name}

## New
{Recorded, NOT yet triaged. Newest first. `/gg:capture` and the agent's inline jot write here. One
`###` block per item. Free-form — NOT typed add/change/remove. A defect in already-shipped behavior is
prefixed `[bug]` in its title (the one optional marker).}

### B-NN — {short title}   <!-- prefix the title with "[bug] " if it's a defect in shipped behavior -->
- **Captured**: {YYYY-MM-DD} — {user | agent}, via {command, e.g. /gg:capture or /gg:next-task task 4}
- **Idea**: {the idea/change/bug, in the user's words, plus any why; for a `[bug]`, what's broken vs. expected}
- **Touches**: {the area(s) of the product it affects, if known}
- **Relates**: {optional, set by capture's reconciliation — "refines: B-03" / "contradicts: B-05" / "reverses: A-NN"}

## Next phase
{Triaged by `/gg:refine-backlog` as "do next" (or recorded straight here by `/gg:quick`, the express
lane). `/gg:discover` consumes this set, grills the items together, and on phase close they move to the
archive's `## Applied`. Same `###` block shape as New.}

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

### B-NN — {short title} — applied phase {N} ({YYYY-MM-DD})
- **Landed**: {AC-N / the tasks / where it went}

## Discarded
{Explicitly dropped in `/gg:refine-backlog` — a recorded decision, not a silent cut.}

### B-NN — {short title} — discarded ({YYYY-MM-DD})
- **Why**: {the recorded reason — the agent's proposed reason the user accepted, or the user's own}
```

## Rules

- **State = section.** A new item is triaged to `## Next phase`, `## Later`, or `## Future`, or is
  discarded (archived). `/gg:capture` and the agent only ever write `## New`; only `/gg:refine-backlog`
  moves items between sections (and to the archive's `## Discarded`); only a phase close
  (`/gg:next-task` §6) moves `## Next phase` items to the archive's `## Applied`. **The one exception is
  `/gg:quick`**, which records its single express item **directly to `## Next phase`** (skipping
  `## New`) — the express auto-triage — and only when `## Next phase` is otherwise empty, so the item is
  the whole micro-phase.
- **Stable, never-reused `B-NN` ids.** Every item carries a `B-NN` id, assigned at `/gg:capture`. The id
  is **stable**: it travels with the item through every section and into the archive, and **never
  changes**. It is **never reused** — the next id is one past the highest `B-NN` found **anywhere** in
  `.gg/BACKLOG.md` *and* `.gg/BACKLOG-ARCHIVE.md`, so a discarded item's id is retired, never handed out
  again. (Compute the next id from both files; never renumber an existing one.) Same `*-NN` discipline as
  the assumptions ledger's `A-NN`.
- **Free-form, not categorized.** An item is an idea in the user's words; gg does not force an
  add/change/remove type. The one optional marker is `[bug]` in the title, for a defect in
  already-shipped behavior — broken is not a new idea, so triage can prioritize it; for a `[bug]` the
  **Idea** line names what's broken vs. what's expected. Everything else stays untyped.
- **Idempotent triage, with deferral tiers.** `/gg:refine-backlog` reports only `## New` by default, so a
  triaged item is never re-presented. The deferral tiers are revisited on purpose: `--later` reports
  `## Later` (on the radar), `--future` reports `## Future` (someday / maybe). Every run opens with the
  counts across all sections, so a deferred item is never invisible — never a black hole.
- **Capture reconciles against the active backlog** (`CAPTURE.md`): a new item may **fold into** an
  existing one, be flagged as **contradicting** one, or stand alone — recorded in `Relates` by the other
  item's `B-NN` (`refines: B-03` / `contradicts: B-05`). Reconcile over the whole active backlog (New +
  Next phase + Later), never the archive; capture never triages.
- **An item that reverses a recorded default** points at the `A-NN` it overturns (`reverses: A-NN`);
  that assumption moves to `ASSUMPTIONS.md ## Overridden` only when the item is later applied.
- **Moved, not deleted.** Applied and discarded items go to `.gg/BACKLOG-ARCHIVE.md`; nothing is
  erased — same discipline as the assumptions ledger and the ROADMAP changelog.
- **Created lazily.** `BACKLOG.md` at the first capture/jot (with `# Backlog — {name}` and `## New` — or
  a `## Next phase` section when `/gg:quick` is the first writer); `BACKLOG-ARCHIVE.md` at the first
  applied/discarded item. No secrets — record the *name* of an env
  var, never its value.
