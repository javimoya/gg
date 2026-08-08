# CLAUDE.md — gg

## Releasing a version
- Bump `version` in `.claude-plugin/plugin.json` and add a `## X.Y.Z` entry to `CHANGELOG.md`.
- If the release changes `gg-shared/` formats or the record's shape, it is **format-changing**: add
  its conversion section to `CONVERSION.md` **and its row to the index there** — the `gg:` stamp in
  each project's WORK header plus that index is how `/gg:where --audit` detects and offers
  conversions. A release that changes only command behavior needs neither.
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
don't shim. The same rule applies to `/gg:where --audit`: it may **report** drift and **offer** the
conversion `CONVERSION.md` records (run only on the user's explicit yes) — it never assumes an
older project must be upgraded and never converts on its own.

## What gg is
A pure-Markdown command plugin: five commands (`new`, `plan`, `go`, `fix`, `where`) that build a
product in **batch 0** (`new`: vision + whole design, one arc) then refine it **batch by batch**. A
"batch" is one `plan → go*` cycle: `plan` opens it in one ceremony (capture + triage + design, one
consolidated gate; ceremony scales with item weight S/M/L), `go` builds it one row per run and
closes it on evidence (green suite + the user walking the Try list). `fix` is the fix-now-record-after lane for small decided changes; `where` is the read-only
GPS. All project state lives in each project's `.gg/` — **seven bounded files** (WORK, BACKLOG,
PRODUCT, DESIGN, NOTES, CONTEXT, RUNBOOK) plus `adr/`; the method/formats live in `gg-shared/`
(METHOD.md + FORMATS.md), read from the plugin and never copied into projects. **History is git's**:
no journals, archives, or changelogs — applied/consumed record blocks are deleted, and gg may commit
its own record per the project's `commit: ask|auto|never` policy (never push). There is no runtime
and no state.json by design.
