---
description: "The record's diet — run it when .gg/ has grown stale or heavy, at your pace, not a ceremony's. The one command that reads the record whole: it reports, per file, what it would reconcile — applied and stale B-NNs to sweep, narrative to prune out of capture blocks, DESIGN/CONTEXT/RUNBOOK accretion beyond current truth, bounds (BACKLOG/CONTEXT/RUNBOOK 32KB, DESIGN 64KB), stale ADRs rewritten in place, and the version stamp vs the plugin (an applicable CONVERSION.md conversion is offered as its own question) — then applies on one yes: discards with no other home ask individually, and it commits as gg(tidy): …. Without the yes, it has changed nothing."
model: inherit
disable-model-invocation: true
---

# /gg:tidy — Return the record to bounded current truth

You are the record's diet: report first, apply on one explicit yes. This is the one command that
reads `.gg/` whole. Shapes, bounds, and register rules:
`${CLAUDE_PLUGIN_ROOT}/gg-shared/GG.md` — read it now. Until the yes, you change **nothing**.

## 1. Sweep (read-only — you may delegate the read to a subagent and take back only findings)
Check, in order:
- **Version, first**: the `BACKLOG.md` header's `gg:` stamp vs the plugin's version
  (`${CLAUDE_PLUGIN_ROOT}/.claude-plugin/plugin.json`), judged via the conversion index
  (`${CLAUDE_PLUGIN_ROOT}/CONVERSION.md`). A format-changing release crossed → the reconcile is
  **that conversion, offered after the report** — and it reconciles shape drift wholesale:
  report only what the conversion won't fix. Stamp merely older, no index row crossed →
  conforming; the pass refreshes the line. No stamp → the pass adds it once the shapes conform.
- **BACKLOG**: blocks whose work already landed (`git log --oneline` titles citing the id);
  blocks grown into chronicles — prune each back to its capture (the story is in git); stale
  agent-minted items never picked up → a keep-or-discard call per item; duplicate ids; a
  `next-id:` at or below an id in use.
- **DESIGN / CONTEXT / RUNBOOK — the register**: narrative history anywhere ("originally…",
  "then we changed…"); a DESIGN section describing what the product no longer has; a CONTEXT
  entry beyond definition + _Avoid_ (never trim the _Avoid_ lists themselves); a RUNBOOK command
  that no longer exists, or a completed one-time procedure; a missing `## Deploy` where the
  project ships.
- **adr/**: a stale body, or an amendment block → rewritten to current truth in place.
- **Bounds** (GG.md): measure now (`wc -c`) — BACKLOG/CONTEXT/RUNBOOK 32KB, DESIGN 64KB. An
  over-bound file is pruned to **~¾ of its bound** — per-file judgement, never one joint sweep:
  pruning files together is how truth gets cut to hit a number.
- **Language & secrets**: non-English prose that isn't a marked user quote; anything that looks
  like a secret value.

## 2. Report
Per file: `{file}: {problem} → {the reconcile}`, with sizes where bounds bite (before → target).
If clean: *"Record consistent — nothing to tidy."* — and stop.

## 3. Apply, on one yes
One yes covers the pass — with two exceptions. Each **discard with no other home** (a backlog
item the user may still want; an entry nothing else records) asks individually before it's cut.
An applicable **conversion is its own question, never bundled**: name it, one line on what it
does, note its procedure opens with a git snapshot; on its yes, execute it from `CONVERSION.md`
in this session — it ends by writing the new stamp and making its own commit.

Applying: verify each finding against the file immediately before editing (retract what no
longer holds); anchored, block-scoped edits only — never regex/sed bulk edits; deletion, never
an archive. Commit: `gg(tidy): {what the pass reconciled}` (pathspec `.gg/` only). Without the
yes, tidy has changed nothing.
