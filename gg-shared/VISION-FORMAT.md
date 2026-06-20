# VISION.md format

`.gg/VISION.md` is the project's north star, produced by `/gg:ideate`. It states the complete
destination — not an MVP. Stable; it changes rarely — when scope genuinely grows, or when evidence
corrects the target (a recorded `R-NN`, below).

## Structure

```md
# Vision — {Project name}

## Problem / opportunity
{The real problem we solve or the opportunity we chase. Concrete, not generic.}

## For whom
{Who uses it. If there are several user types, name them.}

## What it is — and what it is NOT
- **It is**: {essence in 2-3 sentences — the whole product phase 0 builds}
- **It is not**: {explicit out-of-scope boundaries — by product decision, not by effort}

## Constraints and accepted tradeoffs
{The real limits the product is optimized within — budget, timeline, operational, compatibility,
regulatory, risk — and the tradeoffs deliberately accepted. Each is a *product decision*, stated out
loud so it isn't mistaken for a silent cut. Empty is fine; never use it to smuggle in scope cuts.}

## "Done and perfect" (global definition of done)
{What the complete, robust product looks like — the bar everything is measured against, and the bar the
refinement phases close toward. Write it as clauses, and tag each by how it can be judged:}
- {a clause} — `[declared]` {judgeable without running the product — a capability that's present or
  absent, closed by an `AC-N`}
- {a clause} — `[discovered]` {only judgeable by *seeing it run* — a felt / emergent / qualitative
  property the author confirms by watching, closed by a cited `F-NN`}

## Quality bar and non-negotiables
{Cross-cutting requirements no phase may lower. Numbers where they exist.}

## Stage
{`dev` or `launched` — mirrors the ROADMAP header (`STAGE.md`): whether the product has real users
whose data must survive. Default `dev`; flipped only by `/gg:orient`.}

## Unknowns / risks
{What we don't know yet and will have to discover. An open/empirical project names its **central open
question** here — the one the build exists to answer — usually mirrored by a `[discovered]` "done and
perfect" clause and pursued in a `research` phase (`ROADMAP-FORMAT.md`).}

## Revisions
{Corrections to the destination forced by evidence — **empty until** a finding contradicts a clause
above. One `R-NN` block each, newest last; append-only. A revision **edits the clause in place above**
and records the change here, citing the `F-NN` that triggered it. This is the *only* sanctioned way the
destination changes (`CONSTITUTION.md` → "Boundaries vs. cuts" → correction-from-evidence) — never a
silent edit.}

### R-01 — {short title}
- **Revised**: {YYYY-MM-DD} — {the clause / section changed}
- **Was**: {the old wording}
- **Now**: {the new wording — set by the evidence, not by what was hard to build}
- **Trigger**: `F-NN` — {the observed result that proved the old target wrong}
```

## Rules

- **Describe the destination, not an MVP.** "Done and perfect" is the bar the whole loop closes
  toward; make it concrete enough to measure against (the phase-close VISION-conformance check uses
  it).
- **Tag each "done and perfect" clause `[declared]` or `[discovered]`.** `[declared]` is judgeable
  without running — a present/absent capability, closed by an `AC-N`. `[discovered]` is only judgeable
  by *watching the product run* — a felt / emergent / qualitative property (the look, the feel, the tone,
  the sense that it's right) — closed at a phase close by a cited `F-NN` observation, never asserted from a green
  suite. The tag is a grilling question (`GRILLING.md`); when in doubt, a clause that *could* become a
  runnable check is `[declared]` — reserve `[discovered]` for what genuinely can't be reduced to one,
  so the tag never excuses leaving a checkable thing vague.
- **Non-negotiables must be checkable.** A quality bar nobody can verify is decoration. Put numbers
  where they exist.
- **Product intent, not architecture.** No implementation detail here; the design lives in
  `BLUEPRINT.md`, technical decisions in ADRs, the ubiquitous language in `CONTEXT.md`.
- **"It is not" is load-bearing.** Explicit product-scope boundaries (decided by product, never by
  effort) prevent drift.
- **Constraints are boundaries, not cuts.** A tradeoff listed here is an approved product decision.
  Deferred-but-in-scope work is *not* a constraint — it's a note for a future phase.
- **Dev-stage simplifications are assumptions, not boundaries.** "No migrations / no backward-compat"
  while `dev` is a reversible default recorded in `ASSUMPTIONS.md` and consumed at launch — it does
  **not** belong in "It is not" (that's for permanent product exclusions).
- **The destination is corrected only by a recorded `R-NN` — never silently.** VISION is stable, but a
  phase can prove a "done and perfect" clause *wrong about reality*. When a finding (`F-NN`,
  `FINDINGS-FORMAT.md`) contradicts a clause, it spawns a `B-NN` (the single intake — `CONSTITUTION.md` →
  "Findings"); applying that item **edits the clause in place** and logs an append-only `R-NN` in
  `## Revisions` that **cites the triggering `F-NN`**. The decisive guard (`CONSTITUTION.md` → "Boundaries
  vs. cuts" → **correction-from-evidence**): a revision is legitimate only when *evidence proved the
  target wrong*, never when *the target was hard to build* — the latter is a forbidden cut. A revision
  with no citable `F-NN`, or whose `F-NN` doesn't contradict the clause, is a cut in disguise
  (`/gg:orient --audit` flags it). `R-NN` ids are stable and never reused.
- **Stable.** If something big changes here, the plan and the BLUEPRINT almost certainly need
  revisiting — recorded as a new appended `## Phase N` section in the BLUEPRINT, never an edit to its
  frozen phase-0 design (`BLUEPRINT-FORMAT.md`).
