# Reference — description (information-oriented)

Reference is **technical description of the machinery and how to operate it** — the facts a user
consults *while working*. One hardly *reads* reference; one **consults** it. Its value is truth
and certainty: a firm platform to stand on mid-task. It is a **map**: it lets the user know the
territory without having to go and check the territory themselves — so it must be wholly
authoritative, with no doubt or ambiguity.

Uniquely among the four forms, reference is **led by the product, not by user needs**: its
structure mirrors the structure of the thing it describes, so user and documentation can walk
the machinery together.

**Obligation: describe, as succinctly as possible, in an orderly way, and be right.**

## Do

- **Describe and only describe.** Neutral description is the key imperative — and it is
  *unnatural*: the natural instinct is to explain, instruct, opine. Resist all three.
- **Mirror the product's structure.** Sections follow the shape of the API/CLI/schema — without
  forcing an unnatural order where the code's own logic suggests a better one.
- **Adopt standard patterns, uniformly.** Reference is useful when it is consistent: same
  layout, same field order, same terminology for every command/class/option. Readers should find
  things where they expect them, formatted as they expect. Austerity is a feature; this is not
  the place for stylistic delight.
- **Give examples that illustrate without explaining.** A usage example shows a command in
  context — full stop. The moment it starts arguing "why", it's growing an explanation: cut and
  link.
- **State warnings as facts of correct use.** *"You must use a. You must not apply b unless c.
  Never d."* Describing the correct way to use something — including how it behaves and its
  limits — is still description, not instruction.
- **Link out when description feels inadequate.** If a bare description seems useless without
  context, don't inline the context — link to the how-to guide, tutorial or explanation.

## Don't

- **Don't instruct.** No task walkthroughs; that's the how-to guide's job.
- **Don't explain or opine.** No design rationale, no "this is better because" — link to
  explanation.
- **Don't decorate.** No storytelling, no vocabulary flourishes.
- **Don't equate auto-generated API docs with documentation.** Auto-generation keeps reference
  faithful to the code and that's valuable — but it covers one quadrant of four, and only partly.

## Sentence patterns

EN: "X is available as Y, defined in Z." · "Sub-commands are: a, b, c." · "Default: 30 s.
Accepts: integer seconds." · "Returns …; raises … when …" · "You must use a. Never d."

ES: "X está disponible como Y, definido en Z." · "Los subcomandos son: a, b, c." · "Valor por
defecto: 30 s. Acepta: segundos (entero)." · "Devuelve …; lanza … cuando …" · "Debes usar a.
Nunca d."

## Titles

The name of the thing described, plainly: "CLI reference", "`config.yaml` fields", "HTTP API".
No verbs, no promises — a map is titled by its territory.

## Checklist before shipping

- [ ] Every fact verified against the code/product as it is now.
- [ ] Structure mirrors the machinery; entries follow one uniform pattern.
- [ ] Zero explanation, zero instruction; links where they'd have crept in.
- [ ] Complete for its declared scope — a map with holes misleads.
- [ ] Boring. (For reference, that's praise.)
