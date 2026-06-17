# Grilling protocol (shared)

This is the interrogation method used by `/gg:ideate` and `/gg:discover`. The goal is to **reach a
sharp, shared understanding before writing any code** — squeezing what is still vague out of the
user's head and forcing precision — while staying fast by **recording good defaults instead of asking
everything**.

## How to grill

- **Interrogate the load-bearing decisions** until you both hold the same mental picture. Walk the
  decision tree branch by branch, resolving dependencies one at a time (decision A constrains B;
  settle A first).
- **One question at a time.** Wait for the answer before continuing. No batteries of questions.
- **For every question, give your recommended answer** and why, plus the alternatives you weighed and
  why they lose — lead with the recommendation. A question without a recommendation is work you're
  pushing back onto the user.
- **If a question can be answered by exploring, explore** the code and docs instead of asking. Don't
  ask what you can find out yourself.
- Use the harness's question tool for clean forks, but always leave the door open to free-form
  answers: the best answers often don't fit a menu.

## Ask the big questions; default the rest (and record every default)

Grilling can't ask everything or it never ends. Ask the **load-bearing** question(s) for each area;
for everything else, **take the best default and record it as an `A-NN` assumption** in
`.gg/ASSUMPTIONS.md` (`ASSUMPTIONS-FORMAT.md`).

- **The cut is the *unrecorded* assumption**, not the default itself (`CONSTITUTION.md` → "Defaults
  and assumptions"). A logged default is a decision on the record, reversible later by a note.
- **High-blast-radius decisions are always questions, never defaults.** If a choice can't be cheaply
  reversed later by a note — the data model, the platform/stack, sync vs async, the core UX — grill
  it. Only low/medium-blast choices are defaulted.

## During the session

### Sharpen fuzzy language
When the user uses a vague or overloaded term, propose a precise canonical term. *"You're saying
'account' — do you mean the Customer or the User? Those are different things."* When a term is
resolved, **update `CONTEXT.md` right there** (format in `CONTEXT-FORMAT.md`); don't batch it.

### Challenge against the glossary
If the user uses a term that conflicts with the language already fixed in `CONTEXT.md`, call it out at
once. *"Your glossary defines 'cancellation' as X, but you seem to mean Y — which is it?"*

### Stress-test with concrete scenarios
When you discuss domain or design relationships, invent specific scenarios that probe edge cases and
force precision about the boundaries between concepts. *"What if an order arrives with no line items?
What if the partial payment exceeds the total?"*

### Cross-reference with code
When the user states how something works, check whether the code agrees. On a contradiction, surface
it: *"Your code cancels whole Orders, but you just said partial cancellation is possible — which is
true?"*

### Bring what the user hasn't thought of (especially in ideation)
Don't just collect the user's ideas. Add divergence: design alternatives, prior art, approaches from
other domains, risks they haven't seen, and "have you considered X?". **In ideation, push laterally**:
propose possible features, capabilities, and out-of-the-box ideas the user hasn't named — angles from
adjacent products and other domains — to open up areas they haven't contemplated. **Diverge first**
(generate and contrast options), **then converge** (grill until it's nailed down).

### Confirm before you stop — and confirm the defaults you took
When you think you've worked through the load-bearing questions, **don't silently move on** to
writing. Two checkpoints, both mandatory:
1. Name the areas you have *not* yet probed and **propose concrete ones worth opening** — unexplored
   features, edge cases, integrations, failure modes — and let the user decide whether to keep going.
2. **Surface the defaults you took.** Show the high/medium-blast `A-NN` entries explicitly so the
   user can veto or change any before you close (the low-blast ones are listed, not walked one by
   one). The user decides when questioning ends *and* which defaults stand — not the running-out of
   your list.

### ADRs with judgment
Offer to record a decision as an ADR **only** when all three are true (see `ADR-FORMAT.md`): hard to
reverse, surprising without context, and the result of a real trade-off. If one is missing, no ADR.

## The note triage gate (refinement phases only)

When `/gg:discover` opens a **refinement phase** (phase N, after a product already exists), it first
reads `.gg/NOTES.md ## Pending` and asks the user **which notes this phase includes** — all, a
specific one, a recommended set (with one-line reasoning), or a pick — **calling out the `[bug]`-marked
notes** (defects in shipped behavior) so fixes can be prioritized over new ideas. The selected set *is*
the phase. Grilling then runs over the selected notes **together** — a joint view, so their tasks come out
coherent, not note-by-note. Unselected notes stay pending for a later phase (never dropped).

## The anti-scope-cut bar applies here too

Grilling is **never** a tool for reducing scope. If something "for later" surfaces during the
interrogation, it is not dropped — it follows the constitution's moves (`CONSTITUTION.md`,
`CAPTURE.md`): a later **task** of the current phase, a **note** in `.gg/NOTES.md` for a future
phase, or — for a low-stakes choice — a recorded **assumption** (`A-NN`). Grilling defines the
complete product at the agreed bar; it only decides *order/phase* and *which defaults stand*, never
lowers the *what*.

**Never mislabel a deferral as a boundary.** Something pushed to a later phase is *sequencing* → a
**note** in `.gg/NOTES.md` (the only record re-read at the next discover); a *boundary* is something
the finished product will **never** include (→ `VISION.md`). Test it: *will the product ever include
this?* If there's a "later", it's a note, not a boundary — and it must not be parked in the VISION, a
SPEC line, or the JOURNAL, where nothing re-reads it. Watch the partial case: an area can be part
boundary, part deferral — classify each piece separately. (See `CONSTITUTION.md` → "Boundaries vs.
cuts" for the decisive test, the trap, and the worked example.)
