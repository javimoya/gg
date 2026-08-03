# Audit mode — diagnose first, then one fix at a time

Diátaxis works in both directions: the same compass that classifies what you're about to write
classifies what already exists. An audit answers: *what is each piece of this documentation
actually doing, and for whom?* — then improves it the way Diátaxis prescribes: in small,
self-contained, immediately shippable steps. Never a big-bang rewrite; never a reorganisation
that leaves the docs half-moved. Documentation should be **complete at every stage, never
finished** — like a plant: always a whole organism, whatever its size.

## Phase 1 — Map and classify

For every document in scope (and for each section of mixed documents):

1. **Take a bearing**: action or cognition? study or work? → its *actual* form.
2. **Compare with its declared form** — its title, its location, its promises. A doc titled
   "Tutorial" that assumes competence is a how-to; "Advanced usage" full of tables is reference.
3. **Assess against its form's rules** (read the form's reference file) with the four Diátaxis
   assessment questions, verbatim:
   - What user need is represented by this?
   - How well does it serve that need?
   - What can be added, moved, removed or changed to serve that need better?
   - Do its language and logic meet the requirements of this mode of documentation?
4. Run the misplaced-content detectors (SKILL.md §4) at section and paragraph level.

## Phase 2 — Report findings

Deliver a findings table before touching anything:

| # | Location | Actual form vs declared | Finding | Severity | Proposed single fix |

Severity, by user harm:
- **High** — form collision that actively misleads (a "tutorial" that fails newcomers; reference
  that instructs wrongly; a how-to that teaches instead of directing).
- **Medium** — misplaced content that costs time (explanation buried in steps; options catalogues
  inlined; facts scattered outside reference).
- **Low** — friction (bare-noun titles, landing pages as link dumps, >7-item content lists,
  inconsistent reference patterns).

Each finding's proposed fix must be a **single self-contained action** — one move, one split,
one retitle — that leaves the docs better and whole if it's the only thing ever done. Order the
list by severity and let the user pick; don't start until they do.

## Phase 3 — Fix, one at a time

For each approved fix:

1. Re-read the affected text and the target form's reference file.
2. Apply exactly that fix. Moving content out of a doc means giving it a home (or naming the
   home to create) and leaving a link — deletion without relocation loses material; relocation
   without a link strands readers.
3. Verify the doc still stands alone: no dangling references, no half-moved sections, titles
   still true.
4. Stop. Report what changed. The next fix is a fresh decision, not momentum.

If while fixing you find a new problem, add it to the findings list — don't chase it now. One
action per iteration is what keeps every intermediate state shippable.
