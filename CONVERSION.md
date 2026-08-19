# CONVERSION.md — one-time `.gg/` conversions

gg ships **no migration logic in its commands** (see `CLAUDE.md`): an existing `.gg/` is converted
once, deliberately, in a dedicated session. This document holds every known conversion and is
written so an AI session can execute one directly: open Claude Code **in the project to convert**
(not in the gg repo) and say *"read CONVERSION.md from the gg plugin repo and convert this
project's `.gg/`"* — or take `/gg:where --audit`'s offer, which runs the same procedure on an
explicit yes.

## The index — which conversion applies

The stamp is the WORK header's `gg:` line (`FORMATS.md`): the plugin version whose formats the
record follows. Only **format-changing releases** have rows here — a stamp merely older than the
plugin with no row crossed needs no conversion (the record pass refreshes the line). Every release
that changes `gg-shared/` formats or the record's shape adds its row here and its procedure below
(`CLAUDE.md` → Releasing); `/gg:where --audit` reads this index to detect and offer the right
conversion. **Every conversion's closing step writes the new stamp** — an unstamped or old-stamped
record after a conversion is a conversion that didn't finish.

| The `.gg/` you have | What applies |
|---|---|
| v2 files (`ROADMAP.md` / `SPEC.md` / `BLUEPRINT.md` / `VISION.md` …) | **v2 → v3** below — its output is the *current* `FORMATS.md` shapes, so it never needs the prune |
| The seven files (`WORK` `BACKLOG` `PRODUCT` `DESIGN` `NOTES` `CONTEXT` `RUNBOOK` + `adr/`), no `gg:` stamp | Written under 3.0–3.2. Judge the shapes directly: 3.0 marks (ADR amendment blocks, ✓ ledgers under PRODUCT clauses, paragraph-sized Last closes) → **3.0 → 3.1 prune** below (its output lands on current shapes, `exec:` included); already conforming to the 3.x shapes → only **3.x → 4.0.0** below |
| `gg:` stamp from 3.3.0 through 3.9.4 | **3.x → 4.0.0** below — one header-line insert |
| `gg:` stamp at 4.0.0 or above | Nothing — no format-changing release has shipped since the stamp was born |

The target shapes are defined in `gg-shared/FORMATS.md`; read it first, then this. Write all
converted content in English (verbatim user quotes stay in their language).

## v2 → v3 — the full conversion

### 0. Safety first — git is the archive this conversion relies on

1. The project must be a git repo with `.gg/` tracked. If `.gg/` has uncommitted changes, **commit
   them now** (e.g. `chore: .gg snapshot before v3 conversion`) — the v2 files you are about to
   delete survive only in git history.
2. Do not start if a gg session is mid-task elsewhere. The working tree outside `.gg/` is the
   user's — untouched, uncommitted.

### 1. Read the v2 state (all of it, once)

Read: `ROADMAP.md` (header + phase log), `PROGRESS.md`, `VISION.md`, `BLUEPRINT.md` (headings first;
then the phase-0 sections and every `## Phase N` delta), `SPEC.md` (the current phase's `AC-N`s, the
Deliverable + Try list, `## Shows`), `ASSUMPTIONS.md` (`## Open` only), `FINDINGS.md` (headings; then
the blocks cited by live work), `BACKLOG.md` (all sections), `CONTEXT.md`, `RUNBOOK.md`, `adr/`
(list). The archives and `JOURNAL.md` are needed **only** for the id scan in §2 — never read them
whole.

### 2. Seed the id counters (the one thing the archives are needed for)

- `BACKLOG.md next-id:` = 1 + the highest `B-NN` found in **both** `BACKLOG.md` and
  `BACKLOG-ARCHIVE.md` (grep the `### B-` anchors; don't read the files).
- `NOTES.md next-id:` = 1 + highest `A-NN` across `ASSUMPTIONS.md` + `ASSUMPTIONS-ARCHIVE.md`, and
  1 + highest `F-NN` in `FINDINGS.md`.

Ids are never renumbered: every surviving block keeps its id.

### 3. Write the v3 files (`gg-shared/FORMATS.md` shapes, exactly)

**`WORK.md`** — from ROADMAP + PROGRESS + the current-phase slice of SPEC:
- Header: `state:` = v2 `building` → `building`; `shipped` → `idle`; `scoping`/`visioning` →
  `shaping`. `batch:` = v2 `phase`. `kind:` = v2 `research` → `question`, else `build`.
  `exec:` = `solo` (every v2 batch was sequential by definition; `delegated` is only ever a
  `/gg:plan` decision, never a conversion's). `commit:` — **ask the user now**: `ask` (recommended) / `auto` / `never`. `deploy:` — **ask the
  user now**: `ask` (recommended) / `user-runs` / `auto`. `gg:` = the plugin's current version
  (`.claude-plugin/plugin.json` in the plugin repo) — the formats this conversion lands on.
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
  WORK.md must stay under its `FORMATS.md` bound (16KB).
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

### 4. Delete the v2 files

```
git rm .gg/ROADMAP.md .gg/PROGRESS.md .gg/SPEC.md .gg/BLUEPRINT.md .gg/VISION.md \
       .gg/ASSUMPTIONS.md .gg/FINDINGS.md .gg/JOURNAL.md .gg/PRINCIPLES.md \
       .gg/ASSUMPTIONS-ARCHIVE.md .gg/BACKLOG-ARCHIVE.md
```

(BACKLOG.md and CONTEXT.md and RUNBOOK.md were rewritten in place; VISION→PRODUCT and
BLUEPRINT→DESIGN are renames-with-rewrite, so the old names are removed here.)

### 5. Verify, then commit

1. Check the result against `gg-shared/FORMATS.md`: exactly `WORK.md`, `BACKLOG.md`, `PRODUCT.md`,
   `DESIGN.md`, `NOTES.md`, `CONTEXT.md`, `RUNBOOK.md`, `adr/` — no other file, no invented
   sections — and the WORK header carries the `gg:` stamp written in §3.
2. Run `/gg:where --audit` and fix what it reports (dangling ids are usually a missed `Relates`).
3. Sanity-read `WORK.md` as if you were a fresh session: could you resume from "Where to resume"
   alone? If not, sharpen it.
4. Commit: `chore: convert .gg to gg v3` (this commit is also the v3 record's first journal entry).
5. Breadcrumb to the user: the state, the batch, and the next command (`/gg:go` if a batch is in
   flight; `/gg:plan` otherwise).

## 3.0 → 3.1 — the record diet (a one-time prune)

3.1 tightened the record, measured from 3.0 in production: a `.gg/` had grown to ~3.9x its
conversion size in 41 batches, and a `/gg:go` orient to ~65k tokens with only ~⅓ of it load-bearing.
What the prune targets (`FORMATS.md` bounds + METHOD.md → The record register — always the current
numbers: **WORK <16KB, every other whole-read file <32KB**); present-tense current truth with **no
narrative history**; CONTEXT entries
= definition + _Avoid_ only; RUNBOOK = commands + traps; ADR bodies with no amendment blocks; at
most one ✓ per PRODUCT clause; single-line Last closes capped at 5; done-whens ≤500 chars — and a
new `deploy:` line in the WORK header. A `.gg/` written under 3.0 is pruned to those shapes once,
here.

Ground rules for every step: **delete, never archive** — git keeps everything; ids are **never
renumbered**; every current-truth fact survives — only prose, duplication, and history go; English;
env var names, never values.

### 0. Safety first

1. The project must be a git repo with `.gg/` tracked. Commit a snapshot **now**:
   `chore: .gg snapshot before 3.1 prune` — everything this prune deletes survives only in git, and
   the snapshot is the "before" for the size report in §2.
2. Prune only with **no batch in flight**: `WORK.md` must say `state: idle`. If it doesn't, **stop
   and say so** — finish or close the batch first (`/gg:go`), then come back.
3. Never touch the working tree outside `.gg/`.
4. **Anchored, block-scoped edits only — never regex/sed bulk edits** (METHOD.md): re-read the
   exact block from disk immediately before editing it.
5. **Check whether the product's suite reads `.gg/`** (grep the test code for `.gg/`). A test that
   quote-pins record sentences or reads a record file by path couples the suite to this diet — the
   prune will turn it red (klasse's did, 2026-07-27: three glossary words deleted → red suite).
   Expect it, and route the fix as decoupling the test, never as keeping the prose.

### 1. Prune each file

**`WORK.md`** (<16KB):
- Header gains `deploy: {ask | user-runs | auto}` — **ask the user once, now** (`ask` recommended);
  if they pre-decided it when commissioning this prune, write that without re-asking. It also
  gains `exec: solo` (the board is idle by precondition — `delegated` is only ever a `/gg:plan`
  decision) and `gg: {the plugin's current version}` — the stamp of the formats this prune lands on.
- `## Last closes`: ONE line per close (≤200 chars), newest first, capped at 5. Collapse paragraph
  entries to their line; delete everything past the cap.
- `## Fix log`: one-line entries since the last batch open only — older lines and entries grown
  past a line are cut back.
- The batch sections (Board, `## Try`, `## Provenance`, `## Where to resume`) carry no load at
  `idle`: the closed board's rows go (Last closes + git hold the close), Where to resume becomes
  one line — *"idle — next: /gg:plan"* — and the next `/gg:plan` rewrites them all whole anyway.

**`CONTEXT.md`** (<32KB): every entry reduced to its **definition + _Avoid_ list** — delete
history, implementation detail, batch citations, and how-it-used-to-work. Merge duplicate headwords
into one entry. **Keep every live term, and keep its full _Avoid_ list**: the _Avoid_ lists encode
regression traps — the wrong word a fresh session would otherwise reach for — and the prune never
trims them.

**`RUNBOOK.md`** (<32KB): each entry = the command + a one-line purpose + the trap warnings that
keep it safe. Delete war stories and **completed one-time procedures** — finished migrations,
retired-world sections, one-shot reset recipes that already ran. The destructive-paths section
stays whole.

**`PRODUCT.md`** (<32KB): each "done and perfect" clause keeps **at most its latest ✓** (one line);
delete the ✓-walk ledgers under clauses. Rewrite prose to current truth — a clause that narrates
its own revisions becomes the current clause alone.

**`NOTES.md`** (<32KB): backfill the sweep 3.0 closes didn't run. For **every** assumption, apply
the test: *would reversing it now be a change to shipped behavior?* → consumed, **delete**. For
every finding: delete unless live work cites it — an open item's `Relates`, an unconfirmed
`[discovered]` clause. What survives is genuinely open. `next-id:` untouched (it only goes up).

**`BACKLOG.md`** (<32KB): items keep their ids and blocks. Delete any archive-like section that
accreted (`## Done`, `## Applied`, …) — git keeps it. Give each **agent-minted** item's Idea a
plain opening sentence with a concrete example if it lacks one (its owner reads it cold, days
later). The optional `Decided:` field and `## Staged` section now exist (`FORMATS.md`) — available
going forward, never back-filled.

**`DESIGN.md`** (no hard bound — it is read by section — but the same register): current truth
only. Prune narrative history ("originally…", "batch N changed…") and fix any staleness found while
reading — a section describing what the product no longer has is rewritten or deleted.

**`adr/`**: an ADR carrying an amendment block is **rewritten to current truth in place** — one
body, no trail (git keeps the old text); same for any body that reads stale. Wholesale shortening
of *accurate* old ADRs is optional — they are read on demand; the bounds' teeth are on the seven
whole-read files. New ADRs follow the 1-3 sentence rule going forward.

### 2. Verify

1. Run `/gg:where --audit` (or walk its checklist by hand) and fix what it reports: bounds and
   register, dangling ids (a reference to a deleted id is **not** dangling if `git log -S` finds
   it), a `next-id:` at or below an id in use, invented structure or archive-like sections.
2. Report per-file **before → after** sizes to the user (the §0 snapshot holds the before; e.g.
   `CONTEXT.md 41KB → 14KB`).
3. Sanity-read `WORK.md` as a fresh session would: state, last closes, and the next command legible
   at a glance, nothing load-bearing lost.

### 3. Commit, then breadcrumb

1. `git add .gg/` (pathspec-scoped — nothing outside `.gg/`) and commit:
   `chore: prune .gg to gg 3.1 (record diet)`.
2. One-line breadcrumb to the user: the sizes cut + the next command (`/gg:plan` — the state was
   `idle` by precondition; `/gg:fix` for one small thing).

## 3.x → 4.0.0 — the execution field

4.0.0 added **delegated execution**: `/gg:plan` may design a batch for row agents in parallel
worktrees, and `/gg:go` orchestrates them (`gg-shared/DELEGATION.md`). The record's only new shape
is the WORK header's `exec:` field. The `after`/`by` board columns and the Delegations line exist
only on boards a 4.0.0 plan writes as `delegated` — an older record never needs them back-filled.

1. **Safety first**: if `.gg/` has uncommitted changes, commit a snapshot now
   (`chore: .gg snapshot before 4.0.0 conversion`).
2. In `WORK.md ## State`, insert `- **exec**: solo` between `kind:` and `commit:`. Every
   pre-4.0.0 batch was sequential by definition; `delegated` is only ever a `/gg:plan` decision,
   never a conversion's.
3. Rewrite the header's `gg:` stamp to the plugin's current version.
4. Commit (pathspec `.gg/` only): `chore: convert .gg to gg 4.0.0 (exec field)`.

## Notes for the two known projects

These projects keep moving — read each project's actual `.gg/` for the real state; the procedures
above are authoritative, never these notes. Durable facts only:

- **klasse** (`~/code/klasse`): converted to v3 on **2026-07-10**, stamped **4.0.2** on
  2026-08-19 — no conversion pending. Its 3.0 record is where the 3.1 evidence came from (~3.9x
  growth in 41 batches); its batch 108 was the first delegated batch in production.
- **flights** (`~/code/flights`): **full v3** (an earlier note here claiming v2 was stale),
  stamped **4.0.2** on 2026-08-19 — no conversion pending. What it does carry is inherited
  overage for the record pass (`/gg:where --audit`), measured that day: RUNBOOK 90KB, BACKLOG
  65KB, CONTEXT 36KB, NOTES 35KB — each past the 32KB bound — and DESIGN at 188KB (no hard bound,
  but the same register applies).
- **jmux** and **jgit**: full v3, stamped **4.0.2** on 2026-08-19 — nothing pending.
