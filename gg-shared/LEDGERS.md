# Reading & editing .gg (shared)

`.gg/` files grow for the life of the project; a session's context does not. Every command therefore
reads by **anchor**, not by volume, and edits against a **fresh** view of the target block. A grown
ledger can exceed the Read tool's size limit outright — so a whole-file read is never load-bearing:
anything a command needs from a grown file must be reachable by heading or id.

## Read by anchor

- **Locate, then read the span.** A section is found by its heading (`## Open`, `## Phase log`), an
  entry by its id anchor (`### A-07 —`, `### B-12 —`, `### F-03 —`, `**AC-21**`) — search for the
  anchor, then read just that span. Never read a whole ledger "for context"; when an anchored read
  raises a question, follow the ids it cites, one hop at a time.
- **Per-file read depth** — what a command start needs:

  | File | Read |
  |---|---|
  | `ROADMAP.md` | `## State` + `## Phase log` only. The `## Structural changelog` is write-only outside `/gg:orient --audit`. |
  | `PROGRESS.md` | Whole — it holds exactly the current phase (`PROGRESS-FORMAT.md`). |
  | `SPEC.md` | `## Goal`, the current phase's `AC-N` (the `(phase {N})` tag), `## Shows`, the deliverable, `## Open questions`. Another phase's criteria by anchor, on demand. |
  | `BLUEPRINT.md` | `## Shape`, the current `## Phase {N}` section(s), and the phase-0 sections the work at hand touches. |
  | `ASSUMPTIONS.md` | `## Open` only; the archive never. |
  | `JOURNAL.md` | The last entry — read the file tail, never the whole file. |
  | `FINDINGS.md` | Only the `F-NN` blocks the work at hand cites. |
  | `BACKLOG.md` | The section the command acts on. |
  | `VISION.md` / `RUNBOOK.md` / `CONTEXT.md` / `PRINCIPLES.md` | Whole — bounded by design. |
  | `.gg/adr/` | `ls` the directory; read the ADRs whose slug touches the work at hand. |

- **Next-id computation is an anchor scan.** The next `B-NN` / `A-NN` / `F-NN` / `AC-N` is one past
  the highest id matching the anchor pattern across the file(s) its format names (active **and**
  archive) — a pattern scan, never a whole-file read.

## Edit by fresh anchor

- **Re-read the exact block immediately before editing it.** Locate its anchor and read just that
  span; never derive the old text from an earlier or partial view of the file — that is how stale
  edits miss.
- **Anchor edits on short, stable spans** — an id heading, a bold field label, a table row's leading
  cells — never on long prose that may have drifted.
- **Prefer append-at-anchor** — right after a section heading, or at the end of a newest-last ledger —
  over rewriting surrounding content. If an edit fails to match, re-read the block from disk and
  re-anchor; never retry from memory.

## Formats are closed

Write exactly the sections and fields a format defines — never invent a field, a column, a section, or
an archive area (no "archive of earlier phases" inside a working file, no extra backlog fields).
Content with no defined home goes into the JOURNAL entry or a `B-NN`, not into new structure.

## One fact, one home

A datum lives once, in the file that owns it; every other mention is the bare id or a pointer
(`AC-12`, `F-07`, `ADR-0004`). Restating a block in a second file is drift waiting to happen
(`/gg:orient --audit` flags it).
