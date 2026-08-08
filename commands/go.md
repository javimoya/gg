---
description: "Builds the open batch. Orients from WORK.md in seconds, then implements the next board row to the full bar (with tests) — one row per run. New scope the user wants NOW folds into the live batch right here — mint the B-NN, grill what its weight demands, append the rows; no /gg:plan ceremony. Checkpoints WORK after every row so /clear + /gg:go always resumes cleanly. On the last row it closes the batch — green full suite, the user walks the Try list live, consumed records are swept (deleted; git keeps history), one close commit if the policy says so. Run it after /gg:new or /gg:plan, and again while rows remain."
model: inherit
disable-model-invocation: true
---

# /gg:go — Build the next row(s)

You implement the board **to the highest bar**, one row at a time. You work in the project
directory (cwd); state lives in `<cwd>/.gg/`. Method and formats:
`${CLAUDE_PLUGIN_ROOT}/gg-shared/METHOD.md` + `${CLAUDE_PLUGIN_ROOT}/gg-shared/FORMATS.md` — read
both now.

## 0. Precondition
Read ONLY `.gg/WORK.md ## State` and `## Where to resume` — routing must cost nothing; the full
orient (§1) happens only once this gate passes:
- **No `.gg/`** → **stop**, route to `/gg:new`.
- **`state: shaping`** → the kickoff/plan isn't finished. **Stop**, route to `/gg:new` (batch 0) or
  `/gg:plan`.
- **`state: idle`** → nothing on the board. **Stop**, route to `/gg:plan` (or `/gg:fix` for one small
  decided thing).
- **A `Blocked:` note whose condition still holds** → say so and stop — don't pay the orient to
  rediscover it.
- **`state: building`** → proceed. If no pending row remains, run the batch close (§5).

## 1. Orient (cheap by design)
Read whole: `WORK.md` (the board — which row is next), `CONTEXT.md`, `RUNBOOK.md`,
`NOTES.md ## Assumptions`. Read `git log --oneline -5` (the journal tail) — commits since the last
close that aren't gg's mean off-lane work landed: say so and offer a one-line record catch-up (fold
what changed into `DESIGN`/`RUNBOOK`/`PRODUCT` or a `B-NN`) before building on top of it. Read by
section, on demand: `DESIGN.md` sections and the ADRs (`ls .gg/adr/`) the row touches; `PRODUCT.md`
when the row bears on a "done and perfect" clause; the `F-NN`s the row cites. Orient reads — it
never measures `.gg/` sizes; bounds are the close's job (§5, `FORMATS.md`).

## 2. Durable start (first build of the batch only)
If Provenance is still a placeholder: record the base commit (`git rev-parse HEAD`, or
`unversioned`), the pre-existing dirty paths (`git status --porcelain` — the user's, off-limits), an
empty owned-paths list. Run the **RUNBOOK full suite** and record the result in Provenance's
**Suite baseline** line (no full-suite command exists yet — batch 0's first rows — → record
`none (no suite yet)`; pinning the command into `RUNBOOK.md` is part of the first rows). If already
red, say so and decide with the user before building on it. Never commit to set this baseline.

## 3. Implement the row to the bar
- Build **only this row**, complete and robust; cover the edge cases; **write tests** mapping to its
  done-when (a user-triggered write path is verified through the **real route**, end to end —
  METHOD.md → Evidence). Run the focused check and record its real result.
- **A `show` row**'s definition of done is *it runs and is watchable* via its done-when command — a
  thin but real spine, no stub. Run it yourself, then stop and invite the user to look (§4a).
- **A row whose deliverable is user-facing documentation** (README, tutorial, guide, reference,
  conceptual doc) is written with the **`diataxis` skill**: classify with its compass, follow the
  form's rules. `.gg/` files are record, not documentation — their shapes stay `FORMATS.md`'s.
- Tempted to stub, defer, or TODO? Apply METHOD.md's test — the honest moves are a later row
  (record it on the board) or a boundary taken to the user. Never a silent drop.
- **New scope mid-row**: if the user asked for it now, fold it (§3a) — after finishing the row in
  flight. Otherwise jot it — a `B-NN` to `BACKLOG.md ## New` (METHOD.md → Capture) — **return to
  the row**, and triage it at the next checkpoint the user sees via the capture **triage card**
  (METHOD.md → Veto, not go-ahead): your read taken and reported — an S fold executed (§3a), a
  backlog resting in `## New`; an M/L fold or a discard only offered. Never derail the row; never
  let captures pile up untriaged.
- **A debugging detour runs the red loop** (`fix.md` §1): one command that already goes red on the
  bug before any theory; falsifiable hypotheses shown ranked; `[DEBUG-xxxx]`-tagged instrumentation,
  cleaned by one grep.
- **An external wall** (missing credential, third party, a product decision only the user can make):
  record `Blocked: {reason} / unblock when {condition}` in "Where to resume" and stop — don't fake or
  cut. Record ADRs on the three criteria; observations as `F-NN`s.

### 3a. Fold — scope the user wants in THIS batch

Folding is go's move — no `/gg:plan`, no state flip, no gate beyond the user's own ask, whatever the
weight:
- Mint the `B-NN` (bump `next-id:`), weigh it **S/M/L with a one-line why** (plan's scale).
- Grill what the weight demands, inline: **S** — zero questions (but an ambiguous capture earns its
  one deciding question); **M** — 1-3; **L** — the full grilling (METHOD.md), `DESIGN.md` edited in
  place, an ADR when the three criteria hold.
- Append the row(s) — or insert them where ordering matters (before a regenerate/deploy row) — with
  real done-whens, extend `## Try`, and note the fold in "Where to resume": *"folded {date}: B-NN as
  row(s) {K–L}"*.
- The fold itself is never a reason to route to `/gg:plan`. Route there **only** when the look/new
  scope invalidates the *remaining* board — that's plan's re-scope in place, a redesign, not a fold.

## 4. Checkpoint and stop
Update `WORK.md`: row → `done`, add touched files to Owned paths, set "Where to resume" to the next
row. **No bound-keeping here**: the checkpoint never measures `.gg/` sizes or trims record content —
bounds are checked at the close (§5) and nowhere else in go (`FORMATS.md`). Commit per policy
(`gg(b{N}): row {K} — …`, pathspec-scoped). Breadcrumb: *"Batch {N}: row {K}/{M} done. Next:
`/clear` + `/gg:go` (row {K+1} — {where})."* If this session captured `B-NN`s, the breadcrumb
carries their **triage card** (METHOD.md → Capture): one card for all of them, dispositions taken
and reported with their why and way back — silence leaves them standing, and the next `/gg:go`
runs with no round trip.

### 4a. A `show` row just closed
Run its done-when yourself — on the surface the user actually judges: if that's the deployed product
on their phone, the show's job includes proposing that deploy (per the `deploy:` policy), not
staging a local stand-in they won't look at. Then make the breadcrumb a *look-at-this*: invite the
user to run it and react now, and **record the wait in "Where to resume → Notes"** (*"show row {K} awaiting the user's
look"* — cleared when the reaction is routed), so a fresh session never builds past an un-looked-at
show. Route the reactions (METHOD.md → Capture): the verdict on the `[discovered]` target → its
✓ mark in `PRODUCT.md` (or an `F-NN` if it observed something beyond the clause); wanted changes /
bugs → `B-NN`s. **If the look means the remaining rows must change**, stop and route to `/gg:plan`
(re-scope in place — it keeps the done rows); otherwise the batch stands.

## 5. Last row done → close the batch
- **Enumerate every outward action of this close upfront** (deploy, regenerate, anything
  irreversible), each with its rollback named, and take **one** explicit go for the set — deploys
  run per the `deploy:` policy (METHOD.md → Safety). Anything not enumerated still asks separately.
- Run the **RUNBOOK full suite** — it must be **green**; record the delta against Provenance's
  Suite baseline. (You may delegate the run to a subagent and take back only the counts, keeping
  test spew out of context.) A `question` batch's harness is real code under this suite; only its
  *answer* is open — record the measured result as an `F-NN`, close each "answered:" row citing it
  (`yes`/`no`/`inconclusive` all honest), and edit/remove the answered Unknown in `PRODUCT.md`.
- **Self-accounting, spoken** (METHOD.md → Evidence): list everything simplified/deferred/defaulted
  **and anything built that no row or clause asked for**; give each its honest home or justify it. Ask the PRODUCT-conformance question: does the product now
  meet "done and perfect", or what remains?
- **Present the Try list (`WORK.md ## Try`) and STOP — the verdict gates the close.** The user walks
  the load-bearing flows by name, not a free visual pass, and gives a live verdict; the sweep, the
  `state: idle` flip, and the close commit happen only after it lands and is acted on. Acting on it:
  a `[discovered]` clause someone actually watched gets its ✓ in `PRODUCT.md`; reactions become
  `B-NN`s; a failing flow is captured as a `[bug]` and fast-tracked with `/gg:fix` — never patched
  inline outside the record. User can't walk it now → record *"Blocked: awaiting Try walk / unblock
  when the user runs it"* in "Where to resume" and stop; a resumed close skips straight here.
- **Sweep by deletion** (git keeps *committed* history): mark the last row done; delete the applied
  items' traces from `BACKLOG.md` if any remain; delete consumed `A-NN`s and done `F-NN`s from
  `NOTES.md` (the test: reversing it now would be a change to shipped behavior → consumed; a finding
  with no live consumer is consumed at the second close it survives — `FORMATS.md`). **The
  close's own `F-NN` always survives** — it informs the next plan. **Prune accretion back to current
  truth** wherever this batch left it (`FORMATS.md` bounds): a CONTEXT entry beyond definition +
  _Avoid_, a RUNBOOK one-time procedure that has run, a PRODUCT clause's superseded ✓ — deletion,
  never an archive. **Measure every whole-read file now** (`wc -c`) — never trust a standing list;
  this close is the only point in go that measures. A file this batch pushed over its bound is
  pruned to **~¾ of the bound** (`FORMATS.md`), never to just under the line; a file over its bound
  from *before* this batch is the record pass's job (`/gg:where --audit`):
  say so, don't cut another batch's truth to a number. Leave surviving blocks'
  references to deleted ids intact (`FORMATS.md`). **If the blocks were never committed** (policy
  `never`, or every ask declined), deleting them is permanent: say so and get a yes, or leave them
  and only mark the close (METHOD.md → Commits). Add the entry to `## Last closes` — ONE line,
  ≤200 chars, measured (cap 5) — and set `state: idle`.
- Commit per policy: `gg(b{N}): close — applied {B-NNs} · consumed {A-NNs}` with the suite line in
  the body. **The close touches WORK + NOTES (+BACKLOG/PRODUCT/CONTEXT/RUNBOOK only where content
  changed or a bound demands pruning) — nothing else.**
- Breadcrumb (post-verdict): *"Batch {N} closed — verdict acted on: {one line}. Bring the next batch
  to `/gg:plan` (or one small thing to `/gg:fix`)."*
