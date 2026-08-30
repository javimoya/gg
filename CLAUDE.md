# CLAUDE.md — gg

## Releasing a version
- Bump `version` in `.claude-plugin/plugin.json` and add a `## X.Y.Z` entry to `CHANGELOG.md`.
- If the release changes `gg-shared/` formats or the record's shape, it is **format-changing**: add
  its conversion section to `CONVERSION.md` **and its row to the index there** — the `gg:` stamp in
  each project's BACKLOG header plus that index is how `/gg:tidy` detects and offers conversions.
  A release that changes only command behavior needs neither.
- Commit to `main` (repo convention: commit title `X.Y.Z — summary`), then `git push origin main`.
- Annotated tag: `git tag -a vX.Y.Z -F -` then `git push origin vX.Y.Z`.
- **`gh` IS installed** and authenticated against both `github.com` (account `javimoya` — the one
  this repo uses) and `git.epo.org` (work). Publish the Release with
  `gh release create vX.Y.Z --title "X.Y.Z — summary" --notes "…"` after pushing the tag. If
  `github.com` auth has lapsed (401), pushing the tag is the deliverable — ask the user to re-run
  `gh auth login --hostname github.com --web`, or they publish via
  `https://github.com/javimoya/gg/releases/new?tag=vX.Y.Z`.

## Changing a command or format
When you change a command or a `gg-shared/` format, write **only the clean, going-forward behavior**.
**Never add migration, back-fill, or backward-compat logic to "fix up" projects created under an older
version** — no "back-fill the id for items that lack one", no "if the old field is missing, default
it", no version-detection branches. A command spec describes **one** behavior, not a version history. A
`.gg/` that predates a change is the user's to reconcile by hand (or via `CONVERSION.md` in a
dedicated session); the plugin never silently repairs it. This mirrors gg's own method — recreate,
don't shim. The same rule applies to `/gg:tidy`: it may **report** drift and **offer** the
conversion `CONVERSION.md` records (run only on the user's explicit yes) — it never assumes an
older project must be upgraded and never converts on its own.

## What gg is
A pure-Markdown command plugin — **a record with habits, not a workflow engine** (v5). Three
commands: `new` grills a new project's destination (one question at a time, recommendation first,
ELI18) and seeds the record; `go` is the work loop — one thing (a pitch, a bug, backlog ids) from
ask to **one landed commit carrying code + record together**, titled `{area}: {summary} (B-NN)`,
grilling only design weight, building directly or through implementation subagents with every
diff reviewed by the session; `tidy` is the record's diet — the only command that reads `.gg/`
whole or measures bounds, report first, apply on one yes. All project state lives in each
project's `.gg/` — **four bounded files** (BACKLOG — capture only, with the `next-id:` counter
and the `gg:` version stamp; DESIGN — current truth, `## Product` up top; CONTEXT — the glossary;
RUNBOOK — commands plus the standing `## Deploy` convention) plus `adr/`. The method and formats
are one file, `gg-shared/GG.md`, read from the plugin and never copied into projects. **History
is git's**: no journals, archives, or changelogs — applied blocks are deleted in the commit that
lands them, and gg never pushes. There is no runtime, no state machine, and no state.json by
design. The batch/board machinery (plan, fix, where, WORK, NOTES, PRODUCT, delegated execution)
was retired in 5.0.0 on production evidence — see `CHANGELOG.md`.
