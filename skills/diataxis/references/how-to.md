# How-to guide — directions (goal-oriented)

A how-to guide gives **directions** through a real-world problem to a result, for an
**already-competent** user who is **at work**. It answers "How do I…?". It does not teach, does
not explain, does not describe the machinery — it navigates. A rich set of how-to guides is also
marketing: it shows what the product can do.

**Obligation: the user accomplishes their task, correctly and safely.** Responsibility sits with
the user (they know their situation); your job is directions that survive contact with it.

## Do

- **Action and only action.** Every sentence either moves the user forward or prepares the next
  move. "Action" includes judgement — address how the user should *think* (what to check, how to
  decide between paths) as well as what to type.
- **Address real-world complexity.** A guide that works for exactly one narrow case is rarely
  worth having. Use conditional imperatives to stay adaptable: *"If you want x, do y. To achieve
  w, do z."* Sequences may fork and branch; a how-to has multiple entry and exit points.
- **Omit the unnecessary.** Practical usability beats completeness. Unlike a tutorial, a how-to
  guide need not start from zero or run to the end: start and end somewhere reasonable and let
  the reader join it to their own work.
- **Sequence for flow.** Order steps by practical necessity and by the rhythm of the user's
  attention: minimise how long a thought must be held open before it resolves in action; avoid
  jumps back to earlier concerns. Aim to *anticipate* — the helper who has the tool ready before
  the user reaches for it.
- **Write from the user's purpose, not the machinery's.** "Select the appropriate options and
  press Deploy" is not guidance — it's the UI narrated. Map the human goal to the choices: which
  options serve which real-world need.
- **Link out for completeness.** *"Refer to the X reference for the full list of options."* Never
  inline the option catalogue.

## Don't

- **Don't teach.** No defining terms, no familiarising with tools, no "as you may remember".
  Assume competence; a reader who needs teaching needs the tutorial — link it and move on.
- **Don't explain.** A clause at most, then a link. The user is mid-task; digression dilutes the
  guide's power.
- **Don't describe the machinery.** Facts, defaults, full flag lists live in reference.
- **Don't pad for completeness.** Every step earns its place by serving the goal.

## Sentence patterns

EN: "This guide shows you how to…" · "If you want x, do y." · "To achieve w, do z." · "Check
that… before continuing." · "Refer to the x reference for a full list of options."

ES: "Esta guía muestra cómo…" · "Si quieres x, haz y." · "Para conseguir w, haz z." ·
"Comprueba que… antes de continuar." · "Consulta la referencia de x para la lista completa de
opciones."

## Titles

Always say exactly what the guide achieves, verb-first: **"How to integrate application
performance monitoring"**. Not the gerund ("Integrating APM" — procedure or essay?), never the
bare noun ("Application performance monitoring" — could be anything). Search engines agree.

## Checklist before shipping

- [ ] Title starts with "How to" + the actual outcome.
- [ ] Assumes competence; zero teaching, zero term-definitions.
- [ ] Conditionals cover the realistic variations; no false single path.
- [ ] Starts and ends at sensible boundaries; no zero-to-hero padding.
- [ ] Options/facts linked to reference, "why" linked to explanation.
