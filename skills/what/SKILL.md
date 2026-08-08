---
name: what
description: >
  Re-pitch what you just told the user — it did not land. Use whenever the user signals they did
  not understand your last message: "no entiendo", "no te entiendo", "I don't understand", "you
  lost me", "wait — what?", "explícamelo de manera sencilla", "explícamelo más fácil", "con
  ejemplos fáciles de seguir", "explain that simply", "in plain terms" — or when their reply
  answers a different question than the one you asked. Most often fired at a question with
  options the user cannot tell apart. Also use when the user asks for a simple, easy-to-follow
  explanation (with easy examples) of anything you are telling them.
---

# what — re-pitch the thing that did not land

The user stopped understanding. The fault is in the message, not in the reader. Do not
apologise, do not defend the original, do not summarise the whole conversation. Take the one
thing that did not land — usually the last question, its options, or the explanation under a
recommendation — and pitch it again from further back.

## Back up, don't trim

A confusing message is missing context; it is not carrying too many words. Before any detail,
give the ground the original skipped:

- **What are we deciding?** One sentence.
- **What hangs on it?** What changes in the product or the code depending on the answer. One
  sentence.
- Only then the content itself.

Cutting words without adding this ground produces something shorter and no clearer. That is the
main failure mode: do not drop into a blunt, telegraphic register. Same substance, lower floor.

## One easy example per thing

Every option and every concept that survives into the re-pitch gets **one concrete,
easy-to-follow example**: the actual screen, the card, the command, the number, a two-line
before/after. Examples use this project's real objects — a real file, a real user action, real
data — never `foo`/`bar` or an invented abstract case. **Example first, rule after**: the
general statement is the caption of the example, not the other way round.

A load-bearing technical detail that must survive, survives *inside* an example — never as a
bare term.

## Simple to understand

The one measure of the re-pitch is that it is easy to follow. It prescribes no word source and
forbids none: use whatever words explain best, in whatever language the conversation runs in.

- Short sentences. One idea per sentence.
- Active voice. Everyday words.
- New vocabulary is welcome when it makes the thing easier to understand — give a term worth
  introducing its plain meaning in one clause, the first time it appears.

## When it was a question with options (the common case)

Re-explain the decision, then each option, then ask again:

1. **The decision**: what we are deciding and what hangs on it — the two sentences above.
2. **Each option**: plain name → what you get if you pick it, in one sentence → its one easy
   example → its cost, in one sentence. The same options as before — never drop one, never add
   one; the set maps 1:1 to the original ask.
3. **The recommendation, restated**: which one you would pick and why, in one sentence.
4. **Ask again, and wait** — through the same channel the question first went out (a
   selectable-options tool re-asks with the tool: option set and labels unchanged, descriptions
   rewritten to the plain form). The user not understanding is never license to decide for
   them: a re-pitch that ends "so I went with A" has stopped being one.

## Landing check

End with the door open — invite the user, in the conversation's language, to name which part is
still unclear. If a second `what` fires on the same message, the next re-pitch changes the
example and backs up further. Never the same words, louder.

## In gg projects

METHOD.md → Grilling, "Explain before you ask", is the prevention: anything put to the user
should arrive already explained like this. This skill is the recovery for when that failed —
and it stands alone: it works in any conversation, in any project, gg or not.
