# METHOD.md — how gg works (shared)

The method every gg command runs on. Commands read this from the plugin
(`${CLAUDE_PLUGIN_ROOT}/gg-shared/METHOD.md`); it is **never copied into a project**. Project state
lives in `<cwd>/.gg/` — seven bounded files plus `adr/` (`FORMATS.md`). History lives in **git**:
`.gg/` is versioned, so anything deleted from it is recoverable (`git log -S "B-12"`); gg keeps no
journals, archives, or changelogs of its own.

## The bar

**Always produce the final, complete, robust product — at the full bar of the agreed product.**
Quality is not negotiable; **effort is never a reason to cut**. The complete product is reached
**across batches**, not in one shot: each batch is at full bar for what it builds, and every gap is a
recorded item or assumption — visibly, never silently. "Close, not perfect, then refine" is legitimate
*because* nothing is quietly dropped.

- **No "v1 / later" as an excuse to ship less.** No `TODO`, stub, left-in mock, `// for now`, or
  half-finished implementation in the product.
- **Never pick the easier option over the better one.**
- gg assumes a product under active development: when the shape changes, **recreate and reseed** —
  no migrations, no backward-compat shims, no preservation tests, unless the user explicitly asks for
  them (then they're work like any other, at full bar).

### The test (before ever "leaving something for later")

> **"Will the end state of the whole product be any less complete, robust, or clean if I do it this
> way?"** Yes → forbidden, it's a cut — do it right or decompose it honestly. No, it's only order →
> allowed.

### Decompose, don't drop — the honest homes

When you're tempted to defer something, or new scope surfaces mid-work, decompose it into the one home
it belongs in. **A thread parked anywhere else — prose, chat, Claude's native memory — is a silent
drop.** Never use Claude's native memory for project state.

| The thing | Home |
|---|---|
| Continuation of the work in progress | a later **row** on the WORK board |
| A future idea / change / bug / experiment | a **`B-NN`** backlog item (capture, below) |
| A low-stakes decision not worth asking now | an **`A-NN`** assumption (recorded default) |
| An observation of what the running product did | an **`F-NN`** finding |
| An architectural decision (hard to reverse + surprising + real trade-off) | an **ADR** |
| Something the product will *never* do | a **boundary** in `PRODUCT.md` "It is not" (a product decision, said out loud) |
| A "done and perfect" clause that evidence proved wrong | **edit the clause in place**, citing the observation — only evidence revises the destination, never "it was hard" |

The decisive trap: **out of this batch ≠ out of the product.** If there is a "later" for it, it's a
`B-NN`, never a boundary.

## Grilling (used by `/gg:new`, and by `/gg:plan` for M/L items)

The interrogation that reaches a sharp, shared understanding before code — fast because it records
good defaults instead of asking everything.

- **One question at a time**, wait for the answer. **Always lead with your recommended answer** and
  why, plus the alternatives you weighed — a question without a recommendation is work pushed back
  onto the user. A recommendation never proposes below the bar: a shim, stub, or compat-out is never
  the recommended option.
- **Ask in the concrete**: a question the user answers with "no te entiendo" was asked in the
  abstract. Lead with the example — the card, the screen, the number — and let the rule follow.
- **Explore before asking — questions split two ways**: a question research can answer (the code,
  the docs, an API's actual behavior) is **never put to the user** — answer it yourself, or hand it
  to a subagent and keep grilling while it runs, folding what comes back into the next
  recommendation. The user's questions are the ones only they can answer — judgement, taste,
  product intent — and there the reverse holds: never answer for them; a grilling that self-answers
  its own questions has stopped being one.
- **Ask the load-bearing questions; default the rest as `A-NN`s.** High-blast decisions — the data
  model, the stack, sync vs async, the core UX — are always questions, never defaults. Only low/medium
  blast-radius choices are defaulted. **The cut is the *unrecorded* assumption.**
- **Sharpen fuzzy terms** into `CONTEXT.md` the moment they're resolved; challenge terms that conflict
  with the glossary; stress-test with concrete scenarios; cross-reference what the user says against
  the code.
- **Diverge, then converge** (ideation): bring options, prior art, lateral ideas and risks the user
  hasn't named — then grill until pinned.
- **Show, don't only ask** for *felt* dimensions (look, tone, pace): sketch two or three vivid
  contrasts and record the reaction. A dimension that stays subjective becomes a `[discovered]`
  "done and perfect" clause, confirmed later by watching the product run — and it earns a **show** row
  on the board (`FORMATS.md`).
- **One consolidated sign-off, one veto question**: name the areas not yet probed, show the
  high/medium-blast defaults taken, and take a single free-form veto — never successive rounds of
  "tick the defaults to flip". Grilling is never a tool for reducing scope.

## Capture (inline, any session — there is no capture command)

The moment an idea surfaces, jot it and get back to work. Route by tense:

- **Future tense** ("we should…", "X is broken") → a `B-NN` in `BACKLOG.md ## New`: title (prefix
  `[bug]` for a defect in shipped behavior, `[exp]` for an experiment to run), date, who raised it,
  the idea in the user's words, what it touches. Mint the id from the BACKLOG header: **assign the
  current `next-id:` value, then increment it** (ids zero-padded: `B-05`).
  **Reconcile lightly** against the active backlog: fold a refinement into an existing item
  (note `refines: B-NN`), surface a contradiction, otherwise stand alone. An item that reverses a
  recorded default points at it (`reverses: A-NN`). No grilling, no design, no triage — that's
  `/gg:plan`.
- **Past tense** ("when I ran it, X happened") → an `F-NN` in `NOTES.md ## Findings`: observed /
  what happened / reading / leads-to. A finding is a fact, not a decision — if it implies a change,
  that change is its own `B-NN`, linked from **Leads to**.
- **Not capture**: a bug in the task you're building right now (fix it — that's the bar), or a cut
  you were tempted to make (the test above). One-line confirmation, then **return to the task** —
  never derail the build.
- **A capture is offered, never buried — one triage card, once**: items jotted while the user was
  away are surfaced at the next moment they're present — the row checkpoint, a show, the close — as
  **one card for the session's captures**, never a question per item. Each entry: the id + title,
  the Idea's opening sentence (the plain, concrete example `FORMATS.md` already requires — the card
  is where that sentence earns its keep), a weight guess, and your recommendation. Three
  dispositions, the user's call per item: **fold** into the live batch (`/gg:go` folds it, `go.md`
  §3a) · **backlog** (stays in `## New` for a later plan) · **discard** (delete the block; a
  "never" becomes a `PRODUCT.md` boundary, as at the plan gate). The card never blocks — no answer
  leaves the items in `## New` — and an offered item is not re-offered next session: `/gg:plan` is
  its next look. Silent accumulation across a batch is a drop with extra steps.

## Evidence (what "done" means)

- **A done-when closes only on real, observed evidence** — a run test, a driven route, a watched
  behavior — never "should work". **A user-triggered write path is verified through the real route**,
  end to end (request → guard → persistence → read-back), never only isolated units.
- **A `[discovered]` clause closes only on observation**: someone watched it run. Mark it in
  `PRODUCT.md` (`✓ YYYY-MM-DD: {one-line observation}`) only when a try-it confirmed it — never
  inferred from a green suite. Unlooked-at = not yet met, and saying so is the honest close.
- **A `question` batch closes on the measured answer** — `yes` / `no` / `inconclusive` are all
  honest, green closes recorded as an `F-NN`; a negative result is the finding, not a failure. What
  *would* be a cut is faking a target or trimming the experiment. The experiment harness itself is
  real code under the green suite.
- **The close certifies the Try list**: the user walks the batch's load-bearing flows by name
  (`WORK.md ## Try`) and gives a live verdict. The verdict is acted on, not archived.
- **Self-accounting, spoken**: before any close, explicitly list everything simplified, deferred,
  defaulted, or shortcut — and give each its honest home (a row, a `B-NN`, an `A-NN`) or justify it.
  Nothing closes with uncounted cuts. This is the primary cut-defense; run it honestly.

## Safety (unconditional)

- **You only own what this session changed.** Record pre-existing dirty paths up front (WORK
  Provenance); they are the user's — never claimed, never reverted, never committed.
- **Never the blunt instruments**: no `git reset --hard`, `git clean`, `git checkout -- .`,
  `git stash`, or wholesale reverts. Undo precisely, file by file, only what you created.
- **Name the rollback and stop for a yes** before any irreversible or outward action — delete,
  overwrite, drop or recreate a *populated* store, deploy, send, push. At a batch close, **enumerate
  every outward action upfront and take one explicit go** for the set.
- **Deploys follow the WORK header's `deploy:` policy** (`ask | user-runs | auto`, set once at
  `/gg:new`): `ask` proposes each deploy, named with its rollback, and waits for the yes;
  `user-runs` composes the exact command for the user to run themselves (`!` in the prompt) and
  reads the output back — the lane for a harness that blocks agent-run deploys; `auto` is a standing
  yes for the project's named deploy command at show/close points — still announced, never silent.
  Every other outward or irreversible action stops for its own yes.
- **Restore known-good state before stacking a fix** — when *your* change regresses behavior, revert
  that step first; never build a fix on a broken base.
- **Secrets stay out of `.gg/`** (it's committed): names of env vars, never values.
- **Write `.gg/` content in English** (verbatim user quotes stay in their language, marked as quotes).

## Commits (the record of record)

The WORK header carries `commit: ask | auto | never`, chosen once at `/gg:new` (default `ask`).
Commit points: the close of `/gg:new` and of each `/gg:plan`, the end of each `/gg:go` session, the
batch close, and each `/gg:fix`. Message shapes in `FORMATS.md`. Rules:

- **Pathspec-scoped, always**: `git add .gg/ {owned paths}` — the user's pre-existing dirty paths are
  never swept in, and never stage a file whose diff contains changes this session didn't make.
- `ask` → propose the commit at the commit point, one yes/no. `auto` → make it. `never` → don't;
  `WORK.md ## Last closes` is then the only history.
- **Deletion-based sweeps assume git holds the blocks.** Before deleting a record block that was
  never committed (policy `never`, or every `ask` declined), say so and get a yes — or leave the
  blocks and only mark the close. Silent permanent deletion is never gg's call.
- **Never push. Never any outward action on gg's initiative.** Skip committing (and say so) if the
  repo is mid-rebase/merge or otherwise not yours to commit.

## The record register

`.gg/` files are **records, not prose**: present-tense current truth, terse and factual. **No
narrative history** — how a fact came to be, what it was before, which batch changed it, the story
of its discovery: that trail is git's and the ADR's, never the record's. Write the observable fact,
not the aphorism about it. **One fact, one home**: a datum lives once; every other mention is its
bare id (`B-12`, `A-07`, `ADR-0004`) — a fact kept in two homes is the fact that goes stale in one.
`FORMATS.md` gives each file a hard bound, checked at exactly two points: the batch close prunes
accretion back to current truth wherever it grew (to ~¾ of the bound — `FORMATS.md`), and the
record pass (`/gg:where --audit`) takes inherited overage — deletion, never an archive; no session
polices bounds mid-row. **The record is gg's, not the product's**: a product
test that reads `.gg/` files couples the suite to the record's diet — surface it the moment it's
found; a record prune may legitimately turn such a test red, and the honest fix is decoupling the
test, never fattening the record.

## Context discipline

A long session degrades. The levers: `/gg:plan` sizes rows so each fits a session; `/gg:go` does one
M/L row per fresh session and chains S rows only under its hard stop conditions (`go.md`); WORK is
checkpointed after **every** row so `/clear` + `/gg:go` always resumes cleanly — `WORK.md` *is* the
handoff. The `.gg/` files are bounded by design (`FORMATS.md` bounds): read whole the ones the
command names; read `DESIGN.md` and `adr/` by section/slug as the task needs. **Re-read the exact
block from disk immediately before editing it** — never edit from memory — and **never bulk-edit
`.gg/` with regex/sed**: anchored, block-scoped replacements only. **Formats are closed**: write
exactly the sections and fields `FORMATS.md` defines, never invented ones.

## Output discipline

- **Act, don't narrate.** Batch tool calls; report at natural checkpoints; open with the result.
- **Never restate the user's answer back in bold before moving on.** Decisions land in files; the
  chat shows ids and one-liners.
- **Name first, id after** in everything the user reads: *"the export button (B-46)"*, never a wall
  of bare ids — the id is for the record; the name is what a human scans.
- **No empty-bucket reports** ("0 later · 0 discarded"), no recaps that duplicate what WORK already
  says, no ritual disclosures nobody asked for.
- **The close summary is the Try list, presented for the user's walk and verdict** — what to run
  and what to look at. Nothing else.
- **End every working session with a one-line breadcrumb**: where you are + what's next + which
  command continues. That single line is the whole ritual.
