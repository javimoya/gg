# Changelog

All notable changes to `gg` are recorded here. Versions follow the `version` field in
`.claude-plugin/plugin.json`.

## 4.0.1

**Questions batch, answers route** (`gg-shared/DELEGATION.md` → While agents run, new bullet;
`ROW-BRIEF.md` report line pluralized). 4.0.0 wrote the question relay in the singular — "its
question" — leaving unstated the two shapes real batches produce: one agent returning **several
questions in one stop**, and **several agents waiting at once**. Both were proved end to end on
2026-08-19 with live relay tests before writing the rule: a single agent stopped once with three
questions, got the user's three answers in one send, and applied all three (two of them against
its own recommendations — it could not have been right by default); and two agents with disjoint
option sets were answered from one card and each resumed with its own answer, zero crossing. Each
test carried a cryptographic commitment (SHA-256 of a secret held only in the agent's context,
hashed before the user answered, revealed after) proving the resume reaches the **same** agent
context, not a fresh launch. The rule the tests earned: relay the waiting questions as **one**
card — each named by its row, with its recommendation, never a ping per agent — then route each
answer set back to the agent that asked it, one send per agent carrying that agent's whole set;
answer sets never mix, and a batch card never blocks the lanes that aren't waiting. ROW-BRIEF now
tells the agent the same from its side: several questions in one stop beat several stops. Not
format-changing (no `.gg/` shape touched; no CONVERSION.md section, no index row).

## 4.0.0

**Delegated execution** — the first new execution shape since v3 defined the row. A batch may now
be built by **row agents in parallel git worktrees, orchestrated by the `/gg:go` session**, instead
of one row per session. Measured from the flights session of 2026-08-19, where one user ask
("¿puedes lanzar en un subagente la row mientras tú avanzas?") turned a go session into an
improvised orchestrator — four rows built in isolated worktrees while it merged, ran the suite on
the fused tree, and kept the record — and surfaced every mechanic and trap this release makes law:
the semantic conflict only the merged suite can see (two rows green in isolation, red together),
the worktree branched from a stale HEAD, the agent decision reported veto-style (A-383), the show
row colliding with rows already built behind it.

- **plan decides the shape, veto-style** (plan.md §3): sequential-vs-parallel is not a binary but a
  dependency graph, and "sequential" is its degenerate chain. When ≥2 rows are genuinely
  independent and the preconditions hold (git, `commit: ask | auto`), plan slices rows by disjoint
  surfaces, writes `exec: delegated`, fills the `after` (dependencies; a show is a barrier) and
  `by` (agent / agent:{model} / session) columns, and reports the read at the same single gate.
  Agents inherit the session's model; a cheaper model is proposed only for mechanical S rows. A
  batch with no real parallelism stays `exec: solo` — the lane go runs unchanged.
- **the go session is the orchestrator** (go.md §0 + `gg-shared/DELEGATION.md`, new) — never a
  special row 1: a row is product work with an observable done-when, and orchestration has none.
  The session launches ready rows (each worktree branched from the integrated HEAD at launch),
  builds the `by: session` rows itself, and checkpoints WORK after every integration, so `/clear`
  + `/gg:go` resumes orchestration exactly as it resumes a row. Integration is the row's real
  gate: merge, **merged-tree full suite green before the next merge lands**, diff review against
  the bar, records, cleanup. The Delegations line in "Where to resume" is the in-flight and crash
  ledger.
- **questions still reach the user** (`gg-shared/ROW-BRIEF.md`, new — the ~2KB contract a row
  agent reads instead of METHOD/FORMATS/orient): the ask-vs-assume split one level down. Research
  self-answered; low-blast decided and returned as assumption candidates (reported veto-style at
  integration); judgement, taste, product intent, the brief's open list, and anything
  irreversible/outward **stop the agent** — the orchestrator relays the question to the user with
  the agent's recommendation and sends the answer back to the same agent, context intact.
- **the record has one writer**: row agents never read or write `.gg/` — candidates come back in
  a structured report; the orchestrator mints every id (one `next-id:` counter, no races).
- **cost goes to whoever uses it**: DELEGATION.md is read only by orchestrating sessions,
  ROW-BRIEF.md only by row agents — a delegated row skips the whole-file orient entirely (~190KB
  in an undieted project, measured in flights); the solo lane pays one header line and nothing
  else. METHOD's Context discipline gains one parenthetical.

**Format-changing**: the WORK header gains `exec: {solo | delegated}` (per batch at open — `/gg:new`
writes `solo` for batch 0); delegated boards gain the `after`/`by` columns and the Delegations
line. CONVERSION.md gains the **3.x → 4.0.0** section (one header-line insert) and its index row;
the v2 → v3 and 3.0 → 3.1 procedures now land `exec: solo` too. `/gg:where --audit` checks the
delegated preconditions and ledger.

## 3.9.4

**The prevention gets the same level** (`gg-shared/METHOD.md` → Grilling, "Explain before you
ask"). 3.9.3 named ELI18 in the recovery lane only, so gg aimed at a level *after* the user had
already typed "no entiendo" — the first explanation, the one `/gg:new` and `/gg:plan` actually
put to the user, still had no stated altitude. The bullet now carries it, between the example
rule and the STE mechanics: pitch at a bright eighteen-year-old who follows any chain of
reasoning laid out for them and knows nothing of the field they were not told — no term taken as
given, and no toy analogy either. The bullet's closing sentence has the `what` skill inherit it
by name (`…with a new example, at the same ELI18 level`), so the level is defined once and the
recovery points at it rather than restating it. Not format-changing (no `.gg/` shape touched; no
CONVERSION.md section, no index row).

## 3.9.3

**The re-pitch names its level: ELI18** (`skills/what/SKILL.md`, "Simple to understand" →
"Simple to understand — the level is ELI18"). 3.9.1 settled that the re-pitch prescribes no
vocabulary, which left the *altitude* unstated: how much prior knowledge the second explanation
may assume. The section now opens with it — pitch at a bright eighteen-year-old, someone who
follows any chain of reasoning laid out for them and knows nothing of the field that was not
told to them — and names the failure on the other side: **ELI5**, where a toy analogy stands in
for the real thing, the user still cannot make the call, and has to ask a third time. Level, not
vocabulary: the 3.9.1 freedom (no word source prescribed, none forbidden) and the STE mechanics
stand unchanged beneath it. The `description` also gains the trigger "explain it like I'm five" —
the ask that most wants this skill, now answered at ELI18. Not format-changing (no `.gg/` shape
touched; no CONVERSION.md section, no index row).

## 3.9.2

**gg never gauges its own context** (`go.md` §4 + fold bullet, `plan.md` Close, METHOD → Context
discipline). The chain rule told go to keep building only while "the session is comfortably under
~half its context" — a measurement the model cannot make, so sessions invented one ("well over
half its context") to narrate their stop. The same tell lived in the fold bullet ("deep in a spent
session, say so") and in plan's sanctioned exception to the `/clear` ("if the session still has
plenty of context"). All three are gone, and **no self-assessment replaces them** — the rules are
now bare conduct: `/gg:go` builds one row, checkpoints, and stops (chaining removed entirely;
§4 is "Checkpoint and stop", commit `gg(b{N}): row {K}`); plan's Close always ends at the
breadcrumb; whether to `/clear` before the next run is the user's call, never gg's. METHOD's
Context discipline keeps its read/edit rules but drops the "a long session degrades" framing —
mechanics stated, rationale removed. README/CLAUDE.md updated to match. Not format-changing (no
`.gg/` shape touched; no CONVERSION.md section, no index row).

## 3.9.1

**The re-pitch prescribes no vocabulary** (`skills/what/SKILL.md`, "The register" → "Simple to
understand") — 3.9.0 swapped `wait-what`'s `CONTEXT.md` words for "the user's own words"; the
user vetoed both the same day: he is sparing with words, and a re-pitch bound to any word
source is a constraint on the wrong axis, whichever source it names. The section now states the
one measure — easy to follow — and grants the freedom: no word source prescribed, none
forbidden, new vocabulary welcome when it makes the thing easier to understand (a term worth
introducing gets its plain meaning in one clause, the first time it appears). The STE mechanics
stay — short sentences, one idea per sentence, active voice, everyday words — because they are
what "simple to understand" means, not a vocabulary rule. Not format-changing (no `.gg/` shape
touched; no CONVERSION.md section, no index row).

## 3.9.0

**The `what` skill** (`skills/what/SKILL.md`) — the recovery lane for the message that did not
land. 3.8.0 taught the commands to explain before they ask (METHOD → Grilling); this is the
user's side of the same coin: one standalone skill, fired by `/gg:what` or by the words
themselves ("no entiendo", "explícamelo de manera sencilla, con ejemplos fáciles de seguir"),
that re-pitches the last thing put to the user — most often a question with options they cannot
tell apart. Borrowed from mattpocock/skills' `wait-what`, keeping its insight — name the
listener's state, not the wanted output; "be brief" over-corrects into blunter, not clearer —
and dropping its `CONTEXT.md` lean: a record that grew without the user can be exactly the
vocabulary they do not understand. The re-pitch: **back up, don't trim** (what we are deciding +
what hangs on it, before any detail); **one easy example per option** — the project's real
objects, example first, rule after; the STE register in the conversation's language and the
user's own words, no new vocabulary; for a question, the same option set 1:1 with the
recommendation restated and the question asked again — confusion is never license to decide for
the user. A second fire on the
same message changes the example and backs up further, never the same words louder. METHOD →
Grilling's "Explain before you ask" now names the skill as its recovery. Works in any project,
gg or not. Not format-changing (no `.gg/` shape touched; no CONVERSION.md section, no index
row).

## 3.8.0

**Explain before you ask** (METHOD → Grilling, rewriting "Ask in the concrete") — measured from
the klasse + flights session transcripts (2026-07-19 → 2026-08-04): ~15 times in under three
weeks the user had to answer a grilling question, a recommended batch, or a triage entry with
"no te entiendo / explícamelo de manera sencilla, con ejemplos fáciles de seguir" before they
could decide. The explanation the user has to ask for is the one that should have come first.
Now anything put to the user for a decision opens with its plain explanation: a little context
first (what this decides, what hangs on it), one easy-to-follow example per option, written in
Simplified Technical English (ASD-STE100: short sentences, one idea per sentence, active voice,
everyday words — the style holds in whatever language the conversation runs in) and in the
project's own terms (`CONTEXT.md`'s ubiquitous language, never its avoided synonyms). plan's
advisory entry now cites the standard. Not format-changing (no `.gg/` shape touched; no
CONVERSION.md section, no index row).

## 3.7.0

**Veto, not go-ahead** (METHOD.md, new section) — where gg would ask the user to choose between
dispositions it already has a read on (triaging a capture, opening a board it just designed), it
now takes its read, executes it, and reports it in past tense with its why and its way back. The
user's move is the veto, not the go-ahead: silence leaves the decision standing and the next
command runs with no round trip — a question whose answer you were going to recommend anyway is
ceremony charged to the user. Two kinds of decision are never taken this way: irreversible/outward
actions (Safety) still stop for their yes, and judgement/taste/product intent (Grilling) is still
asked — the disposition is yours; the design is theirs. The honesty rule: acting replaces the
question, never the telling — an unreported decision is a silent drop, whichever way it went. Not
format-changing (no `.gg/` shape touched; no CONVERSION.md section, no index row). The three
frictions it removes:

- **go's triage card reports, it no longer asks** (METHOD → Capture, go §3/§4): captures jotted
  while the user was away arrive triaged — an S fold already folded (§3a), a backlog resting in
  `## New` — each with its why and way back. An M/L fold (its grilling is the user's) and a
  discard (a deletion) are the two reads the card can only offer.
- **plan's gate opens first** (plan §4): block moves, deferrals, and `state: building` happen
  before the report — all reversible moves — and the report ends in an open veto window (*"add,
  cut, or change anything — or `/clear` + `/gg:go`"*) instead of waiting for a "go". A discard
  still deletes only on its yes; a vetoed item's block moves back.
- **new's sign-off opens batch 0** (new §4): `state: building` is set before the consolidated
  summary; a veto is applied in one pass, silence leaves the batch standing and `/clear` +
  `/gg:go` builds it. TUTORIAL §5 rewritten to match ("Pass the gate" → "Read the sign-off").

## 3.6.0

**The `diataxis` skill** (`skills/diataxis/`) — user-facing documentation written and audited
with the Diátaxis framework (diataxis.fr). Not a command: a complementary skill, auto-triggered
by description or invoked ad-hoc, that go/new lean on when a row's deliverable is documentation.
Not format-changing (no `.gg/` shape touched — the skill explicitly excludes record files; no
CONVERSION.md section, no index row).

- **The compass first**: two questions (action or cognition? study or work?) classify every doc
  into tutorial / how-to / reference / explanation before a word is written; the form is declared
  so the user can veto it; genuinely ambiguous audience → the compass questions go to the user.
- **One reference file per form** (`references/`): obligations, do/don't, sentence patterns
  (EN/ES), title conventions, shipping checklist — loaded only for the form being written.
- **Audit mode** (`references/audit.md`): map and classify existing docs, findings with severity,
  then fixes one at a time, each self-contained and shippable — never big-bang.
- **Cross-cutting rules**: one form per document, link don't absorb; never scaffold empty
  structures; README stance by project size; language follows the project (default English).
- **Nudges**: go §3 — a row delivering user-facing docs is built with the skill; new §3 — such a
  row names its Diátaxis form in the done-when.
- **`TUTORIAL.md` — Your first project with gg**: the repo's missing tutorial quadrant, found by
  the skill's own audit of the README — builds a toy dice-roller end to end (kickoff → board →
  build loop → Try walk), written under the skill's tutorial rules.
- **README re-formed by that same audit**: "How it works" command detail deduplicated to
  one-liners linking the specs (drift risk removed), FAQ commit answer deduplicated, "How to
  manage the plugin" retitled to the how-to convention, install success signal added, tutorial
  linked from Quick start.

**Seven borrows from the deep read of the rest of mattpocock/skills** (diagnosing-bugs, tdd,
codebase-design, code-review's Spec axis, triage's AGENT-BRIEF, its OUT-OF-SCOPE companion,
research) — each a surgical fold into existing files; no new commands, no new state files, not
format-changing (no CONVERSION.md section, no index row). Considered and declined at the gate:
design-it-twice and the variant show (the user's call), plus the tracker substrate, throwaway
prototypes, batch grilling, handoff documents, and the git-guardrails hook (Safety stays prose
until observed broken).

- **The red loop** (fix §1, echoed in go §3): for a non-obvious defect, one command that already
  goes red on the bug comes before any theory; minimise the repro until every element is
  load-bearing; hypotheses are falsifiable and shown ranked (the user's domain knowledge re-ranks
  them instantly); instrumentation is `[DEBUG-xxxx]`-tagged so cleanup is one grep. The red-loop
  command, gone green, usually *is* the pinning test fix already required.
- **The tautological-test rule** (METHOD → Evidence): a test's expected value comes from an
  independent source of truth, never recomputed the way the code computes it — a test that mirrors
  the implementation passes by construction and certifies nothing.
- **The anti-padding razor** (METHOD → The bar): the bar polices padding as well as cuts —
  abstractions no clause needs are not "the better option"; one adapter is a hypothetical seam,
  two are a real one; a structure whose deletion leaves the product just as complete was padding.
- **Symmetric self-accounting** (METHOD → Evidence, go §5): the close also names anything built
  that no row or clause asked for — keep-and-record, or remove; nothing closes with unasked-for
  additions.
- **Boundary match at capture** (METHOD → Capture): a capture that re-raises a PRODUCT "It is not"
  boundary is surfaced as such — *"still feel the same?"* — never silently backlogged.
- **Primary sources for research answers** (METHOD → Grilling): a research answer stands on the
  current docs, the code, observed behavior — never parametric memory — and names its source.
- **Backlog durability** (FORMATS → BACKLOG): a `B-NN` block describes behavior and contracts,
  never file paths or line numbers (they go stale while the item waits); file:line lives only in
  WORK's "Where to resume".

## 3.4.0

**Three borrows from wayfinder** (Matt Pocock's decision-mapping skill), each folded into gg's
existing ceremonies — no new commands, no new files, not format-changing:

- **Grilling questions split two ways** (METHOD.md → Grilling): a question research can answer
  (code, docs, an API's actual behavior) is never put to the user — answered directly or handed to
  a subagent while the grilling continues, its result feeding the next recommendation. Judgement,
  taste, and product intent stay the user's alone — and the agent never answers those for them.
- **The fog test** (plan §2): a question you can state precisely is ready to be a `question`
  batch — even one you can't answer today; one you can't yet phrase that sharply stays a
  `PRODUCT.md` Unknown, never pre-sliced into vague `B-NN`s.
- **Name first, id after** (METHOD.md → Output discipline): in everything the user reads, lead
  with the item's name — "the export button (B-46)", never a wall of bare ids.

Deliberately **not** borrowed: the shared decision map on an issue tracker (wayfinder's core) —
gg stays single-session, seven files, no tracker by design; `## Staged` + Unknowns + resumable
`shaping` cover the programme view.

## 3.3.0

**Records now carry their gg version, and the audit knows the way forward.** Until now a `.gg/`'s
vintage was guesswork (the CONVERSION.md notes tracked two known projects by hand); now the record
says it itself, and the conversion knowledge is indexed so any session can act on it.

- **The version stamp**: the WORK header gains `gg: {X.Y.Z}` — the plugin version whose `FORMATS.md`
  shapes the record follows. Written once at `/gg:new`; rewritten **only** by a conversion's closing
  step or by the record pass after verifying conformance — never bumped by a session merely running
  under a newer plugin.
- **CONVERSION.md gains the index**: which `.gg/` state needs which conversion, keyed on the stamp.
  Only format-changing releases get rows; every conversion procedure now ends by writing the new
  stamp. The old two-known-conversions framing becomes a growing, agent-executable manual.
- **`/gg:where --audit` checks the version first**: a missing stamp is drift (the record pass adds
  it once the shapes check out); a stamp behind a format-changing release is reported and the
  recorded conversion **offered once** — its own explicit yes, never bundled with the record pass,
  executed in-session per CONVERSION.md (which opens with a git snapshot). When a conversion
  applies it is the reconcile for shape drift wholesale — the audit doesn't itemize what the
  conversion rewrites anyway. No yes → nothing changes.
- **Releasing** (CLAUDE.md): a format-changing release must ship its CONVERSION.md section + index
  row; a commands-only release needs neither.

## 3.2.0

**Tuned from klasse b92–b93** (14 sessions analyzed): bounds were being policed at every row —
`wc -c` in every orient and checkpoint, WORK trimmed mid-row to 5–224 bytes under its 10KB line
and back over it one session later (one session spent 6 edits + 5 measures + a dedicated commit
+ 3 amends on the trim alone), and a prune planned as batch work (B-458 as rows 6–7, ~90 tool
calls) that still left files at or over the bound. Root causes: bounds below a mature project's
irreducible truth (after a maximal prune, CONTEXT/PRODUCT/RUNBOOK floors sat at 20–21KB), prunes
that land at the edge, and enforcement points the specs never named. Also measured: 35 of klasse's
42 live backlog items are agent-minted — captured with a bare-id offer nobody can triage cold,
then ridden to a plan weeks later.

- **Bounds re-tuned, with headroom**: WORK <16KB (was 10), every other whole-read file <32KB
  (was 20). A prune now cuts to **~¾ of the bound** (WORK ≤12KB, others ≤24KB), never to just
  under the line — a file trimmed to the edge is re-trimmed every session after.
- **Bounds are checked at exactly two points**: the batch close (accretion this batch left) and
  the record pass (`/gg:where --audit`, inherited overage). go's orient and checkpoints never
  measure or trim; plan never mints prune rows or `B-NN`s for inherited overage — it routes them
  to the record pass (B-458's anti-pattern, named).
- **Captures get a triage card**: agent-minted `B-NN`s are offered at the next checkpoint/show/
  close as ONE card for the session's captures — id + the Idea's plain opening sentence (the
  concrete example FORMATS already required, now actually shown) + weight guess + recommendation —
  with three dispositions: **fold** into the live batch · **backlog** · **discard** (a "never"
  becomes a PRODUCT boundary). The card never blocks (no answer → `## New`) and is offered once —
  `/gg:plan` is the next look.
- **Stale agent captures surface for discard**: at plan, an agent-minted `## New` item whose
  Captured date predates the last two closes is named for a keep-or-discard call at the gate;
  `--audit` reports the same as backlog debt. A `BACKLOG.md` over its bound reconciles by triage
  at the next plan (merge or discard items), never by rewording blocks to fit.
- CONVERSION.md targets the current bounds instead of pinning 3.1's numbers.

## 3.1.1

**Tuned from 3.1's first day in production** (klasse b87-b90 — including a natural experiment: two
batches ran old 3.0 specs against the dieted record and re-bloated it at the pre-diet rate, +11.6KB
vs +2.2KB/batch under real 3.1, confirming the discipline lives in the specs). Fixes for what the
first real 3.1 batches exposed:

- **Caps bind where the text is born**: plan measures done-whens as written (an over-cap cell is
  rewritten before the gate, never left for the audit); the close writes its Last-closes entry
  measured (≤200 chars). The caps were being enforced by the audit and breached at write time.
- **The record pass**: `/gg:where --audit` may now, on ONE explicit yes after its report, apply the
  reconciliations it just reported — the sanctioned lane for inherited over-bound files that
  batch-scoped close pruning structurally can't reach (B-458's gap; klasse's `gg(record)` commit was
  already doing this off-spec). Per-file passes, never a joint sweep; each destructive cut still
  asks; commits as `gg(record): …`. The close now re-measures every whole-read file instead of
  trusting a standing list (PRODUCT crossed its bound unnoticed).
- **ADR bodies match their real job**: the decision in 1-3 sentences up top, and below it only
  measured evidence that defends the call — never narrative history. (Both post-diet ADRs were
  excellent five-section essays; the evidence deserved a home, the narrative didn't.)
- **Findings retire**: a NOTES finding with no live consumer (no open `Leads to`, no unconfirmed
  clause, no pending time gate) is consumed at the second close it survives — findings were NOTES'
  fastest growth with no judge.
- **The record row is bounded**: a batch may group its doc-sync as one S row for convenience, but it
  carries no `B-NN` and never certifies — a ✓ belongs to the close's walk (or a show's routed
  verdict). The pattern had begun pre-writing close certifications into rows.
- Smaller: the gate names its approving word ("…or say go") and reads a bare "ok"/"no" as assent
  when nothing was raised; the audit's description says out loud that it IS gg's audit (a user
  reached for `/gg:audit`); METHOD + CONVERSION warn that a product test reading `.gg/` couples the
  suite to the record's diet (klasse's suite went red on exactly this).

## 3.1.0

**Tuned from 17 days of 3.0 in production** — 280 klasse sessions (batches 44→86) analyzed end to
end: every user message, the `.gg/` growth curve per batch, and a staleness audit of the record.
What the evidence showed: the record was strikingly *accurate* (17 of 21 spot-checks exact) but had
drifted to ~3.9x its conversion size in 41 batches; a `/gg:go` orient had grown to ~65k tokens
(~⅓ load-bearing) against v3's ~4-6k target; 22 of 29 shows sat as the board's final row,
duplicating the close's Try walk; the same "mételo en esta fase" got three different ceremonies
depending on the day; and 29 of 34 live backlog items were agent-minted deferrals never offered
back. 3.1 fixes what the data convicted:

- **`/gg:go` folds** (new §3a): scope the user wants NOW enters the live batch in-session — mint
  the `B-NN`, weigh S/M/L, grill what the weight demands (full grilling for an L), append/insert
  rows, extend Try. No `/gg:plan`, no state flip, no gate beyond the user's ask. `/gg:plan`'s
  re-scope-in-place remains for the real redesigns; `/gg:fix`'s honesty valve now offers the fold.
- **A capture is offered, never buried**: mid-row discoveries still jot-and-return, but every
  capture is surfaced at the next checkpoint the user sees with a one-line now-vs-later offer.
  Agent-minted items open with one plain sentence anyone can triage cold.
- **A show is never the final row**: the close's Try walk is the batch's last look — a show row
  exists only mid-batch, where the look can still change the remaining rows, and it runs on the
  surface the user actually judges (their deployed product, not a local stand-in).
- **The record register + bounds with teeth** (METHOD/FORMATS): `.gg/` files are terse present-tense
  records — no narrative history (it's git's). WORK <10KB, other whole-read files <20KB; done-whens
  ≤500 chars; Last closes one line each; CONTEXT entries = definition + _Avoid_; RUNBOOK = commands
  + traps (completed one-time procedures deleted); ADRs 1-3 sentences, no amendment blocks; one ✓
  per PRODUCT clause. The close now prunes accretion in CONTEXT/RUNBOOK/PRODUCT too, and
  `/gg:where --audit` reports bound/register violations. `CONVERSION.md` gains the one-time
  3.0→3.1 prune for existing projects.
- **Deploy policy** (`deploy: ask | user-runs | auto`) in the WORK header, set once at `/gg:new` —
  ends the per-close permission renegotiation the sessions showed (`user-runs` composes the exact
  command for the user's own `!` prompt).
- **`Decided` items and `## Staged` groups** in BACKLOG: decisions grilled outside a plan are
  sealed — plan executes them and never re-opens them; pre-grouped future batches (a programme) are
  taken as-is instead of re-derived. Both were user-invented workarounds during the klasse genesis.
- **Advisory `/gg:plan`**: invoked without a chosen set it explains and recommends, touching no
  state until the user picks (an abandoned advisory plan used to leave `state: shaping` dirty).
- Smaller, all evidence-backed: `/gg:go` routes from `## State` + `## Where to resume` alone before
  paying the full orient (blocked/idle boards no longer cost a whole session); off-lane commits
  since the last close are surfaced with a record catch-up offer; an ambiguous S capture earns its
  one deciding question (the B-368 misbuild); small items sharing a surface group into one row;
  grill questions lead with the concrete example; a recommended option never proposes below the bar;
  never bulk-edit `.gg/` with regex/sed (two data-loss incidents, both saved by git).

## 3.0.0

**The lean rebuild — redesigned from measured evidence.** v3 comes from auditing v2 in production:
309 real sessions across two projects (klasse: 43 phases in 14 days; flights: 6 phases in 6 days),
where **45.5% / 41.4% of every character the agent wrote went to `.gg/` documentation instead of
code**, ~70-75% of the `.gg/` bytes were write-only history nobody (human or agent) ever re-read,
every live decision was recorded 4-7 times across files, a phase close updated 7-9 files, and the
express lane (`/gg:quick`) spent 9 ledger writes and three sessions on a 5-line fix without writing
it. The redesign keeps everything the evidence showed working — cold-start orientation, recorded
assumptions, green-suite gates, evidence-based closes, grilling for load-bearing decisions, stable
ids, the safety floor — and deletes the rest. A clean break: **no migration logic in commands**;
`CONVERSION.md` converts an existing v2 `.gg/` by hand (or by an AI session).

### The new shape
- **Five commands** (were seven): **`/gg:new`** (kickoff: vision + whole-product design + batch-0
  board, one resumable arc — merges `ideate` + phase-0 `discover`), **`/gg:plan`** (opens a batch in
  ONE ceremony with ONE consolidated veto gate — merges the capture burst + `refine-backlog` +
  `discover`), **`/gg:go`** (builds board rows; chains S rows in one session under hard stop
  conditions; closes the batch on evidence — replaces `next-task`), **`/gg:fix`** (fix now, record
  after — one line in the Fix log; replaces `quick`, which designed but never fixed), **`/gg:where`**
  (read-only GPS + `--audit`; replaces `orient`). Capture is an inline two-line protocol in every
  session, no longer a command.
- **Seven bounded state files** (were 13 + 2 archives): `WORK.md` (state + board + Try list +
  provenance + fix log — exactly one batch, reset at close), `BACKLOG.md` (New/Later, `next-id:`
  counter in the header), `PRODUCT.md` (the destination; `[discovered]` clauses carry ✓ marks when
  observed), `DESIGN.md` (current truth, edited in place), `NOTES.md` (open A-NNs + live F-NNs),
  `CONTEXT.md`, `RUNBOOK.md`, plus `adr/`.
- **History is git's job.** Deleted outright: `JOURNAL.md`, `ASSUMPTIONS-ARCHIVE.md`,
  `BACKLOG-ARCHIVE.md`, `ROADMAP.md` (state → WORK header; the structural changelog was self-described
  write-only), `SPEC.md` (acceptance = per-row "done when" lines + the test suite itself),
  `BLUEPRINT.md`'s append-only phase sections (DESIGN is edited in place), `FINDINGS.md` close-verdict
  receipts, and the per-project `PRINCIPLES.md` copy (the method is read from the plugin). Applied
  items and consumed assumptions are **deleted at close** — `git log -S "B-12"` recovers anything;
  `next-id:` counters keep ids stable without archives.
- **gg may commit** (the one constitutional break, opt-in): `commit: ask | auto | never` chosen once
  at `/gg:new` (default `ask`). Commits are the journal (`gg(bN): close — applied B-12 B-13 ·
  consumed A-31`), always pathspec-scoped to `.gg/` + owned paths, never a push.
- **Ceremony scales with item weight**: S (bug/tweak — zero questions, zero design prose, one board
  row) / M (1-3 questions) / L (full grilling + ADR), each weight shown at the gate with a one-line
  why so a silent cut can't hide in a misclassification.
- **Stage (`dev`/`launched`) removed** — unused in practice. gg assumes a product under active
  development (recreate & reseed; no migrations/backward-compat unless explicitly asked for); the
  safety floor (name the rollback + a yes before destroying existing data; never touch the user's
  dirty paths) is unconditional.
- **Output discipline** (METHOD.md): no bolded restatement of the user's answers, no empty-bucket
  reports, no recaps duplicating WORK; the close summary is the Try list; one-line breadcrumbs.
- Plugin docs shrink from 17 shared files / ~174KB of specs to **2 files (`METHOD.md` + `FORMATS.md`)
  + 5 commands, ~40KB total**.

### The numbers it targets (from the audit)
A 4-bugs + 1-feature batch: ~10 command invocations / 3 hard gates / 7-9-file close in v2 →
**3-4 sessions / 1 gate + the Try-list verdict / a 3-file close (4 at worst)** in v3. Orientation per build
session: ~17k tokens (disciplined) → **~4-6k**. Docs share of written output: **45% → ~15%
(projected)**.

## 2.9.0

**The flow, tuned by evidence** — from mining a real 15-phase project's 133 sessions: where the loop
leaked defects (a close try-it that never exercised the load-bearing routes), where it paid ceremony
nobody used (a show offered and vetoed 13 phases straight, multi-round default-flip sign-offs, a
"verdict owed" finding per close), and where real practice was already ahead of the spec (the phase
boundary chained in one session, `capture --next` assembling phases without triage).

### Added
- **The Try list — the close certifies named flows (`SPEC-FORMAT.md`).** The deliverable now carries
  the phase's load-bearing flows as a short transient **Try** list (each citing its `AC-N`/`B-NN`,
  rewritten whole at phase open like `## Shows`); the user's decisive try-it walks *that list* and the
  close's single `F-NN` cites it. A flow too risky for a visual pass — a user-triggered write, a heavy
  job — belongs on it by name. (`discover.md` §4 writes it; `next-task.md` §6 walks it.)
- **Route-level evidence for user-triggered writes (`SPEC-FORMAT.md`).** An `automated` criterion for
  a user-triggered write path drives the real route end-to-end — request → guard → persistence →
  read-back — never only isolated units. (The lesson of a guard that silently 400'd two answer types
  for ten phases while every unit stayed green.)
- **`/gg:quick` is the hotfix lane by name.** A failure of a just-shipped load-bearing flow (the Try
  list) after the verdict is captured as a `[bug]` and fast-tracked through `/gg:quick` — never an
  inline hot-fix outside a phase (no task, no test, no JOURNAL entry). `next-task.md` §0/§6 route it;
  the close breadcrumb names the branch.
- **A `## Later` freshness nudge (`refine-backlog.md` §1).** When `## Later` holds an item captured
  two or more shipped phases ago, the opening summary says so and recommends a `--later` review — the
  radar only works if it is looked at.

### Changed
- **A show is offered only when it applies (`discover.md` §4, `VISION-FORMAT.md`, `CONSTITUTION.md`).**
  "In play" is earned, not inherited: a `[discovered]` clause triggers the mandatory-first-show rule
  only while it is unconfirmed (no cited `F-NN`) or when the phase's queued set touches its area or
  adds a new one. A confirmed clause the phase doesn't touch → close-only, no show question at
  sign-off.
- **The sign-off is one pass, one question (`discover.md` §5, `GRILLING.md`).** One consolidated
  summary — kind · areas worth probing · high/medium defaults · show placement (when in play) — and a
  single veto question, free-form answer open; never successive rounds of "tick the defaults to flip".
- **One `F-NN` per close (`FINDINGS-FORMAT.md`, `next-task.md` §6).** A pending state is not a
  finding: "deploy green — verdict owed" never gets its own `F-NN`; the close records one finding —
  the verdict over the Try list — and the wait lives in the breadcrumb and PROGRESS "Where to resume".
  `/gg:orient --audit` flags pending-state findings.
- **The phase boundary may run in one session (`CONSTITUTION.md`, breadcrumbs).** After a close, the
  capture burst → `/gg:refine-backlog` → `/gg:discover` chain shares the try-it context that is its
  input — no `/clear` between them. `/clear` stays mandatory between build tasks and before the new
  phase's first `/gg:next-task`.
- **Two first-class lanes into `## Next phase` (`BACKLOG-FORMAT.md`).** Deliberate triage
  (`/gg:refine-backlog`) and the assembled next phase (`/gg:capture --next` jots, or one `/gg:quick`)
  — neither is a shortcut around the other.
- **The close takes one consolidated go (`next-task.md` §6).** Every outward action of the close —
  deploy, prod reads, regenerates — is enumerated upfront, each with its rollback, under a single
  explicit approval; a drip of mid-close confirmations is how an accidental "no" derails a close.

## 2.8.0

**The record that doesn't bloat** — informed by a real 15-phase project whose `.gg/` grew past what a
session can read: a half-megabyte PROGRESS, a 176KB assumptions ledger read whole at every command
start, and ~40K tokens of pure orientation per task. The record now stays lean by design, and commands
read it by anchor instead of by volume.

### Added
- **`gg-shared/LEDGERS.md` — a shared read/edit discipline for `.gg/`.** Commands read by **anchor**
  (a heading or an id like `### A-07 —` / `**AC-21**`), never a grown ledger whole — a whole-file read
  is never load-bearing (grown ledgers can exceed the Read tool's size limit outright). It carries the
  per-file read-depth table (ROADMAP → `## State` + `## Phase log` only; JOURNAL → the tail; SPEC /
  BLUEPRINT / ASSUMPTIONS / FINDINGS / ADRs → the current phase's anchored sections), the edit
  discipline (**re-read the exact block immediately before editing it**; anchor on short stable spans;
  on a failed match, re-read from disk — never retry from memory), **"Formats are closed"** (never
  invent a field, section, or archive area), and **"One fact, one home"** (cite ids, don't restate).
  `/gg:ideate`, `/gg:discover`, `/gg:next-task`, `/gg:refine-backlog`, `/gg:capture`, `/gg:quick`, and
  `/gg:orient` all load their startup context through it; the constitution's "Context discipline"
  names it as the third lever.
- **`ASSUMPTIONS-ARCHIVE.md` — the assumptions ledger stays lean (`ASSUMPTIONS-FORMAT.md`).** At every
  phase close, `/gg:next-task` §6 **sweeps `## Open`**: a default whose decision is baked into shipped,
  verified behavior moves whole to the archive's `## Consumed` (the test: *reversing it now would be a
  change to shipped behavior — a `B-NN` — not a costless re-decision*); an overridden default now moves
  to the archive's `## Overridden`. `## Open` holds only the defaults still in play — the set commands
  actually read. Ids are computed across both files; stage-deferrals stay Open until the launch flip.
  `## Open` is **appended newest last** — the same direction as every other gg ledger.
- **AC supersession (`SPEC-FORMAT.md`).** When a phase removes or replaces shipped behavior,
  `/gg:discover` §4 sweeps the affected criteria and **collapses each to a single line** —
  `**AC-48** (phase 5) — superseded (phase {N}): {what removed it}` — the id stays retired and
  scannable, the Given/When/Then goes. A criterion left describing removed behavior is a contradiction
  in the contract, not history.
- **New `/gg:orient --audit` checks**: PROGRESS drift (title ≠ header phase; a second board or any
  archive-like section), invented structure (a section/field/column no format defines), changelog
  drift (a line outside the closed set), zombie criteria, stale shows, an `## Open` default the close
  never swept, and verbosity drift (closed-task log past the cap, board cells holding prose, a
  BLUEPRINT phase section re-narrating items).

### Changed
- **Opening a phase replaces `PROGRESS.md` whole (`discover.md` §4, `PROGRESS-FORMAT.md`).** The fresh
  board is written over whatever is there; prior boards are never stacked under any heading (their
  outcome lives in the phase-close JOURNAL entries). The one exception is the in-place re-scope, which
  edits the existing board.
- **The ROADMAP `## Structural changelog` is a closed set (`ROADMAP-FORMAT.md`, `CLOSE-FORMAT.md`).**
  One dated line per structural event only — kickoff · a phase opened · an in-place re-scope · a stage
  flip. **A task close is never a changelog line**, and a phase ship updates its phase-log line
  instead. The changelog is write-only outside `--audit`.
- **`SPEC.md ## Shows` is transient — current phase only.** `/gg:discover` rewrites the section whole
  at phase open and removes it when the phase has no shows; a past show's record is its `F-NN` verdict.
- **Verbosity caps where "terse" didn't bite.** PROGRESS: closed-task log entries hard-capped at two
  lines; board cells are names, 80 characters at most, citing `B-NN`/`AC-N` instead of restating them.
  BLUEPRINT: a `## Phase N` section records the design delta only, capped at ~20 lines — never the
  phase's re-narrated story. JOURNAL: one-line bullets, ids over restatement.
- **`/gg:refine-backlog` no longer loads `PRINCIPLES.md`** — the constitutional rules triage needs are
  stated in its own spec.
- **The ADR filename is the index (`ADR-FORMAT.md`).** The slug says the decision
  (`0007-sqlite-over-postgres.md`), so a session picks relevant ADRs from `ls .gg/adr/` alone — no
  separate index file, which would only drift.

## 2.7.0

A new first-class path for the mid-phase pivot: when a `show` reveals the phase is aimed wrong and work
must change *before* continuing, `/gg:discover` re-scopes the **current** phase in place — one command,
no detour through `/gg:refine-backlog`.

### Added
- **`/gg:discover` re-scopes the current phase in place (`state: building`).** When a `show`'s look
  reveals the remaining plan must change, `/gg:discover` reshapes the phase under way instead of opening a
  new one: it reads the just-captured `## New` reactions + the triggering `F-NN`, **keeps the done tasks
  done** (Provenance / owned paths never rewritten), redesigns only the pending tasks and inserts the new
  ones, re-places the show, appends a dated BLUEPRINT `## Phase N — revised` section, and **promotes the
  folded `## New` items into `## Next phase`** (keeping each `B-NN`) so they archive on this phase's close
  like normally-queued items. It does **not** bump `phase` or change `state`. (`commands/discover.md` §0
  / §2 / §4 / Close.)
- **`/gg:next-task`'s `show` breadcrumb now branches deterministically.** A plain look routes to the next
  task as before; a look that means the remaining plan must change routes straight to **`/gg:discover`**
  to re-scope in place — explicitly **not** `/gg:refine-backlog` (those reactions belong to *this* phase,
  and `refine-backlog` can't queue into `## Next phase` mid-build). (`commands/next-task.md` §5.)
- **`/gg:orient` names the re-scope path** when a `building` project's "Where to resume" shows a
  plan-changing show reaction. (`commands/orient.md`.)

### Changed
- **Docs kept in sync with the new path** — `gg-shared/GRILLING.md` ("The queued set"),
  `gg-shared/BACKLOG-FORMAT.md` (`/gg:discover` is now a third direct writer of `## Next phase`, scoped to
  the re-scope), and the README loop. No migration or back-compat logic — clean going-forward behavior.

## 2.6.1

A whole-repo code-review pass — bug fixes and documentation sync. No change to the workflow's behavior.

### Fixed
- **The CI frontmatter check is now robust (`tests/check_commands.py`).** It split the block on the first
  `---` it found rather than on a whole delimiter line, so a `---` inside a frontmatter value could
  truncate the block and silently drop keys (`model`, `disable-model-invocation`) — the opposite of what
  its own comment promised. It now matches whole `---` delimiter lines, rejects a malformed `----` fence
  with a clear error, and also flags a command that declares an `argument-hint` it shouldn't (the check
  was one-directional before).
- **`/gg:quick`'s deferred-fold branch no longer dead-ends.** When the single express item folds into an
  already-deferred (`## Later` / `## Future`) backlog item, `## Next phase` stays empty; `/gg:quick` now
  stops and routes to `/gg:refine-backlog` instead of running `/gg:discover` on an empty queue (which
  would bounce and leave a misleading breadcrumb).
- **`CHANGELOG.md` now records `/gg:quick` and `/gg:capture --next`** under 2.5.0 — the seventh command's
  introduction had been missing from the history.
- **The plugin and marketplace descriptions name all seven commands** — `/gg:orient` had been omitted
  from both.
- **Cross-reference fixes.** `/gg:orient`'s citation of the BLUEPRINT "Link, don't duplicate" rule is
  capitalized to match its heading, and `/gg:next-task` hedges the lazily-created `.gg/FINDINGS.md` with
  "(if present)".

## 2.6.0

**Research phases and an empirically-correctable target** — for a project whose specification is the
*output* of the work rather than its input: where what to build next emerges from the last result, and
the destination itself can be corrected by evidence, all without giving up the no-cuts / zero-tech-debt
bar.

### Added
- **`kind: build | research` — a per-phase axis (`ROADMAP-FORMAT.md`).** A phase is `build` (capability
  to a known spec) or `research` (*a search*: a question/hypothesis → an experiment → an observed result
  → the next step decided from it). `/gg:discover` sets `kind` from the queued set — a `[exp]`
  experiments / open-question set opens a `research` phase — surfaced for the user's veto at sign-off. A
  research phase uses the same `scoping → building → shipped` states (no new state, no new command); phase
  0 is always `build`, laying the foundation a later search runs on.
- **`reported` — a SPEC evidence type for an open empirical question (`SPEC-FORMAT.md`).** A `reported`
  `AC-N` is an open question whose answer isn't known at spec time, **closed by a cited `F-NN`** —
  `yes` / `no` / `inconclusive` are all honest, green closes; a negative result is the finding, not a
  failure. Four guardrails keep it from becoming a cut: a measure-question never a disguised capability,
  real reproduced evidence (never "should work"), `research` phases only, and the answer is the
  deliverable rather than a pre-written target.
- **`R-NN` vision revisions — the destination corrected from evidence (`VISION-FORMAT.md`).** A finding
  that contradicts a "done and perfect" clause flows through the single `B-NN` intake and, when applied by
  `/gg:discover`, **edits the clause in place and logs an append-only `R-NN`** in a VISION `## Revisions`
  ledger citing the triggering `F-NN`, cascading to the BLUEPRINT. The constitution gains a **third**
  "Boundaries vs. cuts" category — **correction-from-evidence** — with a decisive test: *evidence proved
  the target wrong* (allowed) versus *it was hard to build* (a forbidden cut). `/gg:orient --audit` flags
  an `R-NN` with no supporting `F-NN`.
- **`[exp]` — a backlog marker for the next experiment to run (`BACKLOG-FORMAT.md`).** A research action
  gets a distinct, non-droppable home beside `[bug]`, reusing the whole `capture → refine-backlog →
  discover` spine; it maps onto the normal dispositions (next phase = run it, discard = abandon the line).

### Changed
- **The BLUEPRINT rule reads "whole means settled, not always enumerated" (`BLUEPRINT-FORMAT.md`).** The
  foundation and the seams are settled once; where a property space is genuinely not-yet-knowable, the
  model carries an **extension point** — an open map / registry / plug-point — so a discovered property
  extends it in place without a migration. A closed domain is enumerated; an open one carries extension
  points, and that *is* its honest complete design. An extension point is reserved for what truly can't be
  enumerated — an open map used to dodge a knowable schema is the same dodge as a `[discovered]` tag on a
  checkable clause.

## 2.5.0

Support for **open-ended / experimental** projects — where the target is learned by *seeing* the product
run, and only the author's eye on the running result can judge whether it's right.

### Added
- **`FINDINGS.md` — a home for observations (`F-NN`).** `gg-shared/FINDINGS-FORMAT.md` and a
  lazily-created `.gg/FINDINGS.md` record what the running product *did* when it was run or tried — the
  fourth "decompose, don't drop" home beside a later task, a backlog item, and an `A-NN` assumption (each
  records a decision *not yet made*; a finding records an observation *already made*). `CAPTURE.md` routes
  by tense: a past-tense observation → `FINDINGS.md`, a future-tense idea / change / bug →
  `BACKLOG.md ## New`.
- **`[declared]` / `[discovered]` tags on every "done and perfect" clause (`VISION-FORMAT.md`).** A
  `[declared]` clause is judgeable without running, closed by an `AC-N`; a `[discovered]` clause is only
  judgeable by *watching the product run* — a felt / emergent / qualitative property — and is closed at a
  phase close by a cited `F-NN`, never asserted from a green suite. `/gg:next-task`'s close gate and
  `/gg:orient --audit` enforce it.
- **`show` tasks — the user's looks at the running product.** A `show` builds a watchable slice and stops
  for the user to look (`/gg:next-task` runs it and routes reactions to `FINDINGS.md` / the backlog, never
  inline). `/gg:discover` places shows **where the felt character meaningfully changes** — where a
  `[discovered]` clause becomes judgeable. The **first show is mandatory when the phase has a
  `[discovered]` clause**, anchored to the riskiest one, and **drives task ordering** so it lands as early
  as the non-retrofittable foundation allows (a thin vertical first, then thickened to full bar); its
  placement is surfaced for the user's veto at the `/gg:discover` sign-off. The user's try-it points are
  the phase's shows and the phase close. `PROGRESS-FORMAT.md` carries a task `type` column; `SPEC-FORMAT.md`
  a `## Shows` entry.
- **Grilling "elicit by reacting" (`GRILLING.md`).** For a subjective / `[discovered]` dimension, the
  agent shows a concrete contrast (a sketch, a vivid end-state, a small real sample) and records the
  user's *reaction* instead of an abstract menu pick — and names the cost of the lean option out loud
  before the user chooses, so an austere bar is never picked blind.
- **`/gg:quick` — a seventh command (the express lane), and `/gg:capture --next`.** For one small change
  you've already decided to do now (a phase just shipped, nothing else queued), `/gg:quick` records the
  single item straight into `.gg/BACKLOG.md ## Next phase` with a stable `B-NN` — skipping
  `/gg:refine-backlog`'s triage, because choosing to run `/gg:quick` *is* the triage — then runs
  `/gg:discover` to design just that item as its own micro-phase and hands to `/gg:next-task`. It does
  **not** skip the bar (recorded, designed, tested like any phase); in any other state it degrades to a
  plain `/gg:capture` so the idea is never lost. `/gg:capture --next` is the lighter sibling — it queues an
  item straight to `## Next phase` (self-triage to "do next") **without** designing it, and with no
  emptiness requirement, so several jots can assemble a multi-item next phase.

## 2.4.1

### Changed
- **Command specs consolidated — one home per rule, no restated copies.** `/gg:capture` now points at
  the shared `CAPTURE.md` protocol instead of re-listing its jot + reconcile steps; the `B-NN`
  assignment rule lives once in `BACKLOG-FORMAT.md` (capture and `CAPTURE.md` reference it). `/gg:orient`
  and `/gg:refine-backlog` drop inline paragraphs that duplicated wording already stated elsewhere (the
  `--audit` "changes nothing" caveat, the "idempotent, with deferral tiers" note). No behavior change —
  these mirror the constitution's "link, don't duplicate."

### Fixed
- **`STAGE.md` now describes the stage toggle accurately.** It states that `/gg:orient` offers the
  toggle on every report where there's a product to stage — `§4` skips the offer during `visioning` and
  in `--audit` mode — replacing the inaccurate "offers it every time it runs."
- **The constitution's breadcrumb rule no longer reads as binding on `/gg:capture`.** "Every working
  skill" is re-scoped with its explicit set (`/gg:ideate`, `/gg:discover`, `/gg:next-task`,
  `/gg:refine-backlog`, and `/gg:orient` on a stage flip) and excludes `/gg:capture`, which only jots and
  returns — so it can't be read to contradict capture's lighter close (a one-line confirmation, no
  ritual, no `JOURNAL.md` entry).

## 2.4.0

### Changed
- **`/gg:refine-backlog` is now one reviewed report + a single decision — no more item-by-item walk.**
  It reads the section (`## New` by default; `--later` / `--future` with the flag) and presents **one
  report**: every item with its idea, its `[bug]` marker if any, who raised it, what it touches, and the
  agent's **recommended disposition** (next phase / later / future / discard) with a one-line why. Then
  it asks **one** question — accept the recommendations, send **only the bugs** to the next phase
  (offered only when the set has a `[bug]`: it moves the bugs and leaves everything else exactly where it
  is), send everything, or decide item by item by id — and applies the whole set in a single pass.
  Previously it asked a disposition per item, which was slow on a long backlog.

### Added
- **Backlog items now carry a stable `B-NN` id**, mirroring the assumptions ledger's `A-NN` discipline.
  The id is assigned at `/gg:capture`, is **stable** (it travels with the item across every section and
  into the archive, and is never renumbered), and is **never reused** — the next id is one past the
  highest `B-NN` found in **both** `.gg/BACKLOG.md` and `.gg/BACKLOG-ARCHIVE.md`, so a new id can never
  collide with an applied or discarded one. The id is how you reference items in `/gg:refine-backlog`'s
  single decision, and `Relates` lines now point at the `B-NN`. `/gg:orient --audit` gained a check for a
  duplicate `B-NN`. Ids are assigned **going forward only** — a backlog created before this version is
  not back-filled; the plugin never rewrites an existing project's record to match a newer version.

## 2.3.1

### Fixed
- **`/gg:orient` no longer runs the audit unasked — and now surfaces it.** The `--audit` integrity pass
  is gated firmly to the flag — `/gg:orient` now **gates on the literal `$ARGUMENTS` value** instead of
  inferring it, so plain `/gg:orient` does the GPS report only (no drift-hunting) and **names**
  `/gg:orient --audit` as an available deeper check. (Previously the rich `--audit` checklist could leak
  into a default run, and the option wasn't advertised.)
- **All argument-bearing commands now gate on the literal `$ARGUMENTS`.** `/gg:next-task --gate`,
  `/gg:refine-backlog --later` / `--future`, and `/gg:capture`'s idea read the actual argument value
  instead of inferring it from context — more reliable flag handling across the board.

## 2.3.0

### Added
- **`/gg:orient --audit` — a read-only integrity check of the record.** A deep pass over the whole
  `.gg/` that flags drift the inline discipline doesn't re-validate: header-vs-artifacts contradictions,
  stale `A-NN` cross-refs, duplicated facts that drifted, an `AC-N` marked met without evidence, backlog
  hygiene, dangling blocks, and stray non-English prose. It reports each issue with the command that
  fixes it, **changes nothing** (no stage flip in this mode), and is meant for before a launch flip,
  after a migration, or on resuming a dormant project. It audits the record's *integrity* — never the
  product's *cuts*, which stay the inline self-accounting gate at phase close.

### Changed
- **The BLUEPRINT is now append-only.** `/gg:discover` designs the whole product in phase 0, and that
  design is then **frozen** — never edited. **Every refinement phase appends a dated `## Phase N`
  section** so the design ledger has no gaps, and the **depth scales**: a phase of only bugs/tweaks gets
  a one-line "no design change", while a phase that adds an entity/field/component or supersedes an
  earlier decision gets the full detail. Earlier content is never rewritten — the same move-don't-delete
  discipline as the `JOURNAL.md` and the assumptions ledger. This removes the silent design-drift that a
  living, hand-edited blueprint invites.
- **Link, don't duplicate, in the BLUEPRINT.** It restates no fact that lives authoritatively elsewhere
  — the test framework and run/verify commands belong to `RUNBOOK.md`, acceptance to `SPEC.md`, the
  "why" of a big call to an ADR — so a duplicated fact can't drift out of sync.

## 2.2.0

A real **backlog** with its own triage command. What used to be `NOTES.md` is now `BACKLOG.md`, and a
new `/gg:refine-backlog` lets you triage it one item at a time — instead of deciding everything inside
`/gg:discover`.

### Added
- **`/gg:refine-backlog` — a sixth command.** Between phases it walks each new backlog item one at a
  time and you give it a disposition: **next phase**, **later**, **future**, or **discard** (archived
  with a reason). It is **idempotent** — by default it walks only `## New`, so a triaged item is never
  shown again; `--later` / `--future` revisit those deferral tiers on purpose, and every run opens with
  the counts of all sections so nothing is invisible. It only triages — `/gg:discover` still designs.
- **`BACKLOG-ARCHIVE.md`.** Closed items leave the active backlog: **applied** items at phase close,
  **discarded** items when you drop them (with a recorded reason) — kept for the trace, never deleted.
- **Provenance + a `[bug]` marker on every item.** The `Captured` line records whether **you** raised
  it or the **agent** deferred it; a defect in shipped behavior is prefixed `[bug]`.

### Changed
- **`NOTES.md` → `BACKLOG.md`, sectioned by lifecycle.** The backlog is now `## New` / `## Next phase`
  / `## Later` / `## Future` instead of one `## Pending` list — an item's state *is* its section.
- **Triage left `/gg:discover`.** Discover no longer asks "which notes?" — it **consumes the
  `## Next phase` set** that `/gg:refine-backlog` already queued, and grills those items together.
- **A bug found while trying the shipped product is captured, not patched inline.** It flows through the
  normal `capture → refine-backlog → discover` cycle (so it gets a task, a test, and a `JOURNAL.md`
  entry), instead of an off-spec inline fix that left the record stale.
- **All `.gg/` content is written in English.** A new constitution rule pins the project's on-disk state
  to one language (verbatim user quotes excepted) so the agent's adherence stays sharp; it governs the
  `.gg/` prose only, never the project's own code or stack.

## 2.1.0

`/gg:ideate` is now **resumable**: `visioning` is a real, first-class state, not just a documented name.

### Changed
- **`visioning` is the project's first on-disk state.** `/gg:ideate` writes the `ROADMAP.md` header
  (`state: visioning`) the moment it scaffolds `.gg/`, *before* grilling — so an ideation cut before the
  vision is sharp is now **resumable**: re-run `/gg:ideate` and it continues the grilling from where it
  stopped instead of treating the project as already kicked off. The vision-sharp close promotes
  `visioning → scoping`.
- **Every command routes on `visioning`.** `/gg:discover`, `/gg:next-task`, and `/gg:capture` send a
  mid-ideation project back to `/gg:ideate`; `/gg:orient` reports "ideation in progress" and skips the
  stage toggle (there's no product to stage yet).
- **State vocabulary documented.** `ROADMAP-FORMAT.md` now spells out all four states —
  `visioning → scoping → building → shipped` — and that each one is resumable by re-running its owning
  command (`/gg:ideate`, `/gg:discover`, `/gg:next-task` respectively).

## 2.0.0

The first public release of `gg` — a Claude Code workflow plugin that builds a product in **phase 0**
and then refines it **phase by phase**, keeping all project state on disk in `.gg/`.

### The model
- **A "phase" is one `discover → next-task*` cycle.** Phase 0 builds the whole product end to end;
  phase 1, 2, … each fold in a selected set of captured notes. Within a phase, work is split into
  **tasks**.
- **Five commands**: `/gg:ideate` (once → a sharp VISION), `/gg:discover` (design the whole product
  into a BLUEPRINT + a testable SPEC + recorded ASSUMPTIONS + an ordered task list; in a refinement
  phase, a triage gate picks which notes to include, then grills them together), `/gg:next-task`
  (build exactly the next task, verify it internally, checkpoint, stop — the last task closes the
  phase and is the only point you try the product), `/gg:capture` (jot an idea into the backlog with
  light reconciliation; no grilling), and `/gg:orient` (read-only GPS + the dev/launched stage
  toggle).
- **Strict command preconditions.** Each command refuses out-of-order calls and routes you to the
  right one.

### Highlights
- **Discover-all-up-front + `BLUEPRINT.md`.** The whole data model/architecture is designed once, so
  later phases *extend* it instead of re-opening a frozen structure and writing migrations for it.
- **Recorded assumptions (`ASSUMPTIONS.md`).** Grilling asks the load-bearing questions and logs every
  other choice as a numbered, reversible default — *the cut is the unrecorded assumption*. High-blast
  decisions are always grilled; the discover sign-off surfaces defaults for veto.
- **A `dev` / `launched` stage.** While a product is in development, the system skips migrations,
  backward-compatibility, and data-preservation work; you flip to `launched` (via `/gg:orient`) when
  real users' data must survive (deployed ≠ launched), which seeds launch-readiness notes. Your own
  on-disk data is protected in both stages.
- **One task per `next-task` run; you try the product only at a phase close.** `PROGRESS.md` is both
  the task board and the handoff. Verification is the green RUNBOOK suite at the phase close, the
  runnable deliverable, and a self-accounting + VISION-conformance gate (there is no separate audit).
- **Boundary vs. deferral, made unambiguous.** A *boundary* is what the finished product will never
  include (→ `VISION.md`); a *deferral* is in-scope work pushed to a later phase (→ a note in
  `.gg/NOTES.md`, the only record re-read at the next discover). The constitution and the grilling
  protocol guard against mislabeling one as the other.

### On disk (`.gg/`)
`PRINCIPLES.md` · `VISION.md` · `ROADMAP.md` (state · phase · stage + phase log) · `BLUEPRINT.md` ·
`ASSUMPTIONS.md` · `SPEC.md` · `PROGRESS.md` · `NOTES.md` · `RUNBOOK.md` · `CONTEXT.md` ·
`JOURNAL.md` · `adr/`.
