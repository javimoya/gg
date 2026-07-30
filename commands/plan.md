---
description: "Opens the next batch in ONE ceremony — capture, triage, and design together, one gate. Bring whatever you have (a pasted list of bugs/ideas, items from the backlog, one thing or ten): it records each with a stable B-NN, folds duplicates, weighs every item S/M/L, grills ONLY what has design weight (a decided bug gets zero questions), and writes the board /gg:go builds from. Exactly one consolidated veto gate, always. Invoked without a chosen set ('what should we do next?') it advises first — explains the backlog in plain terms and recommends — touching nothing until you pick. Also the lane for a research batch (an open question closed by a measured answer) and for re-scoping a batch already under way when a show changed the remaining plan. Folding one item you want NOW into a live batch is /gg:go's move, not a plan."
model: inherit
disable-model-invocation: true
argument-hint: "[the items — pasted list, backlog ids, or one thing]"
---

# /gg:plan — Open a batch (capture + triage + design, one ceremony)

You turn whatever the user brings into a **board `/gg:go` can build with no further questions** — in
one session, with one gate. You work in the project directory (cwd); state lives in `<cwd>/.gg/`.
Method and formats: `${CLAUDE_PLUGIN_ROOT}/gg-shared/METHOD.md` +
`${CLAUDE_PLUGIN_ROOT}/gg-shared/FORMATS.md` — read both now.

## 0. Precondition
Read `.gg/WORK.md ## State`:
- **No `.gg/`** → no project yet. **Stop**, route to `/gg:new`.
- **`state: shaping`** → a kickoff or a plan was cut mid-arc. If `batch: 0` route to `/gg:new`;
  otherwise **resume this plan** ("Where to resume" holds the open questions).
- **`state: building`** → a batch is under way. **Adding items to it is `/gg:go`'s fold** (`go.md`
  §3a) — offer to run the fold right here rather than bouncing the user to another command, but it
  is a fold (mint, weigh, grill per weight, append, extend Try — no state flip, no batch bump), not
  a plan. Plan's own move on a live board is the **re-scope in place**: a show's look (or the
  user, explicitly) says the *remaining* plan must change. Then: keep every `done` row untouched
  (rows, Provenance, Fix log), redesign only the pending rows, fold in the new items (each still
  minted as a `B-NN`), re-place the show if it must re-run, **rewrite `## Try` for the re-scoped
  remainder** (keep the done rows' flows), and note in "Where to resume": *"re-scoped in place
  ({date}): rows 1–{T} stay done; {B-NNs} folded; rows {T+1}–{M} redesigned."* **A pending item the
  re-scope drops is never silently deleted**: re-create its block in `BACKLOG.md` (`## New` or
  `## Later`, keeping its `B-NN`), or discard it with its one-line reason at the gate.
  Do **not** bump `batch`. Everything else in this spec applies (weights, the single gate).
- **`state: idle`** → the normal case. Proceed.

## 1. Ingest (capture happens here — there is no separate capture step)
**Advisory entry**: invoked without a chosen set ("what should we do next?"), stay read-only —
explain the backlog items in plain terms (concrete examples, no jargon), recommend the next batch,
and touch nothing until the user picks; an advisory conversation that ends without a batch leaves
the record untouched. A `## Staged` group in BACKLOG is the standing answer: take the top group.
**Once the set is chosen, set `state: shaping` in WORK** — the arc runs under it (any death,
graceful or not, resumes via §0's shaping path; §4 flips it to `building`). Then gather the batch's
candidate set:
- **What the user brought**: `$ARGUMENTS` or the conversation — a pasted list of 9 numbered items is
  the normal case, not an exception. Mint each new item a `B-NN` (bump `next-id:` in the BACKLOG
  header; create `BACKLOG.md` lazily) — the items go straight onto this plan, they never wait in
  `## New`.
- **What the user points at** in `BACKLOG.md ## New` / `## Later` (by id, or "and the backlog ones"),
  or the top `## Staged` group when one exists (that grouping was decided ahead — take it, don't
  re-derive it). Items *not* brought in stay where they are — never invisible: open with one line of
  counts (*"backlog holds {K} new · {J} later"*) when anything stays behind. Name for a
  keep-or-discard call at the gate any **agent-minted `## New` item now stale** — its Captured date
  predates the last two `## Last closes` entries; stale agent captures never ride the backlog
  indefinitely.
- **Reconcile lightly** (METHOD.md → Capture): fold duplicates into the survivor (keep its `B-NN`),
  surface contradictions. An item that reverses a default notes `reverses: A-NN`.

## 2. Weigh and design (ceremony scales with weight, not ritual)
Classify every item — **S/M/L, each with a one-line why** (the why is the defense against a silent
cut hiding in a misclassified S):
- **S** — bug/tweak, no design contact. **Zero questions, zero design prose.** One board row with a
  done-when. But an S whose capture admits two readings (unknown cause, an ambiguous ask) earns its
  **one** deciding question — ambiguity, not size, is what makes questions.
- **M** — light design contact. 1-3 load-bearing questions; edit `DESIGN.md` in place only if the
  shape changes; real defaults become `A-NN`s.
- **L** — genuinely designed. Full grilling (METHOD.md), `DESIGN.md` edited in place, an ADR when the
  three criteria hold.
- An item marked **`Decided`** (`FORMATS.md`) is executed at its recorded calls, never re-grilled —
  even where they contradict an older record; only a genuinely new fork inside its scope may ask.

**Whatever the weights — even an all-S bug batch — check the whole set against `PRODUCT.md`**:
surface any contradiction with the destination or an "It is not" boundary before it gets built. An
item backed by an observed finding that proves a "done and perfect" clause wrong edits that clause
in place (evidence revises the destination; "it was hard" never does). While grilling: sharpen terms
into `CONTEXT.md`; pin new run/verify commands into `RUNBOOK.md`.

**A `question` batch**: when the set is an open question / `[exp]` items, set `kind: question` and
design the *search* — the experiment, the harness (real code under the green suite), the observable
signal, and what result answers it. Its rows' done-when is "answered: recorded as `F-NN`";
`yes`/`no`/`inconclusive` all close it honestly. **A mixed set opens as `build`** — an `[exp]` item
rides as a row with an `answered:` done-when.

## 3. Draft the board (still under `shaping` — no destructive writes yet)
Write the batch into `WORK.md` per `FORMATS.md`: bump `batch`, set `kind`, rewrite Board/Try/
Where-to-resume whole and reset Provenance to placeholders — prune the Fix log, **leave
`## Last closes` intact**. The Board: rows sized so each fits one fresh session — an L item may span
several rows, and **small items sharing a surface group into one row** (fewer, meatier rows beat a
parade of S rows; an S with no siblings is one row). Each row's **done when** (a `[bug]` row's
carries the broken-vs-expected essence; **≤500 chars, measured as written** — an over-cap cell is
rewritten before the gate, never left for the audit to report), the **show** rows only where a
`[discovered]` clause in play becomes judgeable mid-batch (riskiest first, as early as the
foundation allows — a bug-batch needs no show, and **never as the final row**: if the earliest
judgeable moment is the batch's end, place no show — the close's Try walk is that look), and the
`## Try` block (deliverable + how to see it + the load-bearing flows, citing B-NNs —
a user-triggered write or heavy job belongs on it by name). Record doc-sync the batch causes may
group into one S row for convenience — no `B-NN` of its own, and it never certifies (`FORMATS.md`).
**Inherited record overage is never batch work**: a `.gg/` file past its bound from before this
batch routes to `/gg:where --audit` (the record pass), never onto the board or into a `B-NN`.
**Do not touch `BACKLOG.md` yet** — the
block moves, deferrals, and discard deletions happen only after the gate, so a veto never has to
reconstruct deleted blocks.

## 4. The gate — exactly ONE consolidated veto question, always
Present one report: every item with its disposition and **weight + why**, the grilled decisions, the
high/medium-blast `A-NN`s (low ones listed), any discards with reasons, show placement and why not
earlier, and the board. Take **one** question with the free-form answer open — the gate is where
users *add* work, not just assent, so it is never skipped, and it is never two rounds. Name the
approving word in the question itself (*"add, cut, or change anything — or say **go**"*), and read a
bare "ok"/"no" as assent when nothing was raised — don't make the user guess which word you wanted. **Apply the
answer in one pass**: a gate-added S item folds straight onto the board; a gate-added M/L item
re-enters §2 (its questions are the application of the answer, not a new gate). Only now touch
`BACKLOG.md` — move the taken items' blocks out, send deferrals to `## Later`, delete discards
(reason already stated in the report; a "never" becomes a `PRODUCT.md` boundary). Then set
`state: building`.

## Close
Persist WORK/BACKLOG/NOTES (+DESIGN/CONTEXT/RUNBOOK/PRODUCT if touched); commit per policy
(`gg(b{N}): plan — {B-NNs} · {M} rows`); breadcrumb: *"Batch {N} open: {M} rows. Next: `/clear` then
`/gg:go`."* One sanctioned exception to the `/clear`: if the session still has plenty of context and
the batch is all-S, you may offer to start `/gg:go` right here — the user's call.

**Cut mid-plan**: persist partial state, `state: shaping`, open questions into "Where to resume",
breadcrumb: *"Plan checkpointed; {K} questions open. `/clear` + `/gg:plan` continues."*
