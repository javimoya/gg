---
description: "The work loop: bring one thing — a feature pitch, a bug (screenshot welcome), backlog ids — and it goes from ask to landed commit in one conversation. Orients cheaply (the cited B-NN blocks, the DESIGN sections touched, CONTEXT, the RUNBOOK suite command — never the whole record), grills only what has design weight (a decided bug gets zero questions; recommendation first, ELI18), builds at the size the change asks — directly when small, through implementation subagents when it spans layers (each prompt a self-sufficient contract, the verified API handed forward, parallel only when truly independent, every diff reviewed by this session) — proves it (green suite, real routes), and lands ONE commit carrying code + record together, citing the B-NN. Ideas that surface on the way are captured as B-NNs without derailing. Ships per RUNBOOK's standing Deploy convention."
model: inherit
disable-model-invocation: true
argument-hint: "[a pitch, a bug, or backlog ids]"
---

# /gg:go — Take one thing from ask to landed commit

You take what the user brings — a feature pitch, a bug, backlog ids — and land it: grilled at its
weight, built at the bar, proven, committed with its record. You work in the project directory
(cwd); state lives in `<cwd>/.gg/`. The method and the record's shapes:
`${CLAUDE_PLUGIN_ROOT}/gg-shared/GG.md` — read it now.

## 0. Precondition
No `.gg/` → not a gg project: say so and route to `/gg:new` — or just help without gg.

## 1. Orient (cheap by design — never the whole record)
Read: the `B-NN` blocks the ask cites, and the `BACKLOG.md` header (`next-id:`); `CONTEXT.md`
whole (it is the language); `RUNBOOK.md`'s Full suite and Deploy sections; `DESIGN.md ## Product`
plus **only the sections the change touches**; the ADRs it touches (`ls .gg/adr/`);
`git log --oneline -5`. That's it — DESIGN is read by section, and nothing here sweeps the record.

## 2. Weigh, then grill only what has weight
- **A decided bug or tweak**: zero questions. An ask that admits two readings earns its **one**
  deciding question — ambiguity, not size, is what makes questions.
- **Design weight**: grill per GG.md — one question at a time, recommendation first, ELI18;
  research is self-answered from primary sources; judgement, taste, and product intent are the
  user's. Sharpen resolved terms into `CONTEXT.md`; note the calls that change `DESIGN.md`; an
  ADR when the three criteria hold. An item marked `Decided` executes its recorded calls —
  never re-grilled.
- **Mint the id(s)**: assign `next-id:`, bump the header. Work brought fresh needs no BACKLOG
  block — the landing commit is its record; an item taken from the backlog keeps its block until
  the landing commit deletes it.

## 3. Build — the size decides who types
- **Small and local** → build it directly.
- **A slice spanning layers or surfaces** → delegate to implementation subagents and stay the
  reviewer. Slice per layer or surface; each prompt is a dense, self-sufficient contract: the
  repo idioms and files to mirror (file:line), the `CONTEXT.md` terms to use verbatim, the
  **verified API of the previous slice pasted in**, hard scope fences ("this layer ONLY — do not
  touch …"), the focused check to run, and the instruction to report contradictions loudly
  rather than absorb them. Sequential when each slice feeds the next its API; **parallel only
  for genuinely disjoint surfaces** (git worktrees when they would collide). A subagent's open
  question is relayed to the user with the agent's recommendation — never absorbed, never
  guessed.
- **You review every diff** against the ask and the bar — sweep for TODO/stub/mock leftovers and
  anything built that nobody asked for — and run the checks yourself: an agent's green claim is
  not evidence.
- **A non-obvious bug runs the red loop first**: one command that already goes red on the bug
  before any theory — usually the pinning test, written first; minimise the repro until every
  element is load-bearing. Hypotheses falsifiable ("if it's X, changing Y flips it") and shown
  ranked before testing any — the user's domain knowledge re-ranks them instantly.
  Instrumentation tagged `[DEBUG-xxxx]`, cleaned by one grep. Add the pinning test when the
  defect class merits one (a logic bug: yes; a label typo: say why not, and skip).
- **Strays**: an idea or bug that isn't this work → capture the `B-NN`, return. New scope the
  user wants NOW is simply more of this work — weigh it, grill what its weight demands, build
  it. An **external wall** (missing credential, third party, a decision only the user can make)
  → say so and stop; never fake or cut around it.

## 4. Prove it (GG.md → Evidence)
Run the RUNBOOK full suite — **green, or red only where it was already red** (say which). A
user-triggered write path is verified through the real route, end to end. Spot-run what the
tests don't see. Then one honest line of self-accounting: anything simplified, deferred, or
built unasked is named and homed (a `B-NN`, or removed) — nothing lands with silent cuts.

## 5. Land it
**One commit, code + record together** (GG.md → Commits): title `{area}: {summary} (B-NN)`; the
body carries what a reader needs — a bug's root cause and its lesson live here, not in the
backlog. In the same commit: the applied `B-NN` blocks deleted from `BACKLOG.md`;
DESIGN/CONTEXT/adr edited only where the shape or language actually changed. Pathspec-scoped;
never push. A multi-item ask lands separable changes as separate commits, each citing every
`B-NN` it applies.

Then ship per **`RUNBOOK ## Deploy`**: what the convention green-lights runs now and is
announced; everything else waits for its explicit ok (GG.md → Safety).

Breadcrumb: one line — what landed + what's next.
