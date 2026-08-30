# GG.md — the record and the habits (shared)

gg is a record with habits, not a workflow engine. The working loop — pitch, grill, build, review,
ship — belongs to the conversation; gg fixes on disk the part no session keeps well on its own: a
**bounded record of what is currently true**, and the habits that keep the work honest. Commands
read this file from the plugin (`${CLAUDE_PLUGIN_ROOT}/gg-shared/GG.md`); it is never copied into a
project. Project state lives in `<cwd>/.gg/` — **four bounded files plus `adr/`**. **History is
git's**: applied and superseded blocks are deleted, never archived — `git log -S "B-12"` recovers
anything; gg keeps no journals, no changelogs, and no state machine.

## The record — four files + `adr/`

### BACKLOG.md — capture only

```md
# Backlog — {Project name}
next-id: B-47
gg: 5.0.0

## New
### B-46 — [bug] {short title}   <!-- [bug] = defect in shipped behavior; [exp] = experiment -->
- **Captured**: {YYYY-MM-DD} — {user | agent}
- **Idea**: {the ask in the user's words; for a [bug], broken vs expected}
- **Touches**: {area(s), if known — optional}
- **Decided**: {YYYY-MM-DD} — {the settled calls, one line — optional: grilled and sealed}
```

Rules: **`next-id:` is the only counter** — every mint assigns the current value, then increments
the header; ids are stable, zero-padded (`B-05`), never reused. **`gg:` is the version stamp** —
the plugin version whose shapes this record follows; written at `/gg:new`, rewritten only by a
conversion (`CONVERSION.md`) or by `/gg:tidy` after verifying the shapes conform. **A block is a
capture, never a chronicle**: the ask, not the story of its fix — a root cause, a lesson, a
post-mortem lives in the landing commit's body or an ADR, never here. An agent-minted Idea opens
with one plain sentence its owner can read cold, days later. A `Decided` item was grilled and
sealed: execute its calls, don't re-ask. Applied items are **deleted in the commit that lands
them**; a discard is a deletion and waits for the user's yes.

### DESIGN.md — the current-truth design

```md
# Design — {Project name}

## Product
{The destination in a few lines: the essence, who it's for, the explicit **it-is-not**
boundaries (product decisions, said out loud — what prevents drift), and the bar it's held to.}

## Shape
{The architecture in a few sentences: major components, how they connect.}
## Data model
## Stack & platform
{Load-bearing choices; link the ADRs that record the why.}
```

Rules: **edited in place — always the current truth**; git diff is the design history; an ADR is
the why of a hard call. **Read by section, never whole.** Sections beyond this spine are added as
the design needs them — but every section describes the product as it is today, no archaeology.
Something the product will *never* do is an it-is-not boundary — re-deciding one is legitimate,
but it happens out loud, never by silently building over it.

### CONTEXT.md — the glossary

```md
# {Context name}
{One or two sentences on what this context is.}

## Language
**{Term}**:
{One or two sentences. What it IS, not what it does.}
_Avoid_: {the synonyms not to use}
```

Rules: only domain terms specific to this project; opinionated — pick the word, list the rest
under _Avoid_ (the _Avoid_ lists encode regression traps — the wrong word a fresh session would
reach for — and are never trimmed); updated the moment grilling resolves a term. An entry is the
definition plus its _Avoid_ list — no history, no implementation detail.

### RUNBOOK.md — how to run, verify, and ship

```md
# Runbook — {Project name}
## Prerequisites & setup
## Environment      {env vars by NAME + purpose; never a value}
## Full suite       {THE single canonical command}
## Focused tests
## Lint / build
## Deploy           {the standing convention: the command(s), what may run unasked when a change
                     lands, and what ALWAYS waits for an explicit ok — decided once, honored by
                     every session}
## Destructive paths
{Anything that writes data, deploys, sends, or is irreversible — flagged so no session runs it
blindly. Destroying a POPULATED store always stops for a yes.}
```

Rules: every entry copy-pasteable — the command, a one-line purpose, and the trap warnings that
keep it safe; war stories stay in git; a completed one-time procedure is deleted at the next tidy.
**`## Deploy` is the deploy policy**: written once (at `/gg:new`, or the first time the project
ships), it replaces per-session asking about what it already settles.

### adr/ — decisions

`.gg/adr/NNNN-slug.md`, created lazily; the slug states the decision
(`0007-sqlite-over-postgres.md`). The decision in **1–3 sentences up top** — context, decision,
why — and below it only measured evidence that defends the call. Offer an ADR only when all three
hold: hard to reverse · surprising without context · a real trade-off. A stale body is rewritten
to current truth in place — no amendment blocks; git keeps the old text. **An ADR is how a
decision is remembered, never enforced** (→ Decisions age).

### Bounds — checked by `/gg:tidy` and nowhere else

BACKLOG, CONTEXT, RUNBOOK ≤ **32KB**; DESIGN ≤ **64KB** (it is read by section). No working
session measures or trims — tidy does, on the user's yes; a prune cuts to ~¾ of the bound, never
to just under the line. **One fact, one home**: a datum lives once; every other mention is its
bare id — a fact kept in two homes is the fact that goes stale in one.

## Decisions age — the record is memory, not law

Every recorded decision — an ADR, an it-is-not boundary, a `Decided` item, a DESIGN choice — was
right in the context it was written in, and contexts move. None of them is a wall. When a new
idea conflicts with one, **present the idea on its merits and name what it would supersede**:
*"this breaks ADR-0023 — its why was {X}, which {still holds / has lapsed because Y}"* — then
let the user decide. Never withhold or handicap an idea because a record forbids it; never rank
options by record-compliance; never present a recorded decision as an impossibility — a
constraint is something the world imposes, a decision is something the project chose and can
un-choose. Superseding a stale decision out loud is the record working, not breaking. Two things
stay: executing a sealed `Decided` item doesn't re-open its calls (that protects the user from
re-grilling — but a new idea against those calls is still presented, never suppressed), and
**the user's yes is what supersedes** — on it, the old record is rewritten or deleted in the
landing commit like any other record edit; until it, the decision stands.

## Capture (inline, any session — no command)

The moment an idea, bug, or "we should…" surfaces that isn't the work in hand: mint the `B-NN`,
write the block, get back to work — two lines of chat, no triage ceremony. An observation about
the running product folds into the file that owns it (DESIGN, RUNBOOK), becomes a `B-NN` if it
implies work, or belongs to the commit body. A bug in the very thing being built right now is not
a capture — fixing it is the bar.

## The bar

**The final, complete, robust product — never the easier option over the better one.** No TODO,
stub, left-in mock, or `// for now`. No padding either: an abstraction nothing needs is not
quality. Before leaving anything for later, the test: *will the end state of the product be any
less complete, robust, or clean this way?* Yes → forbidden — do it right, or decompose honestly
(a `B-NN`, or an it-is-not boundary taken to the user). No, it's only order → fine. When the
shape changes, **recreate, don't shim** — no migrations or backward-compat layers unless the user
explicitly asks.

## Evidence

Done means observed: a run test, a driven route, a watched behavior — never "should work". The
full suite is green, or red only where it was already red (say which). A user-triggered write
path is verified through the real route, end to end, never only isolated units. A test's expected
value comes from an independent source of truth — a known literal, a worked example, the spec —
never recomputed the way the code computes it. Before landing, one honest line of
self-accounting: anything simplified, deferred, or built that nobody asked for is named and given
its home (a `B-NN`, or removed) — nothing lands with silent cuts.

## Grilling

Only design weight earns questions — a decided bug gets zero. One question at a time, always
leading with the recommended answer and why; a question without a recommendation is work pushed
back onto the user. **Explain before you ask, at ELI18**: context first, one concrete example per
option, in the project's own terms (`CONTEXT.md`'s words, never its avoided synonyms). Questions
split two ways: what research can answer (the code, the docs, the API's observed behavior —
primary sources, never parametric memory) is answered, never asked; judgement, taste, and product
intent are the user's alone and never self-answered. Sharpen resolved terms into `CONTEXT.md` the
moment they land.

## Safety (unconditional)

- **You own only what this session changed.** The user's dirty paths are never claimed, reverted,
  or committed.
- **Never the blunt instruments**: no `git reset --hard`, `git clean`, `git checkout -- .`,
  `git stash`, no wholesale reverts. Undo precisely, file by file, only what you created.
- **Irreversible or outward actions** — delete, overwrite, drop or recreate a *populated* store,
  send, push, and any shipping step `RUNBOOK ## Deploy` doesn't green-light — name the rollback
  and wait for the yes.
- **Never push. Never any outward action on gg's initiative.**
- **Secrets stay out of `.gg/`** (it's committed): env var names, never values. **Records are
  written in English** (verbatim user quotes stay in their language, marked as quotes).

## Commits

One coherent change lands as **one commit carrying the code and the record together** — the
DESIGN/CONTEXT/ADR edits it caused, and the applied `B-NN` blocks deleted — title
`{area}: {summary} (B-NN)`, the body carrying what a reader needs (a bug's root cause and its
lesson live here). Always pathspec-scoped: `git add .gg/ {owned paths}` — the user's paths are
never swept in. gg's own operations commit as `gg(new): …`, `gg(tidy): …`, `gg(convert): …`.
Skip committing (and say so) if the repo is mid-rebase/merge or otherwise not yours to commit.

## Output discipline

Act, don't narrate: batch tool calls, open with the result, report at natural checkpoints. Name
first, id after — *"the export button (B-46)"*, never walls of bare ids. No ritual disclosures,
no empty-bucket reports, no restating the user's answer in bold before moving on. End a working
session with one line: what landed + what's next.
