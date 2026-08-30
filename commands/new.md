---
description: "Kicks off a new gg project from an idea, in one conversation: divergent brainstorming, then convergent grilling (one question at a time, always leading with its recommendation) to pin the destination and the load-bearing design — then seeds the record: DESIGN.md with the product up top, CONTEXT.md, RUNBOOK.md (including the standing Deploy convention), BACKLOG.md with its id counter and version stamp, and any ADR a hard call earned. No board, no batches, no policy questionnaire — it ends recommending the first /gg:go. Runs once per project; an interrupted kickoff continues by re-running it (the files written so far stand)."
model: inherit
disable-model-invocation: true
argument-hint: "[the idea]"
---

# /gg:new — Kick off a project (grill the destination, seed the record)

You turn an idea into a pinned destination and a seeded record, in one conversation. You work in
the project directory (cwd); state lives in `<cwd>/.gg/`. The method and the record's shapes:
`${CLAUDE_PLUGIN_ROOT}/gg-shared/GG.md` — read it now; it is never copied into the project.

## 0. Precondition
- **`.gg/` with a stamped `BACKLOG.md` exists** → already kicked off; `/gg:new` runs once. Say so
  and route to `/gg:go` (or `/gg:tidy` if the record looks stale).
- **A partial `.gg/`** (a prior kickoff cut short) → continue: read what exists, fill what's
  missing. Do not re-derive what's already written.
- **No `.gg/`** → proceed.

## 1. Grill the destination (GG.md → Grilling)
- **Diverge first**: widen the space — options, prior art, lateral ideas the user hasn't named,
  risks, "have you considered X?". Then **converge**: one question at a time, recommendation
  first, to pin the essence — the problem, who it's for, the **it-is-not** boundaries, the bar.
  The input is `$ARGUMENTS` if passed, else what the user brings.
- Then the **load-bearing design**: the data model, the architecture, the stack — settled whole
  up front, so later work extends instead of re-layering. High-blast calls are questions, never
  defaults; record the hard-to-reverse ones as ADRs (the three criteria). Where a property space
  genuinely can't be enumerated yet, design an extension point — never as a dodge for a knowable
  schema.
- A "someday we could X" surfacing mid-grilling is captured as a `B-NN` and the grilling resumes;
  a genuinely open central question becomes a line in `## Product` (and a `[exp]` item if an
  experiment should answer it).

## 2. Seed the record (GG.md shapes, exactly)
- `DESIGN.md` — `## Product` up top, then Shape / Data model / Stack & platform.
- `CONTEXT.md` — the terms the grilling sharpened.
- `RUNBOOK.md` — commands as they're known ("n/a" is honest for what doesn't exist yet), and
  **`## Deploy` the moment the project has a way to ship**: ask the user once how shipping runs —
  what may run unasked when a change lands, what always waits for their ok — and write it down.
- `BACKLOG.md` — `next-id:` (past whatever this arc minted), the `gg:` stamp read from
  `${CLAUDE_PLUGIN_ROOT}/.claude-plugin/plugin.json`, and the captured blocks under `## New`.
- If the cwd is **not a git repo**, offer `git init` — git is gg's only history; without it,
  deleted record blocks are unrecoverable.

## 3. Close
Commit: `gg(new): {project} — record seeded` (pathspec-scoped: `.gg/` plus anything the kickoff
wrote). End with the first build, concrete and recommended: *"First slice I'd build: {the thin
vertical spine through the stack}. `/gg:go {it}` starts it."*

**Cut mid-arc**: the files written so far stand; re-running `/gg:new` continues from them.
