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

### Elicit by reacting (subjective dimensions — show, don't only ask)
Some choices can't be settled by an abstract menu — they're *felt*: the look, the tone, the pace, the
aesthetic, the sense that it's right. For any dimension you'd tag `[discovered]`
(`VISION-FORMAT.md`), the user often **can't know what they want until they see it**, so a word-pick
silently locks the wrong target. Don't force the pick — provoke a reaction:
- **Show a concrete contrast, not a list of adjectives.** Sketch two or three end-states vividly (or
  show a tiny real sample / ASCII mock / reference example) and ask which is closer — record the
  *reaction*, not a menu label.
- **Name the latent cost of the lean option out loud.** When one choice is cheaper to build or to
  *certify* but risks disappointing, say so before they choose: *"the lean target is cheap to hit but may
  feel flat in use — is that acceptable, or is the quality you're after part of the bar?"* If the user
  takes the austere option, they must take it **knowing** the cost, never by default.
- A dimension that stays subjective after this becomes a `[discovered]` "done and perfect" clause,
  confirmed later by *watching the product run* (a cited `F-NN`), not by a green suite.

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

## The queued set (refinement phases only)

Triage no longer happens in grilling — `/gg:refine-backlog` owns it (one reviewed report, then a single
decision) before `/gg:discover` runs. When `/gg:discover` opens a **refinement phase** (phase N, after a product already
exists), it reads the set the user already queued in `.gg/BACKLOG.md ## Next phase` — **that set *is*
the phase**. Grilling then runs over those items **together** — a joint view, so their tasks come out
coherent, not item-by-item. Items still in `## New` / `## Later` stay in the backlog for a later phase
(never dropped) — **except** when `/gg:discover` is asked to **re-scope the current phase in place** (a
`show` revealed the plan must change — `discover.md` §0 / §2): it then folds the just-captured `## New`
reactions into *this* phase and promotes them to `## Next phase`, so they archive at this phase's close.

## The anti-scope-cut bar applies here too

Grilling is **never** a tool for reducing scope. It defines the complete product at the agreed bar; it
only decides *order/phase* and *which defaults stand*, never lowers the *what*. Anything that surfaces
"for later" follows the constitution's honest moves — a later **task**, a **backlog item** in
`.gg/BACKLOG.md`, or a low-stakes **assumption** (`A-NN`) — never a silent drop or a deferral mislabeled
as a boundary. (See `CONSTITUTION.md` → "Boundaries vs. cuts" for the decisive test, the trap, and the
worked example.)
