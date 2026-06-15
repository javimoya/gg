# Stage: development vs. launched (shared)

A gg project has a **stage** — `dev` or `launched` — recorded in `ROADMAP.md`'s header. It answers
one question: **does the product have real users whose data must survive?** All data-defensiveness
follows from it. **Deployed ≠ launched**: a product can be in the cloud with zero real usage and
still be `dev`.

## Storage & who flips it

- Stored as `stage: dev | launched` in `.gg/ROADMAP.md`'s header. **Default `dev`** at kickoff
  (`/gg:ideate`).
- **Only `/gg:orient` flips it**, on the user's explicit go (it offers the toggle every time it
  runs). Flipping is a loud, recorded state change (a `JOURNAL.md` entry + a ROADMAP changelog line).
  There is no separate stage command.
- The **user** decides the flip — deploying does not. `launched → dev` is especially loud ("this
  tells gg your users' data is expendable").

## What changes between stages

| Concern | `dev` (no real users) | `launched` (real users) |
|---|---|---|
| Schema / shape change | recreate & reseed | migration that preserves data |
| Backward compatibility | none — change the shape freely | required; name what still speaks the old contract |
| Delete | hard-delete | soft-delete / preserve as the product needs |
| Preservation tests ("byte-for-byte unchanged") | skip — nothing to preserve | required where data must survive |
| Environments | one; verify locally | staged; verify in the target before claiming done |
| Reset / seed scripts | first-class dev tools | destructive — gated and flagged in the RUNBOOK |

In `dev`, defensive data engineering is **over-engineering** — don't do it, and record each omission
as an `A-NN` assumption (`ASSUMPTIONS-FORMAT.md`) so the launch flip can consume it. In `launched`,
the same work is real and built at full bar.

## Stage-independent (true in BOTH stages)

These protect the user's *own* work and never relax with the stage (`CONSTITUTION.md` → "Safety and
reversibility"):
- Never `git reset --hard` / `clean` / wholesale revert; own only what you changed; secrets out of
  `.gg/`.
- **Name the rollback and confirm before destroying data that exists on disk now** — recreating a
  *populated* dev store still stops for a yes. Stage turns off defending *hypothetical* users, never
  protecting *what actually exists* (including the developer's own authored test content).

## The launch flip (dev → launched)

Flipping to `launched` is not a one-line state change — it **consumes the caution dev deferred**.
`/gg:orient` seeds **launch-readiness notes** into `.gg/NOTES.md ## Pending`, folded into the next
phase by `/gg:discover`:
- establish a migration baseline from the current schema;
- snapshot the current API/data shape as the v1 contract;
- reclassify every dev reset/seed script as destructive and gate it (RUNBOOK "Destructive paths");
- quarantine wipe-on-setup tests.

This is **sequencing, not a cut**: the work always existed; dev deferred it honestly, and the
dev-stage assumptions are the receipt.
