# BLUEPRINT.md format

`.gg/BLUEPRINT.md` is the **whole-product design**, produced by `/gg:discover` in phase 0 and
extended by later phases. It is the artifact that kills the layering bug: because the data model and
architecture are decided **once, up front**, later phases extend a coherent design instead of
re-opening a structure an earlier phase froze (and then writing migrations to protect it). Each
phase's SPEC is a *slice* of this blueprint and never reduplicates it.

## Structure

```md
# BLUEPRINT — {Project name}

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
that record the "why".}

## Stage note
{How the stage affects the shape — e.g. "dev: a single local SQLite file, recreated when the schema
changes; launched: the same schema under migrations." The defensive parts are launched-only
(`STAGE.md`).}
```

## Rules

- **Decided once, extended coherently.** Phase 0 designs the whole thing; later phases *extend* it
  (add entities, fields, components) but **never silently re-open a frozen structure**. Reworking the
  shape is a real, recorded decision (an ADR and/or a note), not an incidental layer.
- **Designed whole is what removes migrations.** The reason to design the full data model up front is
  precisely so it isn't built in slices that each need a migration. In `dev`, a shape change is a
  recreate-and-reseed (`STAGE.md`); in `launched`, it's a real migration.
- **The blueprint is WHAT the product is built on; the SPEC slices it.** Per-phase acceptance
  criteria, the deliverable, and "how to see it" live in `SPEC.md` and *reference* the blueprint —
  they don't copy the schema. One source of truth for the design.
- **High-blast decisions here are grilled, not defaulted** (`CONSTITUTION.md` → "Defaults and
  assumptions"). The data model and stack are load-bearing — settle them with the user, and record
  the genuinely architectural ones as ADRs (`ADR-FORMAT.md`).
- **No secrets.** Record the *name* of an env var or credential the design needs, never its value
  (`.gg/` is committed).
