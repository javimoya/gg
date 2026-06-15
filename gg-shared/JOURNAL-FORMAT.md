# JOURNAL.md format

`.gg/JOURNAL.md` is the project's **append-only narrative history** — one entry per working session,
in order. It captures every session (ideation, discovery, each `next-task` run), and at a **phase
close** it absorbs the hand-off: what the phase built, how to verify it, the acceptance evidence, and
the test delta. `/gg:orient` reads the latest entry to recall "what just happened".

## Structure

Newest entries are appended at the BOTTOM. The ordinary per-session entry:

```md
# Journal — {Project name}

## {YYYY-MM-DD} — /{skill}: {short title}
- **Did**: {2–6 bullets: what happened; decisions taken (link SPEC/ADR/A-NN); what was written}
- **State change**: {state X → Y; phase N; stage flip; or "—"}
- **Next**: {the one concrete next action — same as the session's breadcrumb}
```

A **phase close** (the last `/gg:next-task` of a phase) uses the richer variant — it is the hand-off
the next phase reads:

```md
## {YYYY-MM-DD} — /gg:next-task: phase {N} shipped — {short title}
- **Built**: {the capability this phase delivered}
- **How to verify**: {the SPEC's "How to see it" command} → **observed**: {the real result you saw}
- **Acceptance evidence**:
  | AC | evidence type | check run | observed result | status |
  |----|---------------|-----------|-----------------|--------|
  | AC-1 | automated | {test} | {what you saw} | confirmed |
- **Tests**: baseline {N pass / M fail at phase start} → close {N pass / 0 fail} ({the RUNBOOK suite})
- **Notes applied**: {which `## Pending` notes were folded, now in NOTES `## Applied`} (— for phase 0)
- **VISION conformance**: {does the product now meet "done and perfect", or what remains}
- **State change**: building → shipped; phase {N}
- **Next**: {the breadcrumb — try it, then capture + discover for the next phase}
```

## Rules

- **Append-only. Never edit or delete a prior entry.** A correction is a new entry. This is what lets
  a future session reconstruct how the project actually evolved, missteps included.
- **One entry per working session**, written by the close ritual (`CLOSE-FORMAT.md`). `/gg:orient`'s
  read-only report writes nothing here — only its stage flip, a working action, gets an entry.
- **The phase-close entry is the hand-off and is self-sufficient.** "How to verify" cites the REAL
  observed result (never "should work"); every `AC-N` is `confirmed` only with cited evidence; the
  test line is baseline→close with the RUNBOOK command. An `AC` you can only infer is NOT met.
- **`Next` mirrors the breadcrumb** so the journal and the hand-off never disagree.
- **Link, don't duplicate.** Point to the SPEC/BLUEPRINT/ADR that holds the detail.
- **Created lazily** at the first close, with the `# Journal — {name}` header.
