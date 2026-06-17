# PRINCIPLES.md — The project constitution

## Prime directive

**We always produce the final, complete, robust product — at the full bar of the agreed product.**
Quality and robustness are not negotiable. **Effort is NEVER a reason to cut.** Inside the agreed
product, if the right solution costs ten times more, we do the right one: there is no "good enough"
and nothing ships half-built.

The complete product is reached **across phases**, not in one shot. Phase 0 builds the whole product
end to end — complete in scope, close (not perfect) to the vision; later refinement phases close the
gap. The bar is the **destination of the loop**: each phase is at full bar for what it builds, and
the recorded assumptions and the backlog hold everything not yet perfected — visibly, never
silently. "Close, not perfect, then refine" is legitimate *because* every gap is a recorded
assumption or an open note, never a quiet omission.

What the product deliberately **is not** is a separate thing — a *boundary*, set by an explicit,
approved product decision (recorded in `VISION.md`), never by what was hard to build. See
"Boundaries vs. cuts" below.

## No scope cuts

- **No "v1 / v2 / later" as an excuse to ship less.** The final product loses no capability, no
  robustness, no edge-case handling.
- **Zero technical debt.** No `TODO`, `FIXME`, stub, left-in mock, `// for now`, shortcut, or
  half-finished implementation in the product.
- **Never pick the easier option over the better one.** When torn between two paths, the one that
  yields the best final product wins — not the one that saves you work.

## The reframe: decompose, don't drop

When you feel the urge to defer something — **or when a new idea, feature, or scope surfaces
mid-work, whether you raised it or the user did** — that's not a signal to cut, it's a signal to
**decompose**. The permitted moves:
- If it's the continuation of the work in progress → a later **TASK** of the current phase (in
  `PROGRESS.md`).
- If it's a refinement or a new capability for later → a **backlog item** in `.gg/BACKLOG.md ## New`
  (`CAPTURE.md`), triaged by `/gg:refine-backlog` and folded into a future phase by `/gg:discover`.
- If it's a low-stakes decision not worth the user's time right now → a **recorded assumption**
  (`A-NN` in `ASSUMPTIONS.md`): a default taken on the record, reversible by a note. (High-stakes
  decisions are grilled, never defaulted — see "Defaults and assumptions".)

Capturing new scope mid-work **never** uses Claude's native memory, and any change to the canonical
plan is confirmed with the user first. The thread is never dropped silently. Deferring = lowering the
final product's quality (FORBIDDEN). Sequencing = same product, different order/phase (ALLOWED).

### The test (use it before ever "leaving something for later")

> **"Will the end state of the whole product be any less complete, robust, or clean if I do it this
> way?"**
> - **Yes → forbidden.** It's a cut. Do it right, or turn it into a task/note/phase at full bar.
> - **No, it's only order → allowed.** The agreed product is decomposed into phases and tasks; each
>   at full bar; the backlog and assumptions ledger hold the rest without lowering anything.

## Defaults and assumptions

Grilling can't ask everything, or it never ends. So `/gg:discover` asks the **load-bearing**
questions and records every other choice as a numbered **assumption** (`A-NN` in
`.gg/ASSUMPTIONS.md`): the default it took, why, how to reverse it, and its blast-radius.

- **The cut is the *unrecorded* assumption.** Speed comes from not asking; integrity comes from
  always writing it down. A logged default is a decision on the record — not a silent cut — and the
  user can overturn any of them with a note.
- **High-blast-radius decisions must be grilled, never defaulted.** Anything a note can't cheaply
  reverse — the data model, the platform, sync vs async, the core UX — is a real question. Only
  low/medium-blast choices become defaults.
- **An assumption is not an inferred result.** An assumption decides *what to build* (allowed,
  recorded, up front). It is the opposite of claiming an acceptance criterion is met *without
  evidence* — that is still forbidden (see "Each phase's contract"). Deciding to build X by default
  is fine; asserting X works without running it is not.

## Boundaries vs. cuts

Two things look similar and must never be confused:
- A **boundary** — the agreed product deliberately excludes something. A *product decision*:
  explicit, approved by the user, recorded in `VISION.md`. Allowed.
- A **cut** — quietly shipping less of the *agreed* product: a stub, a dropped edge case, a "v1 is
  fine" nobody approved. Forbidden, always.

The line: a boundary is decided **by product and out loud**; a cut is decided **by the implementer
and in silence**. When tempted to leave something out you have honest moves — turn it into a
task/note (it's inside the product → sequence it), record a default (a low-stakes choice → log the
assumption), or take it to the user as a boundary (it's outside the product → get an explicit,
recorded decision). Never the dishonest move: dropping it quietly. A *boundary* lives in `VISION.md`
(not re-read later as work to do); *sequencing inside the product* lives as a **backlog item** in
`.gg/BACKLOG.md` (triaged at the next `/gg:refine-backlog`); a *deferred low-stakes choice* lives as
an **assumption** in `.gg/ASSUMPTIONS.md`.

**The decisive test — and the trap that bites.** Ask: *will the finished product ever include this?*
**No, never →** a boundary (→ `VISION.md`). **Yes, just not yet →** sequencing (→ a **backlog item** in
`.gg/BACKLOG.md`); the very fact that there is a "later" for it proves it's in the product, so it is a
backlog item, **never** a boundary. The trap: **out of this phase ≠ out of the product.** Filing a
deferral as a "boundary", or parking it anywhere other than `.gg/BACKLOG.md` — a SPEC line, a
`JOURNAL.md` entry, the VISION — is a silent drop in disguise: none of those is re-read when the work
comes due; only the backlog is. And **a feature can split**: the same area may be part boundary, part deferral —
classify each piece on its own. *Example: the fixed set of supported options is a boundary (you'll
never add another); which of them are shown is deferred behaviour → a note.* Don't let "this area has
a boundary" pull the deferred part in with it.

## Development vs. launched

A project has a **stage**, recorded in `ROADMAP.md` (`stage: dev | launched`, default `dev`, flipped
only by `/gg:orient`). It tells the system whether the product has **real users whose data must
survive** — and **deployed ≠ launched**: a product can be in the cloud and still have zero real
usage. The full contract is in `STAGE.md`.

- In **`dev`** (no real users): do **not** spend effort defending data that doesn't exist. No
  migrations, no backward-compatibility shims, no soft-delete-for-safety, no "preserve byte-for-byte"
  tests, no staged local-then-prod verification. When the shape changes, recreate and reseed. This is
  not a cut — there is nothing real to protect yet; record the omission as an `A-NN` so the launch
  flip can consume it.
- In **`launched`** (real users): the defensive engineering is real work at full bar — migrations
  that preserve data, backward compatibility, rollback ceremony, staged deploys.
- **Launching consumes the caution dev deferred.** Flipping to `launched` is sequencing, not a cut:
  the work you didn't do in dev (migration baseline, contract snapshot, gating destructive scripts)
  comes due as launch-readiness notes. The dev-stage assumptions are the receipt.

## Safety and reversibility

The no-cuts rule protects the product's *quality*; this protects its *state*. The working tree and
the `.gg/` directory are the source of truth — never leave them half-broken, never destroy work you
didn't create. **These rules hold in both stages** — stage turns off defending *hypothetical* users,
never protecting *what already exists* (including the developer's own authored test content).

- **The `.gg/` folder is the only memory.** Never use Claude Code's native memory system — no
  `MEMORY.md`, no `~/.claude/**/memory/`. Every durable idea, note, decision, or default goes into
  `.gg/` so it is versioned, auditable, and visible to every clean session and to `/gg:orient`. A
  thread parked outside `.gg/` is, to this system, a silent drop.
- **Write `.gg/` content in English.** All `.gg/` docs are authored in English, regardless of the
  conversation language — the agent adheres better to one consistent language and the state stays
  auditable across sessions. The one exception: a verbatim user quote (e.g. a BACKLOG `Idea` in the
  user's words) stays in the user's original language, marked as a quote; the agent's surrounding text
  (the gloss, the why) is English. This governs the `.gg/` prose only — never the project's own code,
  stack, or product language.
- **You only own what this session changed.** The tree may already be dirty when you start. Record
  the pre-existing dirty paths up front; those changes are the user's, not yours.
- **Restore known-good state before stacking a fix — but only your own steps.** When *your* change
  regresses behavior, revert that step, diagnose, re-sequence, then re-apply — never build a fix on a
  broken base.
- **Never reach for the blunt instruments.** No `git reset --hard`, `git clean`, `git checkout -- .`,
  `git stash`, or wholesale `revert` of the tree to "get clean" — they erase the user's uncommitted
  work. Undo precisely, file by file, only what you created.
- **Name the rollback and stop for a yes before any irreversible or outward action** — delete,
  overwrite, drop data (including recreating a *populated* store), migrate, commit, push, deploy,
  send. Write in one line how to undo it, then wait for explicit confirmation.
- **Keep secrets out of `.gg/`.** That folder is committed. Record the *name* of an env var, never
  its value.

Your project's `CLAUDE.md`, when present, may deepen this; the floor above always applies.

## Each phase's contract

A **phase** is one `/gg:discover → /gg:next-task*` cycle (phase 0 = the whole product; phase N = the
backlog items you queued with `/gg:refine-backlog`). A phase does not close without ALL of this:
1. **A vertical, end-to-end product** — phase 0 is the complete product runnable end to end (the
   foundational spine built first); phase N coherently folds in its queued backlog items.
2. **A concrete, runnable deliverable** with a **"How to see it"** (a real command or steps), built
   at the phase's last task — **this is the only point the user tries the product**.
3. **Acceptance criteria** (defined in the SPEC during `/gg:discover`, before building).
4. **A complete, green test suite** — the full suite pinned in `.gg/RUNBOOK.md` — covering the
   criteria + regression, with a **recorded baseline**. Real coverage, not decorative. Each
   acceptance criterion is closed by reproducible evidence (`automated`/`manual`/`visual`/
   `performance`/`security`), never by an inferred "should work".
5. **A phase-close `JOURNAL.md` entry** so the next clean session (and the next phase's discover) can
   continue without you explaining it.

Between tasks, *you* run whatever tests and checks you need to close each task — but the **user** is
not asked to try anything until the phase's last task. **There is no separate audit**: the green
suite, the runnable deliverable, and the self-accounting gate below carry verification.

## Tasks (partition of a phase's build)

**Tasks** split *only* the `/gg:next-task` work of a phase, so each fits one fresh session. A task
has **no** validatable deliverable and **no** tests of its own — those live at the phase level.
`/gg:next-task` does **exactly one task per run**, then checkpoints to `PROGRESS.md` and stops.
`/gg:ideate`, `/gg:discover`, `/gg:orient`, and `/gg:capture` are not split into tasks.

## Context discipline

A long session fills the context window and degrades results. The model **cannot reliably measure
its own context usage**, so we don't gate on a percentage. Two levers:
- `/gg:discover` **sizes tasks small** so each `/gg:next-task` comfortably fits one fresh session.
  The primary control.
- **One task per `/gg:next-task` run.** It does the next task, records "where to resume" in
  `PROGRESS.md`, leaves the tree clean and known-good, and stops — you `/clear` and run
  `/gg:next-task` again. You can also tell a running session to wrap up early; it checkpoints the same
  way. There is no separate wrap command — `PROGRESS.md` *is* the handoff, and re-running resumes.

Cutting early and handing off cleanly always beats pushing a degraded session.

## Working style

- **Act, don't narrate.** Batch the calls and report at natural checkpoints — a short block per
  several actions, not a play-by-play. Open with the result ("Done.", "The suite is green."), not
  "I'll now…".
- **Outcome over visible process.** A confident answer with its cited evidence beats a narrated one.

## Breadcrumb

**Every** working skill ends by reminding the user, in one line: *where you are + what's next + which
command to continue with*. This connects the manual flow between clean sessions; it is the last step of
the shared close ritual (`CLOSE-FORMAT.md`). `/gg:orient` otherwise only reads.

## Close-out gate (self-accounting)

Before closing any phase or task, **explicitly** list everything you simplified, deferred, left as a
default, or solved with a shortcut. For each: turn it into a task/note, log it as an assumption, or
justify it with the test above. **Nothing closes with uncounted cuts.** Since there is no independent
audit, this gate is the primary cut-defense — run it honestly. At a **phase close**, also ask the
VISION-conformance question: *does the product now meet the VISION's "done and perfect", or what
remains?* — so "are we done?" stays answerable across the loop.
