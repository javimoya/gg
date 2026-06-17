# CLAUDE.md — gg

## Releasing a version
- Bump `version` in `.claude-plugin/plugin.json` and add a `## X.Y.Z` entry to `CHANGELOG.md`.
- Commit to `main` (repo convention: commit title `X.Y.Z — summary`), then `git push origin main`.
- Annotated tag: `git tag -a vX.Y.Z -F -` then `git push origin vX.Y.Z`.
- **`gh` is NOT installed here and there is no `GH_TOKEN`/`GITHUB_TOKEN`** — don't investigate gh/the
  API. I can't publish the GitHub Release; pushing the tag is the deliverable. The user publishes the
  Release via the web (`https://github.com/javimoya/gg/releases/new?tag=vX.Y.Z`) or their own `gh`.

## Changing a command or format
When you change a command or a `gg-shared/` format, write **only the clean, going-forward behavior**.
**Never add migration, back-fill, or backward-compat logic to "fix up" projects created under an older
version** — no "back-fill the id for items that lack one", no "if the old field is missing, default
it", no version-detection branches. A command spec describes **one** behavior, not a version history. A
`.gg/` that predates a change is the user's to reconcile by hand (or by re-running the relevant
command); the plugin never silently repairs it. This mirrors gg's own constitution — in `dev` there is
nothing real to migrate: recreate, don't shim. The same rule applies to `/gg:orient --audit`: it may
**report** drift, but it never assumes older projects must be upgraded.

## What gg is
A pure-Markdown command plugin: six commands (`ideate`, `discover`, `next-task`, `refine-backlog`,
`capture`, `orient`) that build a product in **phase 0** then refine it **phase by phase**. A "phase"
is one `discover → next-task*` cycle; between phases, `refine-backlog` triages the captured backlog
(`.gg/BACKLOG.md`) — one reviewed report, then a single decision — into the next phase. All project state lives in each project's
`.gg/`; the shared protocols/formats live in `gg-shared/`. There is no runtime and no state.json by
design.
