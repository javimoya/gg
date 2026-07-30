---
description: "Read-only GPS for a gg project. Reconstructs where you are from WORK.md, the backlog counts, and the recent git log, and tells you exactly what to run next. Changes nothing on its own. Pass --audit for a deeper integrity check of the .gg/ record (drift, dangling ids, stale done-whens, bounds) — this IS gg's audit, there is no /gg:audit — and after its report, one explicit yes lets the same session apply the reconciliations it just reported (the record pass, committed as gg(record): …). Useful after hand-edits, a conversion, or resuming a dormant project."
model: inherit
effort: medium
disable-model-invocation: true
argument-hint: "[--audit]"
---

# /gg:where — Where am I?

You are the project's GPS: reconstruct state from disk, say what's next, change **nothing** — the
one exception is the audit's apply step, which runs only on the user's explicit yes after the report
(below). You work in the project directory (cwd); state lives in
`<cwd>/.gg/`. Formats: `${CLAUDE_PLUGIN_ROOT}/gg-shared/FORMATS.md`.

## 1. Read the state (read-only)
- **No `.gg/`** → say so, route to `/gg:new`. Done.
- `WORK.md` whole: state / batch / kind / commit policy, the board, "Where to resume", the Fix log,
  "Last closes". `BACKLOG.md`: counts (`{K} new · {J} later`) and the newest titles. The **"It is"
  essence line** from `PRODUCT.md`. `git log --oneline -5` for what recently landed. Keep it quick —
  this is the GPS, not the audit.

## 2. Report (a few lines)
Project + one-sentence vision; **batch {N}, state, kind**; what recently shipped ("Last closes" +
git log); **you are here** — the next open question if `shaping`, the next row if `building`, "board
empty — bring the next batch" if `idle`, any `Blocked:` note verbatim; the backlog counts; and, when
`idle`, a one-line PRODUCT-conformance read (which "done and perfect" clauses carry a ✓, which
don't). End with the exact next command — e.g. *"row 3/6: `/clear` + `/gg:go`"* or *"`/gg:plan` when
you have the next batch (or `/gg:fix` for one small thing)"*. Name `--audit` as an option when
resuming after a gap or after hand-edits; do not run it unasked.

## --audit — read-only integrity pass (only when passed)
Check the record against `FORMATS.md` and report each finding as `{file}: {problem} → {the command
or edit that reconciles it}`; change nothing. You may delegate the sweep to a subagent and report
its findings. Check:
- **Header vs board drift**: `building` with every row done (an unclosed batch); `idle` with pending
  rows; a board bigger than one batch.
- **Dangling ids**: a `Relates`/`reverses`/`Leads to` pointing at an id that is neither live in
  `.gg/` nor recoverable in git history (`git log -S "B-NN"`) — a reference to a deleted
  applied/consumed id is **not** dangling (`FORMATS.md`); duplicate ids; a `next-id:` at or below an
  id in use.
- **Invented structure**: sections or fields not in `FORMATS.md`; an archive-like section anywhere;
  "Last closes" grown past its cap of 5, or an entry that is a paragraph instead of a line; a Fix
  log carrying lines from before the current batch open.
- **Bounds and register** (`FORMATS.md` bounds, METHOD.md → The record register): a file past its
  bound (WORK 16KB, other whole-read files 32KB) — for `BACKLOG.md` the reconcile is triage at the
  next `/gg:plan` (merge or discard items), never rewording blocks to fit; a done-when past ~500
  chars; a show row sitting as
  the board's final row; a CONTEXT entry carrying history or implementation beyond definition +
  _Avoid_; an ADR with an amendment block, a decision not statable from its opening sentences, or
  narrative beyond its measured evidence; narrative batch history
  living inside any current-truth file.
- **Evidence honesty**: a `[discovered]` clause carrying a ✓ with no named observation; a done row
  whose done-when names behavior the product no longer has; a `question` batch closed with no `F-NN`.
- **Staleness**: `RUNBOOK.md` naming commands that no longer exist; `CONTEXT.md` terms or `PRODUCT.md`
  clauses referring to removed surfaces; a `Blocked:` note whose condition reads met.
- **Backlog debt**: agent-minted `## New` items whose Captured date predates the last two
  `## Last closes` entries — report count and ages; the reconcile is `/gg:plan`'s keep-or-discard
  call at its gate, never a silent sweep.
- **Language & secrets**: non-English prose that isn't a marked user quote; anything that looks like
  a secret value.

If clean: *"Record consistent — no drift found."* For a live health check, point the user at the
RUNBOOK's full-suite command to run themselves — the audit itself executes nothing.

### The record pass — apply the findings, on one explicit yes
After the report, offer **once** to apply the reconciliations — including inherited over-bound files
the batch closes can't reach (per-file passes, never one joint sweep: each file's bound is its own
judgement, and pruning them together is how truth gets cut to hit a number). Rules: verify each
finding against the file before applying it (retract what doesn't hold); anchored block edits only;
deletion, never an archive; each genuinely destructive cut (an entry with no other home) asks before
it's made; commit as `gg(record): {what the pass reconciled}` (pathspec `.gg/` only). Without the
yes, where has changed nothing.
