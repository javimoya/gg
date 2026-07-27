---
description: "Kicks off a new gg project from a vague idea, in one arc. Runs divergent brainstorming then convergent grilling to pin a sharp PRODUCT (the destination), then designs the whole product (DESIGN — data model, architecture, seams) and writes the batch-0 board — so the next session can start building with /gg:go. Sets the commit policy once (ask/auto/never). Resumable: an arc cut mid-grilling checkpoints to WORK and re-running /gg:new continues it. Once batch 0 is building it does nothing and routes you onward."
model: inherit
disable-model-invocation: true
argument-hint: "[the idea]"
---

# /gg:new — Kick off a project (vision + whole-product design, one arc)

You turn a vague idea into a **sharp destination and a designed batch 0** in one resumable arc — the
user never wanted these as two ceremonies. You work in the project directory (cwd); state lives in
`<cwd>/.gg/`. Method and formats: `${CLAUDE_PLUGIN_ROOT}/gg-shared/METHOD.md` +
`${CLAUDE_PLUGIN_ROOT}/gg-shared/FORMATS.md` — read both now; the method is never copied into the
project.

## 0. Precondition
Read `.gg/WORK.md ## State` if it exists:
- **No `.gg/`** → a new project. Proceed to §1.
- **`state: shaping`, `batch: 0`** → a prior kickoff was cut mid-arc. **Resume**: read `WORK.md`
  whole (its "Where to resume" holds the open questions), the partial `PRODUCT.md`, `CONTEXT.md`,
  and `DESIGN.md` if present — continue from the first incomplete step (§2, §3, or §4: a written
  board still at `shaping` means only the sign-off remains). Do not re-scaffold.
- **Anything else** → the project is already kicked off; `/gg:new` runs once. **Stop** and route to
  `/gg:where`.

## 1. Scaffold
- `mkdir -p .gg`; create `WORK.md` (per `FORMATS.md`) **header first** so any surviving `.gg/` always
  carries a state: `state: shaping`, `batch: 0`, `kind: build`, `commit: ask`, `deploy: ask` (both
  provisional — confirmed in §3). Create `PRODUCT.md` as a skeleton. `CONTEXT.md`, `DESIGN.md`, `NOTES.md`, `BACKLOG.md`,
  `RUNBOOK.md`, `adr/` are created lazily when first written.

## 2. Grill the destination (METHOD.md → Grilling)
- **Diverge first**: widen the space — options, prior art, lateral ideas the user hasn't named, risks,
  "have you considered X?". Then **converge**: one question at a time, with your recommended answer,
  to pin the destination — the problem, who it's for, what it is and is NOT, the "done and perfect"
  bar, the non-negotiables. Sharpen terms into `CONTEXT.md` inline. The input is `$ARGUMENTS` if
  passed, else what the user brings.
- Write `PRODUCT.md` as it firms up (`FORMATS.md`): tag each "done and perfect" clause `[declared]` or
  `[discovered]` — a felt/emergent property the user can only judge by watching it run is
  `[discovered]` (sharpen it by showing contrasts, not adjective menus). An open/empirical idea names
  its central open question under Unknowns — answered later by a `question` batch, not pinned now.
- A decision about the product being pinned belongs **in this grilling**; a genuinely-later idea
  ("someday we could X") is still jotted as a `B-NN` (METHOD.md → Capture — the backlog may be born
  during kickoff), then the grilling resumes.

## 3. Design the whole product (same arc — no new command, no /clear)
- Grill the **load-bearing** design decisions and write `DESIGN.md` (`FORMATS.md`): the data model
  and architecture **settled whole up front** — the irreversible foundation and the seams decided
  once, so later batches extend instead of re-layering. Where a property space genuinely can't be
  enumerated yet, design an extension point; never as a dodge for a knowable schema. Record the
  hard-to-reverse calls as ADRs (the three criteria); every other choice becomes an `A-NN` in
  `NOTES.md` — **high-blast decisions are grilled, never defaulted**.
- Pin run/verify commands into `RUNBOOK.md` as they firm up.
- **Ask the one-time configs** (together, one question each at most):
  1. **Commit policy** — *"May gg commit its own record and the code it writes? `ask` (propose at
     each close — recommended) / `auto` / `never`."* Write the answer to the WORK header.
  2. **Deploy policy** — *"How do deploys run? `ask` (propose each, named with its rollback, wait
     for the yes — recommended) / `user-runs` (gg composes the exact command, you run it yourself) /
     `auto` (a standing yes for the named deploy command at show/close points)."* Write it to the
     WORK header (METHOD.md → Safety).
  3. If the cwd is **not a git repo**, offer `git init` (git is gg's only history — without it,
     deleted record blocks are unrecoverable; `never` + no git means WORK "Last closes" is the only
     trace).
- **Write the batch-0 board** into `WORK.md` (`FORMATS.md`): the whole product decomposed into rows
  sized so each fits one fresh `/gg:go` session, **foundational-first** — a thin vertical spine
  through the whole stack before breadth. Batch-0 rows carry short names alone (no `B-NN` — they
  were never backlog). Give each row a one-line observable **done when**. Place a **show** row where
  the riskiest `[discovered]` clause first becomes judgeable — as early as the expensive-to-retrofit
  foundation allows, and **never as the final row** (the close's Try walk is the batch's last look);
  a product with no `[discovered]` clause needs no forced show. Write the `## Try`
  block (deliverable + how to see it + the load-bearing flows); Provenance stays a placeholder
  (`/gg:go`'s durable start fills it).

## 4. Sign-off (one consolidated summary, one veto question)
Present together: the destination in one paragraph, the design's load-bearing calls, the
high/medium-blast `A-NN`s taken (low ones listed, not walked), where the first show lands and why it
can't be earlier, and the board. Take **one** veto question (free-form open — that's where real
vetoes arrive). Then set `state: building` in WORK.

## Close
Persist everything written; commit per the policy just set (`gg(b0): plan — {M} rows`,
`FORMATS.md`); breadcrumb: *"Batch 0 designed: {M} rows on the board. Next: `/clear` then `/gg:go`."*

**Cut mid-arc**: persist the partial PRODUCT/CONTEXT/DESIGN, leave `state: shaping`, write the open
questions into WORK "Where to resume", breadcrumb: *"Kickoff checkpointed at {where}; `/clear` +
`/gg:new` continues."*
