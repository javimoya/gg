---
name: diataxis
description: >
  Write and audit user-facing technical documentation with the Diátaxis framework (tutorials,
  how-to guides, reference, explanation). Use whenever the task is to create, improve, review or
  restructure product documentation — a README, getting-started or installation guide, usage
  guide, API/CLI reference, conceptual or architecture doc, or a whole docs/ tree — even if the
  user never says "Diátaxis". Also use when asked whether existing docs are well organised. Not
  for `.gg/` record files (FORMATS.md owns those), changelogs, commit messages, docstrings or
  code comments.
---

# Diátaxis — documentation by user need

Diátaxis (diataxis.fr) derives four — and only four — forms of documentation from two axes of
user need: whether the content informs **action** (doing) or **cognition** (knowing), and whether
it serves the user's **acquisition** of skill (study) or **application** of skill (work). Each
form serves exactly one need; most documentation problems are one form bleeding into another.

| | serves study (acquisition) | serves work (application) |
|---|---|---|
| **informs action** | **Tutorial** — a lesson | **How-to guide** — directions |
| **informs cognition** | **Explanation** — discussion | **Reference** — description |

| | Tutorial | How-to guide | Reference | Explanation |
|---|---|---|---|---|
| oriented to | learning | goals | information | understanding |
| answers | "Can you teach me to…?" | "How do I…?" | "What is…?" | "Why…?" |
| analogy | cooking lesson | recipe | food-packet label | culinary history essay |

## 1. Classify first (the compass)

Before writing a word, take a bearing with two questions:

1. Does this content inform **action** (steps, doing) or **cognition** (facts, thinking)?
2. Does it serve **acquisition** (the user at study) or **application** (the user at work)?

The answers name the form. State the classification in one line before writing, so the user can
veto it — e.g. *"Form: how-to guide (competent user, at work, wants the result)."*

- **Ambiguity is about audience, not topic.** "Document installation" is a tutorial if it exists
  to win over newcomers, a how-to if it serves practitioners setting up yet another machine. If
  the surrounding context (existing docs, who the project is for, what the user asked) doesn't
  settle it, ask the user the two compass questions — don't guess, a misclassified audience bends
  the whole document.
- **Never classify by difficulty.** Basic ≠ tutorial, advanced ≠ how-to. An expert taking a course
  on a hard topic is at study (tutorial); a beginner doing routine paperwork is at work (how-to).
- The compass applies at every level: a document has one form, but re-take the bearing per
  section, paragraph, even sentence, whenever the writing starts to feel difficult — that feeling
  is usually a form violation.

## 2. Load the form's rules

Read the matching reference file before writing — it carries the obligations, the do/don't
rules and the sentence patterns for that form:

- `references/tutorial.md` — learning-oriented lessons
- `references/how-to.md` — goal-oriented directions
- `references/reference.md` — information-oriented description
- `references/explanation.md` — understanding-oriented discussion

## 3. Rules that hold across all forms

- **One form per document; link, don't absorb.** When content of another form wants in (a "why"
  inside steps, options inside a guide), move it to its own document — or name where it will live —
  and link. Inlining harms twice: it pollutes this document and hides the content from its right
  place.
- **Never create empty scaffolding.** Do not lay out tutorial/how-to/reference/explanation
  folders or headings with nothing in them — Diátaxis calls this out verbatim: "Don't do that.
  It's horrible." The four-part structure is an *outcome* of well-formed documents accumulating,
  never a template to pre-fill. Write the one document that's needed now, complete and shippable.
- **Four boxes are not the required navigation.** A docs tree may nest Diátaxis under another
  dimension (per-platform, per-persona) or vice versa; what's non-negotiable is purity *within*
  each document. Keep any contents list to ~7 items (mechanically ordered lists may run longer);
  a landing page reads as an overview with context, never a bare list of links.
- **README stance, by project size.** Small project, single doc: one README whose sections are
  cleanly separated by form, each section obeying its form's rules — the compass applied at
  section level. Project with a docs/ tree: the README is a short landing/overview that links out
  to the four forms; it does not duplicate them.
- **Language.** Match the language of the project's existing documentation; in a fresh project
  with no signal, write in English. The reference files carry sentence patterns in English and
  Spanish.
- **Accuracy is on you, not the framework.** Diátaxis structures content; it does not make it
  true. Verify every command, flag, path and behaviour against the actual code or product before
  asserting it — a perfectly-formed document that lies is worse than none.
- **Out of scope.** `.gg/` record files keep their FORMATS.md shapes; changelogs, commit
  messages, docstrings and code comments are not Diátaxis documents.

## 4. Self-check: misplaced-content detectors

Scan the draft (or the doc under audit) against these symptoms:

| Symptom | Diagnosis | Move |
|---|---|---|
| Choices, branches, "if you want X" in a tutorial | how-to leaked in | one managed path; cut alternatives |
| "Why" digressions inside steps | explanation leaked in | one-line why at most + link out |
| A "tutorial" that assumes the reader fills gaps | it's a how-to | rename/reframe, or make it truly safe and complete |
| A "how-to" that defines terms, teaches tools | it's a tutorial | assume competence; strip teaching |
| Reference examples growing "why" paragraphs | explanation trapped | split into an explanation doc, link |
| List/table-shaped, unmemorable prose in an essay | reference trapped | extract to reference, link |
| Reads fine away from the keyboard, filed under API docs | explanation trapped | extract to explanation |
| Doc title is a bare noun phrase ("Deployment") | form undeclared | retitle: "How to deploy…" / "About deployment" / etc. |

## 5. Audit mode

When the task is to review or restructure existing docs, read `references/audit.md` and follow
it: map and classify every document (and mixed sections within documents), report findings with
severity, then — only after the user picks — apply fixes **one at a time**, each self-contained
and shippable. Never big-bang rewrites, never a reorganisation that leaves the docs half-moved.
