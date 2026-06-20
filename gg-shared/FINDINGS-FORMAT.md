# FINDINGS.md format

`.gg/FINDINGS.md` is the **observations ledger** — what the running product actually *did* when it was
run or tried, recorded the moment it's seen so the learning never dissolves into PROGRESS/JOURNAL prose.
It is the **fourth "decompose, don't drop" home** (`CONSTITUTION.md`): a later task, a backlog item, and
a recorded assumption all capture a **decision not yet made**; a finding captures an **observation
already made**. It is the past-tense sibling of the other three.

## Structure

```md
# FINDINGS — {Project name}

## Findings
{Observations on the record. Newest first. One `###` block each. Append-only; an observation is a fact —
never edited away, only its "Leads to" is updated when it spawns work.}

### F-01 — {short title}
- **Observed**: {YYYY-MM-DD} — {user | agent}, {the run or try-it that produced it, in the project's own
  terms — e.g. "the task-3 show slice" / "the phase-2 deliverable"}
- **What happened**: {the observed result — concrete and measured where it can be, not a guess}
- **Reading**: {one line — what it means / why it matters}
- **Leads to**: {`B-NN` (a backlog item it spawned — the single intake for any change, including a next experiment marked `[exp]` or one that revises the VISION) | `—` (just on the record)}
```

## Rules

- **A finding is an observation, not a decision.** It records what the running product *did* — distinct
  from an `A-NN` assumption (which decides *what to build*, before the fact) and from a `B-NN` backlog
  item (a not-yet-made decision about what to build next). Recording the observation is not itself a
  decision; the decision it may trigger is a separate `B-NN`, linked from **Leads to**.
- **Append-only, numbered, never deleted.** `F-NN` ids are stable and never reused. A finding is a fact
  on the record; you never erase it — when it is acted on you update its **Leads to**, you don't remove
  the block (same move-don't-erase discipline as the backlog archive and the assumptions ledger).
- **Recorded the moment it's seen**, by whoever saw it. `CAPTURE.md` routes a *past-tense observation*
  ("when I ran it, X happened") here, and a *future-tense* idea / change / bug to `BACKLOG.md ## New`. A
  try-it at a `show` task or at a phase close that provokes a reaction lands here — never inline in the
  build, never only in prose.
- **Terse, like the rest of `.gg/`.** Record the *reduced* result — the number, the verdict, the
  one-line reading — not a pasted transcript or raw logs.
- **A `[discovered]` "done and perfect" clause is closed by a finding.** A VISION clause tagged
  `[discovered]` (`VISION-FORMAT.md`) can only be reported *met* at a phase close by citing the `F-NN`
  that observed it — the observation-analogue of "an `AC-N` is closed only by real evidence, never
  'should work'".
- **A `reported` acceptance criterion is closed by a finding too.** A `research` phase's `reported`
  `AC-N` (`SPEC-FORMAT.md`) closes by citing the `F-NN` that recorded its measured result —
  `yes` / `no` / `inconclusive` all honest; a negative result is the finding, not a failure.
- **A finding may also revise the destination.** When an observation contradicts a "done and perfect"
  target, the `B-NN` it spawns becomes, when applied, an `R-NN` correction-from-evidence in `VISION.md`
  (`VISION-FORMAT.md`) — the finding is the cited trigger; the revision is never made without it.
- **Created lazily** at the first finding, with the `# FINDINGS — {name}` header. No secrets — record the
  *name* of an env var, never its value (`.gg/` is committed).
