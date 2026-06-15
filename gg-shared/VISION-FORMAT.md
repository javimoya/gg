# VISION.md format

`.gg/VISION.md` is the project's north star, produced by `/gg:ideate`. It states the complete
destination — not an MVP. Stable; changes rarely (when the product's scope genuinely grows).

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
{What the complete, robust product looks like. The bar everything is measured against — and the bar
the refinement phases close toward.}

## Quality bar and non-negotiables
{Cross-cutting requirements no phase may lower. Numbers where they exist.}

## Stage
{`dev` or `launched` — mirrors the ROADMAP header (`STAGE.md`): whether the product has real users
whose data must survive. Default `dev`; flipped only by `/gg:orient`.}

## Unknowns / risks
{What we don't know yet and will have to discover.}
```

## Rules

- **Describe the destination, not an MVP.** "Done and perfect" is the bar the whole loop closes
  toward; make it concrete enough to measure against (the phase-close VISION-conformance check uses
  it).
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
- **Stable.** If something big changes here, the BLUEPRINT and the plan almost certainly need
  revisiting.
