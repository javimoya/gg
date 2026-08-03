# Tutorial — a lesson (learning-oriented)

A tutorial is an **experience** the learner lives under your guidance: a practical activity with a
meaningful, achievable goal, through which they *acquire* skill. It is a lesson — and in a lesson
nearly all responsibility falls on the teacher. The learner's only job is to follow directions;
if they do and something breaks, the failure is yours. What the learner *does* is not what they
*learn*: they build the sample app, but they learn the tools, the vocabulary, the feel of the
craft, and the confidence to come back.

**Obligation: a successful learning experience.** Not task completion, not coverage.

## Do

- **Say where you're going.** Open by describing what will be achieved: *"In this tutorial we
  will create and deploy a scalable web application. Along the way we will meet containerisation
  tools."* Never open with "In this tutorial you will learn…" — presumptuous, and it frames study
  instead of experience.
- **First-person plural throughout.** "We" affirms that tutor and learner are in it together.
- **Visible results, early and often.** Every step produces something the learner can see,
  however small — cause and effect, rapidly and repeatedly.
- **Maintain a narrative of the expected.** The learner is anxious at every step. Keep telling
  them what they'll see: *"You will notice that…"*, *"The output should look something like…"*,
  *"After a few moments, the server responds with…"*. Show exact expected output. Flag failure
  signs: *"If the output doesn't show X, you have probably forgotten Y."* Warn of surprises
  before they happen ("this command prints several hundred lines").
- **Point out what to notice.** Learning requires reflection: *"Notice that…"*, *"Remember
  that…"*, *"Let's check…"* — observation is an active part of the craft.
- **Encourage repetition.** Design steps that can be re-run safely; repetition is sometimes the
  only teacher.
- **Stay concrete.** *This* problem, *this* command, *this* result. Learning moves from the
  concrete and particular toward the general — never start at the general.
- **Aspire to perfect reliability.** A learner who follows your directions and doesn't get the
  promised result loses confidence in the tutorial, the tutor and themselves. Test the whole
  path; pin versions; eliminate environmental surprises. The tutorial must be safe: nothing the
  learner does can break their machine or their project.
- **Close by naming the achievement.** Describe (and mildly admire) what they built, and point to
  where their study can continue.

## Don't

- **Don't explain.** The hardest temptation. The absolute maximum: one clause in the plainest
  language — *"We use HTTPS because it's more secure"* — plus a link to the explanation doc.
  Explanation is only pertinent when the *learner* wants it, and mid-lesson they don't.
- **Don't offer choices or alternatives.** No flags they could also use, no second way to do it.
  One managed path. Options belong to how-to guides and reference.
- **Don't abstract or generalise.** No "in general, systems like this…" — that's explanation.
- **Don't assume prior knowledge or let gaps stand.** Be explicit about the basics (which
  directory, which window, what the prompt should look like). If the reader must fill gaps, you
  have written a how-to guide and mislabelled it.

## Sentence patterns

EN: "In this tutorial, we will…" · "First, do x. Now, do y. Now that you have done y, do z." ·
"The output should look something like…" · "Notice that… / Remember that…" · "If the output
doesn't show…, you have probably forgotten to…" · "You have built…"

ES: "En este tutorial vamos a…" · "Primero, haz x. Ahora, haz y. Ahora que has hecho y, haz z." ·
"La salida debería parecerse a…" · "Fíjate en que… / Recuerda que…" · "Si la salida no
muestra…, probablemente te ha faltado…" · "Has construido…"

## Titles

Promise the experience: "Your first X", "Let's build a Y", "Getting started with Z" — never a
bare topic noun, never "How to…" (that's a how-to guide's promise).

## Checklist before shipping

- [ ] End-to-end complete: a newcomer on a clean environment reaches the promised result.
- [ ] Zero forks: no options, no "alternatively".
- [ ] Every step shows its expected result; failure signs flagged.
- [ ] Explanations ≤ one clause each, linked out.
- [ ] "We" voice; opening promise; closing recap.
