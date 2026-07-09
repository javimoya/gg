---
description: "Builds the open batch. Orients from WORK.md in seconds, then implements board rows to the full bar (with tests): one M/L row per fresh session, but consecutive S rows chain in a single session under hard stop conditions. Checkpoints WORK after every row so /clear + /gg:go always resumes cleanly. On the last row it closes the batch — green full suite, the user walks the Try list live, consumed records are swept (deleted; git keeps history), one close commit if the policy says so. Run it after /gg:new or /gg:plan, and again while rows remain."
model: inherit
disable-model-invocation: true
---

# /gg:go — Build the next row(s)

You implement the board **to the highest bar**, one row at a time — chaining small ones when it's
safe. You work in the project directory (cwd); state lives in `<cwd>/.gg/`. Method and formats:
`${CLAUDE_PLUGIN_ROOT}/gg-shared/METHOD.md` + `${CLAUDE_PLUGIN_ROOT}/gg-shared/FORMATS.md` — read
both now.

## 0. Precondition
Read `.gg/WORK.md ## State`:
- **No `.gg/`** → **stop**, route to `/gg:new`.
- **`state: shaping`** → the kickoff/plan isn't finished. **Stop**, route to `/gg:new` (batch 0) or
  `/gg:plan`.
- **`state: idle`** → nothing on the board. **Stop**, route to `/gg:plan` (or `/gg:fix` for one small
  decided thing).
- **`state: building`** → proceed. If no pending row remains, run the batch close (§5).

## 1. Orient (cheap by design)
Read whole: `WORK.md` (the board — which row is next), `CONTEXT.md`, `RUNBOOK.md`,
`NOTES.md ## Assumptions`. Read `git log --oneline -5` (the journal tail). Read by section, on
demand: `DESIGN.md` sections and the ADRs (`ls .gg/adr/`) the row touches; `PRODUCT.md` when the row
bears on a "done and perfect" clause; the `F-NN`s the row cites.

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
- Tempted to stub, defer, or TODO? Apply METHOD.md's test — the honest moves are a later row
  (record it on the board) or a boundary taken to the user. Never a silent drop.
- **New scope mid-row** (yours or the user's): jot it — a `B-NN` to `BACKLOG.md ## New` (METHOD.md →
  Capture) — and **return to the row**. Never derail the build.
- **An external wall** (missing credential, third party, a product decision only the user can make):
  record `Blocked: {reason} / unblock when {condition}` in "Where to resume" and stop — don't fake or
  cut. Record ADRs on the three criteria; observations as `F-NN`s.

## 4. Checkpoint, then chain or stop
Update `WORK.md`: row → `done`, add touched files to Owned paths, set "Where to resume" to the next
row. Then decide:

- **Chain** to the next row in this same session **only if ALL hold**: the next row is `S` · no
  `show` is waiting for the user · the session is comfortably under ~half its context · the row just
  closed needed no debugging detour longer than the row itself. Chain = go back to §3.
- **Otherwise stop.** Commit per policy (`gg(b{N}): rows {K–L} — …`, pathspec-scoped). Breadcrumb:
  *"Batch {N}: row {K}/{M} done. Next: `/clear` + `/gg:go` (row {K+1} — {where})."* One M/L row per
  fresh session is the rule; chaining S rows is the only widening.

### 4a. A `show` row just closed
Run its done-when yourself, then make the breadcrumb a *look-at-this*: invite the user to run it and
react now, and **record the wait in "Where to resume → Notes"** (*"show row {K} awaiting the user's
look"* — cleared when the reaction is routed), so a fresh session never chains past an un-looked-at
show. Route the reactions (METHOD.md → Capture): the verdict on the `[discovered]` target → its
✓ mark in `PRODUCT.md` (or an `F-NN` if it observed something beyond the clause); wanted changes /
bugs → `B-NN`s. **If the look means the remaining rows must change**, stop and route to `/gg:plan`
(re-scope in place — it keeps the done rows); otherwise continue the batch.

## 5. Last row done → close the batch
- **Enumerate every outward action of this close upfront** (deploy, regenerate, anything
  irreversible), each with its rollback named, and take **one** explicit go for the set. Anything not
  enumerated still asks separately.
- Run the **RUNBOOK full suite** — it must be **green**; record the delta against Provenance's
  Suite baseline. (You may delegate the run to a subagent and take back only the counts, keeping
  test spew out of context.) A `question` batch's harness is real code under this suite; only its
  *answer* is open — record the measured result as an `F-NN`, close each "answered:" row citing it
  (`yes`/`no`/`inconclusive` all honest), and edit/remove the answered Unknown in `PRODUCT.md`.
- **Self-accounting, spoken** (METHOD.md → Evidence): list everything simplified/deferred/defaulted;
  give each its honest home or justify it. Ask the PRODUCT-conformance question: does the product now
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
  `NOTES.md` (the test: reversing it now would be a change to shipped behavior → consumed). **The
  close's own `F-NN` always survives** — it informs the next plan. Leave surviving blocks'
  references to deleted ids intact (`FORMATS.md`). **If the blocks were never committed** (policy
  `never`, or every ask declined), deleting them is permanent: say so and get a yes, or leave them
  and only mark the close (METHOD.md → Commits). Add the one-line entry to `## Last closes` (cap 5),
  set `state: idle`.
- Commit per policy: `gg(b{N}): close — applied {B-NNs} · consumed {A-NNs}` with the suite line in
  the body. **The close touches WORK + NOTES (+BACKLOG/PRODUCT only if their content changed) —
  nothing else.**
- Breadcrumb (post-verdict): *"Batch {N} closed — verdict acted on: {one line}. Bring the next batch
  to `/gg:plan` (or one small thing to `/gg:fix`)."*
