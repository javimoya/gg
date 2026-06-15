# CLAUDE.md — gg

## Releasing a version
- Bump `version` in `.claude-plugin/plugin.json` and add a `## X.Y.Z` entry to `CHANGELOG.md`.
- Commit to `main` (repo convention: commit title `X.Y.Z — summary`), then `git push origin main`.
- Annotated tag: `git tag -a vX.Y.Z -F -` then `git push origin vX.Y.Z`.
- **`gh` is NOT installed here and there is no `GH_TOKEN`/`GITHUB_TOKEN`** — don't investigate gh/the
  API. I can't publish the GitHub Release; pushing the tag is the deliverable. The user publishes the
  Release via the web (`https://github.com/javimoya/gg/releases/new?tag=vX.Y.Z`) or their own `gh`.

## What gg is
A pure-Markdown command plugin: five commands (`ideate`, `discover`, `next-task`, `capture`,
`orient`) that build a product in **phase 0** then refine it **phase by phase**. A "phase" is one `discover → next-task*` cycle. All project state lives in each project's
`.gg/`; the shared protocols/formats live in `gg-shared/`. There is no runtime and no state.json by
design.
