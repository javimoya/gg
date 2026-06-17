# ASSUMPTIONS.md format

`.gg/ASSUMPTIONS.md` is the **recorded-defaults ledger** — every decision `/gg:discover` took *for*
the user instead of grilling them, written down so the speed of "good defaults" never becomes a silent
cut. It is what makes "complete, close-not-perfect, then refine" honest: the gaps are visible and
reversible, not hidden.

## Structure

```md
# ASSUMPTIONS — {Project name}

## Open
{Defaults taken instead of grilling. Newest first. One `###` block each. Reversible by a note.}

### A-01 — {short title}
- **Phase**: {0 | N}
- **Question not asked**: {the decision we didn't put to the user}
- **Default taken**: {what we assumed}
- **Why**: {why it's a reasonable default}
- **Reverse it**: `/gg:capture {the idea that overturns it}`
- **Blast radius**: {low | medium} — {one line: what changes if reversed}

## Overridden
{A default a note later reversed. Moved here whole — the trace stays visible, never deleted.}

### A-01 — {short title} — overridden {YYYY-MM-DD}
- **Was**: {the default} · **Now**: {what the note decided} · **In**: phase {N}
```

## Rules

- **Append-only, numbered, reversible.** `A-NN` IDs are stable. A default is legitimate *only* because
  it is recorded, attributed, and cheaply reversible by a note. The cut is the *unrecorded* assumption
  (`CONSTITUTION.md` → "Defaults and assumptions").
- **High-blast decisions never live here.** They are grilled questions, not defaults. If you can't
  cheaply reverse a choice with a note (the data model, the stack, sync vs async, the core UX), ask —
  don't log it as an assumption. Only `low`/`medium` blast-radius choices belong here.
- **Surfaced at the discover sign-off.** Before `/gg:discover` closes, it shows the `high`/`medium`
  defaults explicitly for the user to veto or change; the `low` ones are listed but not walked one by
  one. This keeps the ledger a real control, not a wall nobody reads.
- **An assumption is not an inferred AC.** It decides *what to build*, on the record — it never
  licenses claiming an acceptance criterion passed without evidence (that stays forbidden;
  the phase-close `JOURNAL` acceptance evidence must be real).
- **Reversed, not deleted.** When a backlog item overturns a default, move its block to `## Overridden` —
  same move-don't-erase discipline as the backlog archive and the ROADMAP changelog.
- **Created lazily** at the first default recorded, with the `# ASSUMPTIONS — {name}` header. No
  secrets — record the *name* of an env var, never its value.
- **The launch flip seeds assumptions into work.** The data-defensiveness skipped in `dev` is
  recorded here; flipping to `launched` (`STAGE.md`) consumes those entries into launch-readiness
  backlog items — sequencing, not a cut.
