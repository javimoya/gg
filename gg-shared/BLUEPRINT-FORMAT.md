# BLUEPRINT.md format

`.gg/BLUEPRINT.md` is the **whole-product design**, designed by `/gg:discover` in phase 0. It is the
artifact that kills the layering bug: because the data model and architecture are decided **once, up
front**, later phases build on a coherent design instead of re-opening a structure an earlier phase
froze (and then writing migrations to protect it). Each phase's SPEC is a *slice* of this blueprint and
never reduplicates it.

After phase 0 the blueprint is **append-only**: the phase-0 design is **frozen** — never edited — and
**every refinement phase appends a dated `## Phase N` section**, newest last, so the design ledger has
no gaps. The **depth scales with the phase**: a phase that changed no design (a heterogeneous set of
bugs and small tweaks) gets a **one-liner** — an explicit "no design change", so a reader knows the
design held through that phase instead of wondering whether a change went unrecorded; a phase that added
entities / fields / components, or superseded an earlier decision, gets the **full detail**. The current
design is the phase-0 content plus the appended sections read in order (the latest supersession wins).

## Structure

```md
# BLUEPRINT — {Project name}

<!-- Phase 0: the whole-product design. Frozen once phase 0 closes — never edited; later phases append below. -->

## Shape
{The architecture in a few sentences: the major components and how they connect.}

## Data model
{The whole data model / schema, designed up front so later phases extend it rather than re-layering
it. Entities/tables/collections, their key fields, and relationships. This is the part that, designed
whole, removes the need for incremental migrations.}

## Shared types & contracts
{Cross-cutting types, API shapes, and naming used across the product, so phases agree on the seams.}

## Stack & platform
{The load-bearing technology choices — language, framework, datastore, deploy target. Link the ADRs
that record the "why". Don't restate facts owned elsewhere (run/test commands, the test framework) —
those live in `RUNBOOK.md`; link them.}

## Stage note
{How the stage affects the shape — e.g. "dev: a single local SQLite file, recreated when the schema
changes; launched: the same schema under migrations." The defensive parts are launched-only
(`STAGE.md`).}

<!-- Every refinement phase appends a dated section below, newest last. Depth scales with significance. Never edit the content above. -->

## Phase {N} — {short summary} ({YYYY-MM-DD})
{Scaled to the phase. **No design change** → one line, e.g. "Bug fixes (diff viewer, fuzzy search) and
UI tweaks — no design change." **Design changed** → the new entities / fields / components (with why),
or a **supersession** of an earlier decision (name what it revises, and an ADR if it meets the bar).
Never edit the content above; the delta lives here.}
```

## Rules

- **Append-only after phase 0.** Phase 0 designs the whole thing and is then **frozen** — never edited.
  Each later phase **appends a dated `## Phase N` section** (newest last); it **never edits** earlier
  content and **never silently re-opens** a frozen structure. A change to an earlier decision is recorded
  in the new section as a **supersession** (point at what it revises) — the same move-don't-delete
  discipline as the assumptions ledger and the ROADMAP changelog; the genuinely architectural ones are
  also ADRs (`ADR-FORMAT.md`).
- **Every phase is recorded; the depth scales.** A refinement phase is a heterogeneous set — bugs,
  tweaks, new features — and **each one appends a section**, so the design ledger has no gaps. A phase
  that changed no design gets a **one-line** entry (naming what it did + "no design change" — an explicit
  record, never a silent omission, so the reader never wonders if a change was forgotten); a phase that
  added or superseded design gets the detail it deserves.
- **Designed whole is what removes migrations.** The reason to design the full data model up front is
  precisely so it isn't built in slices that each need a migration. In `dev`, a shape change is a
  recreate-and-reseed (`STAGE.md`); in `launched`, it's a real migration.
- **Link, don't duplicate.** The blueprint is the single source of truth for the **design** (data
  model, architecture). Don't restate facts that live authoritatively elsewhere — the test framework
  and run/verify commands are in `RUNBOOK.md`, per-phase acceptance is in `SPEC.md`, the "why" of a big
  call is in an ADR. **Link them instead.** A duplicated fact drifts (the blueprint says XCTest while
  the RUNBOOK moved to another framework); a single source can't.
- **The blueprint is WHAT the product is built on; the SPEC slices it.** Per-phase acceptance criteria,
  the deliverable, and "how to see it" live in `SPEC.md` and *reference* the blueprint — they don't copy
  the schema. One source of truth for the design.
- **High-blast decisions here are grilled, not defaulted** (`CONSTITUTION.md` → "Defaults and
  assumptions"). The data model and stack are load-bearing — settle them with the user, and record the
  genuinely architectural ones as ADRs (`ADR-FORMAT.md`).
- **No secrets.** Record the *name* of an env var or credential the design needs, never its value
  (`.gg/` is committed).
