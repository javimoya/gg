# CONVERSION.md — convert a v2 `.gg/` to v3, by hand or by an AI session

gg v3 ships **no migration logic in its commands** (see `CLAUDE.md`): an existing v2 `.gg/` is
converted once, deliberately, in a dedicated session. This document is written so an AI session can
execute it directly: open Claude Code **in the project to convert** (not in the gg repo) and say
*"read CONVERSION.md from the gg plugin repo and convert this project's `.gg/` to v3"*.

The v3 formats you are writing are defined in `gg-shared/FORMATS.md`; read it first, then this.
Write all converted content in English (verbatim user quotes stay in their language).

## 0. Safety first — git is the archive this conversion relies on

1. The project must be a git repo with `.gg/` tracked. If `.gg/` has uncommitted changes, **commit
   them now** (e.g. `chore: .gg snapshot before v3 conversion`) — the v2 files you are about to
   delete survive only in git history.
2. Do not start if a gg session is mid-task elsewhere. The working tree outside `.gg/` is the
   user's — untouched, uncommitted.

## 1. Read the v2 state (all of it, once)

Read: `ROADMAP.md` (header + phase log), `PROGRESS.md`, `VISION.md`, `BLUEPRINT.md` (headings first;
then the phase-0 sections and every `## Phase N` delta), `SPEC.md` (the current phase's `AC-N`s, the
Deliverable + Try list, `## Shows`), `ASSUMPTIONS.md` (`## Open` only), `FINDINGS.md` (headings; then
the blocks cited by live work), `BACKLOG.md` (all sections), `CONTEXT.md`, `RUNBOOK.md`, `adr/`
(list). The archives and `JOURNAL.md` are needed **only** for the id scan in §2 — never read them
whole.

## 2. Seed the id counters (the one thing the archives are needed for)

- `BACKLOG.md next-id:` = 1 + the highest `B-NN` found in **both** `BACKLOG.md` and
  `BACKLOG-ARCHIVE.md` (grep the `### B-` anchors; don't read the files).
- `NOTES.md next-id:` = 1 + highest `A-NN` across `ASSUMPTIONS.md` + `ASSUMPTIONS-ARCHIVE.md`, and
  1 + highest `F-NN` in `FINDINGS.md`.

Ids are never renumbered: every surviving block keeps its id.

## 3. Write the v3 files (`gg-shared/FORMATS.md` shapes, exactly)

**`WORK.md`** — from ROADMAP + PROGRESS + the current-phase slice of SPEC:
- Header: `state:` = v2 `building` → `building`; `shipped` → `idle`; `scoping`/`visioning` →
  `shaping`. `batch:` = v2 `phase`. `kind:` = v2 `research` → `question`, else `build`.
  `commit:` — **ask the user now**: `ask` (recommended) / `auto` / `never`.
- **The batch sections — Board, `## Try`, `## Provenance`, `## Where to resume` — are filled only if
  a phase is in flight** (`state: building`). For a shipped/idle project leave them empty per
  `FORMATS.md` (Where to resume: one line, *"idle — next: /gg:plan"*) — the next `/gg:plan` rewrites
  them whole anyway.
- Board (in-flight only): one row per PROGRESS task, statuses preserved (`in-progress` → `pending`,
  with "Where to resume" naming it). Give each row a **size** (S = bug/tweak, M = light design
  contact, L = designed feature; judge from the task) and a one-line **done when** derived from the
  task + the `AC-N`s it maps to (collapse the criterion to its observable essence; drop the AC id
  machinery). A v2 `show` task keeps `size: show`, its done-when = its "How to see it" from SPEC
  `## Shows`.
- `## Try` (in-flight only): from SPEC's Deliverable + "How to see it" + Try list (cite `B-NN`s,
  drop `AC-N` refs).
- `## Provenance` / `## Where to resume` (in-flight only): **content-faithful,
  reference-translated** — keep the base commit, dirty/owned paths, and the concrete next step
  exactly, but add a `Suite baseline:` line (from the PROGRESS baseline if recorded, else `none`),
  rewrite v2 command names to their v3 equivalents (`/gg:next-task`→`/gg:go`, `/gg:orient`→
  `/gg:where`, `/gg:capture`/`/gg:refine-backlog`/`/gg:discover`→`/gg:plan`, `/gg:quick`→`/gg:fix`),
  strip stage/STAGE.md language, drop mentions of A-NNs this conversion deletes, and condense —
  WORK.md must stay under 10KB.
- `## Fix log`: empty. `## Last closes`: the last ≤5 shipped phases from the ROADMAP phase log, one
  line each (`batch {N} ({date}): {the B-NNs / one-line summary}`).

**`BACKLOG.md`** — header + `next-id:`; `## New` = v2 `## New`; `## Later` = v2 `## Later` +
`## Future` merged (keep blocks as they are, but **drop any v2 `[jot]` tag** — it was capture
provenance, not a category; `[bug]` stays; anything experimental becomes `[exp]`). v2 `## Next
phase` items: if a phase is in flight they are the board — their `B-NN`s live on WORK rows; delete
the blocks. If none is in flight, they are the user's chosen next set — put them at the top of
`## New` with a note (`queued pre-v3`) so the next `/gg:plan` picks them up first.

**`PRODUCT.md`** — from VISION: Problem / For whom / It is–is not / "Done and perfect" clauses with
their `[declared]`/`[discovered]` tags / Quality bar / Unknowns. **`## Constraints and accepted
tradeoffs` has no v3 section**: fold each entry into "It is not" (a boundary) or "Quality bar" (an
accepted tradeoff) — keep the content, not the section. Drop `## Stage` (the concept is gone
in v3) and `## Revisions` (applied `R-NN`s are already folded into the clauses; git keeps the trail).
Add a `✓ {date}: {one-line observation}` mark to each `[discovered]` clause that a v2 phase close
confirmed with a cited `F-NN` (the VISION or the finding itself names it); leave unconfirmed clauses
unmarked. Fix any clause that still names a removed surface (check against the current product).

**`DESIGN.md`** — from BLUEPRINT, **current truth only**: Shape / Data model / Shared types &
contracts / Stack & platform, with every `## Phase N` delta folded in (the latest supersession wins)
and all per-phase history, revision sections, and the Stage note dropped. This is a rewrite, not a
copy: the result must describe the product as it is today, in one pass, with no archaeology.

**`NOTES.md`** — `## Assumptions`: the genuinely-open `A-NN` blocks from v2 `## Open` (drop the
`_(…swept at close)_` pointer stubs and the `Phase` field; keep id, question-not-asked, default, why,
reverse-it, blast radius). Drop stage-deferral assumptions ("no migrations while dev" etc.) — v3 has
no stage; that behavior is now the default and needs no record. `## Findings`: only the `F-NN` blocks
cited by live work — an open backlog item's `Relates`, the in-flight board, or an unconfirmed
`[discovered]` clause. Everything else is deleted (git keeps it).

**`CONTEXT.md`** — keep. Optionally slim entries that grew into mini-essays of phase history back to
one-or-two-sentence definitions; delete entries for removed concepts (or keep the one-line "removed"
gloss if the term still appears in code).

**`RUNBOOK.md`** — keep; delete stage-conditional language (reset/seed scripts are simply first-class
tools; destroying a *populated* store still asks — that rule is in `METHOD.md` now); fix any command
or reference the product no longer has.

**`adr/`** — keep as-is.

## 4. Delete the v2 files

```
git rm .gg/ROADMAP.md .gg/PROGRESS.md .gg/SPEC.md .gg/BLUEPRINT.md .gg/VISION.md \
       .gg/ASSUMPTIONS.md .gg/FINDINGS.md .gg/JOURNAL.md .gg/PRINCIPLES.md \
       .gg/ASSUMPTIONS-ARCHIVE.md .gg/BACKLOG-ARCHIVE.md
```

(BACKLOG.md and CONTEXT.md and RUNBOOK.md were rewritten in place; VISION→PRODUCT and
BLUEPRINT→DESIGN are renames-with-rewrite, so the old names are removed here.)

## 5. Verify, then commit

1. Check the result against `gg-shared/FORMATS.md`: exactly `WORK.md`, `BACKLOG.md`, `PRODUCT.md`,
   `DESIGN.md`, `NOTES.md`, `CONTEXT.md`, `RUNBOOK.md`, `adr/` — no other file, no invented sections.
2. Run `/gg:where --audit` and fix what it reports (dangling ids are usually a missed `Relates`).
3. Sanity-read `WORK.md` as if you were a fresh session: could you resume from "Where to resume"
   alone? If not, sharpen it.
4. Commit: `chore: convert .gg to gg v3` (this commit is also the v3 record's first journal entry).
5. Breadcrumb to the user: the state, the batch, and the next command (`/gg:go` if a batch is in
   flight; `/gg:plan` otherwise).

## Notes for the two known v2 projects

These projects keep moving — **read each ROADMAP header for the real state; the §3 mapping is
authoritative**, never these notes. Durable facts only:

- **klasse** (`~/code/klasse`): large v2 files (SPEC ~230KB with 200+ ACs, BLUEPRINT ~208KB with
  ~49 `## Phase N` sections). For DESIGN, **skim every phase section — including ones labeled "No
  design change"**, whose re-scope/close paragraphs still carry component contracts; fold whatever
  still describes shipped behavior, and skip only what a later phase superseded (e.g. phase 40
  removed the phase-6 guided lesson — CONTEXT.md flags removals). Its ASSUMPTIONS `## Open` mixes
  truly-open blocks with `_(swept at close)_` pointer stubs — only the former survive.
- **flights** (`~/code/flights`): watch A-55 (the launch-flip assumption) — it drops with the stage
  concept; the backup/recovery material in RUNBOOK stays. Some backlog blocks carry the v2 `[jot]`
  tag — drop it (§3).
