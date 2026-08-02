# FORMATS.md — the `.gg/` files (shared)

Seven bounded files plus `adr/`. Each file is small enough to read whole (only `DESIGN.md` and `adr/`
are read by section/slug). **Bounds have teeth**: `WORK.md` stays under **16KB**; every other
whole-read file stays under **32KB**. A file past its bound is pruned back to its current-truth core
(METHOD.md → The record register), and **a prune buys headroom**: cut to about **three quarters of
the bound** (WORK ≤12KB, others ≤24KB), never to just under the line — a file trimmed to the edge
is back over it one session later. **Bounds are checked at exactly two points**: the batch close
(accretion this batch left) and the record pass (`/gg:where --audit`, applied on the user's
explicit yes — inherited overage). No other moment measures or trims: mid-batch, an over-bound
file is noted in passing at most, never cut. Deleted, never parked in an archive.
**Formats are closed**: exactly these sections and fields, never invented ones. **History is
git's**: applied, consumed, and superseded blocks are **deleted**, not archived — `git log -S
"B-12"` recovers anything. No secrets anywhere (`.gg/` is committed): env var *names*, never values.

## WORK.md — the hot file (state + the current batch)

Bounded hard: exactly one batch; the close resets it. Always readable whole (<16KB).

```md
# WORK — {Project name}

## State
- **state**: {shaping | building | idle}
- **batch**: {N}                     <!-- counts batches, 0 = the initial product -->
- **kind**: {build | question}       <!-- question = a research batch: closes on a measured answer -->
- **commit**: {ask | auto | never}   <!-- set once at /gg:new -->
- **deploy**: {ask | user-runs | auto} <!-- set once at /gg:new (METHOD.md → Safety) -->
- **gg**: {X.Y.Z}                      <!-- the plugin version whose formats this record follows -->

## Board
| # | item | size | done when | status |
|---|------|------|-----------|--------|
| 1 | B-12 {short name} | S | {one-line observable done-when} | done |
| 2 | B-14 show: {slice} | show | runs and is watchable via: {command} | pending |
| 3 | B-14 {short name} | M | {…} | pending |
<!-- batch 0's rows, born before any backlog, carry the short name alone — no B-NN -->
<!-- a show is never the final row — the close's Try walk is the batch's last look -->

## Try
{The batch's deliverable + load-bearing flows, walked at the close. Rewritten whole at batch open.}
- **Deliverable**: {what's runnable} — **see it**: {the real command/steps}
- {flow, one line, citing the B-NN it exercises}

## Provenance
- **Base commit**: {git HEAD at the batch's first build, or `unversioned`}
- **Suite baseline**: {N pass / M fail at the batch's first build, or `none (no suite yet)`}
- **Pre-existing dirty paths**: {the user's, off-limits — never claimed, reverted, or committed}
- **Owned paths**: {files this batch created/changed — grows as rows close}

## Where to resume
- **Next**: row {K} — {file:line / the concrete next step a stranger could act on}
- **Notes**: {gotchas; or "Blocked: {reason} / unblock when {observable condition}"}

## Fix log
{One line per /gg:fix since the last batch open; pruned at batch open.}
- {YYYY-MM-DD} — {what} — {test added?} — {suite result}

## Last closes
{ONE single line per batch close (≤200 chars), newest first, capped at 5 — never a paragraph;
older history is git's.}
- batch {N} ({YYYY-MM-DD}): {applied B-NNs} — {one-line outcome}
```

Rules: **state** ∈ `shaping` (mid `/gg:new` or `/gg:plan`, resumable — "Where to resume" holds the
open questions) / `building` (rows pending) / `idle` (no batch open). **gg** is the version stamp:
the plugin version whose `FORMATS.md` shapes this record follows — written once at `/gg:new`;
rewritten **only** by a conversion (`CONVERSION.md`, its closing step) or by the record pass after
it verifies the shapes conform, never by a working session merely running under a newer plugin. **Board rows** are short names
citing the `B-NN` they build (batch 0's rows carry the short name alone), ≤80 chars; status ∈
`pending / done` — the row in flight is the one "Where to resume" names; a `[bug]` row's done-when
carries the broken-vs-expected essence, so the build session needs no other source. **A done-when
is the observable check plus only the pins the build genuinely needs, as short clauses — ≤500
chars**; anything longer is design prose that belongs in `DESIGN.md` or an ADR, cited by id.
**size** ∈ `S`
(bug/tweak, no design contact — zero questions, zero design prose) / `M` (light design contact — 1-3
questions) / `L` (genuinely designed — full grilling, ADR if warranted) / `show` (a watchable slice
whose done-when is its "how to see it"; **mid-batch only, where the look can still change the
remaining rows** — riskiest first, as early as the foundation allows. **Never the final row**: when
the earliest judgeable moment is the batch's end, there is no show row — the close's Try walk is
that look. A show runs on the surface the user actually judges — usually the deployed product on
their own device, not a locally staged stand-in). A `question` batch's done-when is "answered:
recorded as `F-NN`".
A batch may group the record doc-sync its rows caused into **one S row for convenience** — that row
carries no `B-NN` of its own (it is not product) and never certifies: a ✓ belongs to the close's
walk (or a show's routed verdict), never to a record row.
**Provenance is written once** per batch, before the first code change; only Owned paths grows.
**Opening a batch rewrites Board/Try/Where-to-resume whole and resets Provenance to placeholders**;
the Fix log is pruned; **`## Last closes` always survives** (capped at 5 — under `commit: never` it
is the only history). The close marks rows done, updates Last closes (batch 0's line names the
deliverable instead of applied ids), sets `state: idle`.

## BACKLOG.md — future work only

```md
# Backlog — {Project name}
next-id: B-47

## New
{Captured, not yet planned. Newest first.}

### B-46 — [bug] {short title}        <!-- [bug] = defect in shipped behavior; [exp] = experiment to run -->
- **Captured**: {YYYY-MM-DD} — {user | agent}
- **Idea**: {in the user's words; for a [bug], broken vs expected}
- **Touches**: {area(s), if known}
- **Relates**: {refines: B-03 / contradicts: B-05 / reverses: A-NN — optional}
- **Decided**: {YYYY-MM-DD} — {the settled calls, one line}   <!-- optional: grilled and sealed -->

## Later
{Deliberately deferred at a /gg:plan. Same block shape.}

## Staged
{Optional: pre-grouped future batches (a programme), decided ahead with the user. One line per
batch, in order — the next /gg:plan takes the top group instead of re-deriving triage.}
- {batch name}: B-12 · B-14 · B-19 — {one-line intent}
```

Rules: **`next-id:` is the only counter** — every `B-NN` mint (capture or `/gg:plan`) bumps it,
wherever the item lands: **assign the current value to the item, then increment the header**. Ids
are zero-padded to two digits (`B-05`), grow naturally past 99 (`B-100`), and are stable, never
reused. **An agent-minted item's Idea opens with one plain sentence anyone can triage** — a concrete
example, no internal jargon: its owner reads it cold, days later. **A block describes behavior and
contracts, never file paths or line numbers** — those go stale while the item waits; file:line
belongs only in WORK's "Where to resume". An item marked **`Decided`** was
grilled and settled with the user — `/gg:plan` executes it and never re-opens its recorded calls,
even where they contradict an older record (only a genuinely new fork inside its scope may ask). When `/gg:plan` takes an item, its block **moves out** of BACKLOG onto the WORK board — the
row cites the `B-NN`, and the block's load-bearing detail (a `[bug]`'s broken-vs-expected) lands in
the row's done-when, never only in the chat report. Applied/discarded items are **deleted** — a
discard's reason is stated at the plan gate; if it's a "never", it becomes a PRODUCT boundary. **A
reference to a deleted id is not dangling**: an id exists if it is live in `.gg/` or recoverable via
`git log -S`; sweeps leave surviving blocks' references (`refines:`, `reverses:`, `Leads to:`)
intact.

## PRODUCT.md — the destination

```md
# Product — {Project name}

## Problem / opportunity
## For whom
## What it is — and what it is NOT
- **It is**: {essence — the whole product}
- **It is not**: {explicit boundaries — by product decision, never by effort}

## "Done and perfect"
{The bar, as clauses. Tag each; mark confirmations when a try-it observes them.}
- {clause} — `[declared]`
- {clause} — `[discovered]` ✓ {YYYY-MM-DD}: {one-line observation}

## Quality bar and non-negotiables
{Checkable; numbers where they exist.}

## Unknowns / risks
{An open/empirical product names its central open question here — answered by a `question` batch.}
```

Rules: the destination, not an MVP; product intent, not architecture. `[declared]` = judgeable
without running (closed by a done-when); `[discovered]` = only judgeable by watching it run (closed
by an observed ✓; when in doubt, `[declared]` — never use the tag to leave a checkable thing vague).
**A clause carries at most its latest ✓** (one line); a new observation replaces the old one — the
walk history is git's, not a ledger under the clause.
**Edited in place; a "done and perfect" clause is revised only by observed evidence** (name the
observation in the edit's session and commit message) — never because it was hard to build.
"It is not" is load-bearing: it's what prevents drift. An Unknown a `question` batch answered is
edited or removed at that close, citing the `F-NN`.

## DESIGN.md — the current-truth design

```md
# Design — {Project name}

## Shape
{The architecture in a few sentences: major components and how they connect.}
## Data model
{Entities, key fields, relationships — settled whole so batches extend instead of re-layering.
Enumerate the knowable; give a genuinely open property space an extension point (an open map /
registry), never as a dodge for a knowable schema.}
## Shared types & contracts
## Stack & platform
{Load-bearing choices; link the ADRs that record the why.}
```

Rules: **edited in place — always the current truth**; git diff is the design history, an ADR is the
"why" of a hard call. Read by section, not whole, when the task is narrow. Link, don't duplicate:
run/test commands live in RUNBOOK, acceptance on the WORK board, rationale in ADRs.

## NOTES.md — open assumptions + live findings

```md
# Notes — {Project name}
next-id: A-31 · F-12

## Assumptions
{Defaults taken instead of asking, still in play. Newest last.}

### A-30 — {short title}
- **Question not asked**: {the decision not put to the user}
- **Default taken**: {what was assumed} — **why**: {one line}
- **Reverse it**: {the note that overturns it}
- **Blast radius**: {low | medium} — {what changes if reversed}

## Findings
{Observations that matter to live work. Newest first.}

### F-11 — {short title}
- **Observed**: {YYYY-MM-DD} — {user | agent}, {the run/try-it that produced it}
- **What happened**: {the reduced result — the number, the verdict; never pasted logs}
- **Reading**: {one line — what it means}
- **Leads to**: {B-NN | —}
```

Rules: high-blast decisions are never assumptions — they're grilled. At batch close, **delete**
consumed assumptions (the test: would reversing it now be a change to shipped behavior? → consumed)
and findings whose work is done — git keeps them; ids never reused (`next-id:` only goes up).
A finding stays only while something live cites it — an open `Leads to` item, an unconfirmed
`[discovered]` clause, a time gate not yet reached; **one with no such consumer is consumed at the
second close it survives** (its durable rule moves to RUNBOOK/DESIGN, or it is git's). A
pending state ("verdict owed") is never a finding. A close with nothing observed records no finding —
the Try-walk verdict is acted on, not archived.

## CONTEXT.md — the glossary

```md
# {Context name}
{One or two sentences on what this context is.}

## Language
**{Term}**:
{One or two sentences. What it IS, not what it does.}
_Avoid_: {the synonyms not to use}
```

Rules: only domain terms specific to this project; be opinionated (pick the word, list the rest under
_Avoid_); updated the moment a term is resolved in grilling. **An entry is the definition plus its
_Avoid_ list — nothing else**: no implementation details, no history, no batch citations, no
how-it-used-to-work. An entry that outgrew this is pruned back at the next close (git keeps the rest).

## RUNBOOK.md — how to run and verify

```md
# Runbook — {Project name}

## Prerequisites & setup
## Environment            {env vars by NAME + purpose; never a value}
## Full suite             {THE single canonical command — the baseline and the close run exactly this}
## Focused tests
## Lint / static analysis
## Build
## Destructive paths & external effects
{Anything that writes data, deploys, sends, or is irreversible — flagged so no session runs it
blindly. Reset/seed scripts are first-class dev tools, but destroying a POPULATED store still stops
for a yes (METHOD.md → Safety).}
```

Rules: one canonical full-suite command; every entry copy-pasteable; update it the session the stack
changes ("n/a" for sections that don't apply yet). **An entry is the command, a one-line purpose,
and the trap warnings that keep it safe — war stories stay in git.** A one-time procedure that has
run to completion (a migration, a reset recipe) is deleted at that close.

## adr/ — decisions

`.gg/adr/NNNN-slug.md`, created lazily; the slug says the decision (`0007-sqlite-over-postgres.md`).
Body: the decision in **1-3 sentences up top** — context, decision, why. Below that, only
**measured evidence** that defends the call (the numbers, the sensitivity run, the live counts) —
never narrative history (optional Status / Options / Consequences only when they earn it). Offer an
ADR **only** when all three hold: hard to reverse · surprising without context ·
a real trade-off. Next number = highest in `ls .gg/adr/` + 1. **No amendment blocks**: an ADR whose
body went stale is rewritten to current truth in place (git keeps the old text) — and a body kept to
1-3 sentences has almost nothing that *can* go stale.

## Commit messages (when the commit policy says to commit)

- Session of build rows: `gg(b{N}): rows {2–4} — {short summary, citing B-NNs}`
- Batch close: `gg(b{N}): close — applied {B-NNs} · consumed {A-NNs}` (+ a suite line in the body;
  batch 0 names the deliverable instead of applied ids)
- Fix: `gg(fix): {what} — {root cause, one line}`
- Record pass: `gg(record): {what the pass reconciled}`
- Kickoff: `gg(b0): plan — {M} rows` · Plan: `gg(b{N}): plan — {B-NNs} · {M} rows`

Always pathspec-scoped (`git add .gg/ {owned paths}`); never push (METHOD.md → Commits).
