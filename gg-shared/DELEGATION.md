# DELEGATION.md — delegated execution (shared)

How a `/gg:go` session runs a batch whose WORK header says `exec: delegated`: it **orchestrates**
row agents instead of building one row itself. Read by that session only — never by row agents
(theirs is `ROW-BRIEF.md`), never copied into a project. Everything in `METHOD.md` still binds;
this file adds only the mechanics of building rows through agents.

## The shape

One live session — the **orchestrator** — owns the batch. It launches one subagent per `by: agent`
row, each in its own git worktree; builds the `by: session` rows itself (go.md §3); integrates each
agent's work as it lands (merge → merged-tree suite → diff review → record); and checkpoints WORK
after every integration, so `/clear` + `/gg:go` resumes the orchestration exactly as it resumes a
row. The board's `after` column is the dependency graph — "sequential" is just its degenerate
chain. **Only the orchestrator ever touches `.gg/`**: row agents return candidates; the
orchestrator mints ids (one `next-id:` counter, no races).

## Preconditions (plan set them; re-check cheaply)

A git repo, and `commit: ask | auto` — worktrees branch and merge commits. Under `ask`, take
**one** explicit yes at the first launch: *"delegated execution merges each row's branch into
{branch} as it lands — one yes covers this batch's merges"*; record commits still follow the
policy at each checkpoint. Preconditions not met → say so and run the batch as `solo` (go.md,
unchanged), reporting the one-line `exec:` header fix.

## Launch

- **Ready** = `pending`, every `after` row `done`, and no show row awaiting its look (go.md §4a —
  while a look is owed, nothing new launches). Launch every ready `by: agent` row: branch
  `gg/b{N}-r{K}` **from the current integrated HEAD at launch time** — a stale base is the known
  trap — then `git worktree add` on that branch.
- **The brief is the agent's whole context** — it never orients, never reads `.gg/`. Compose it
  from what your orient already loaded: the row (item + done-when verbatim, its `B-NN` block's
  load-bearing detail); the pins — the `DESIGN.md` sections / ADR excerpts it builds against, the
  `CONTEXT.md` terms it must use, the `A-NN`s it must respect; the RUNBOOK commands it needs
  (focused check — never the deploy/destructive ones); the **decided-vs-open list** — what plan
  sealed (execute, don't re-ask) and what is still open (stop and ask); the worktree path, branch,
  and commit shape (`b{N}-r{K}: {summary}`); and the instruction to read
  `${CLAUDE_PLUGIN_ROOT}/gg-shared/ROW-BRIEF.md` first. Model per the row's `by` cell.
- A `by: session` row that comes ready is built by the orchestrator itself per go.md §3, at its
  place in the graph — checkpoint before starting it (it is the expensive kind).

## While agents run

- **Relay questions, don't absorb them.** An agent that stopped with `status: question` gets its
  questions put to the user **now**, each with the agent's recommendation, explained in plain
  terms (METHOD.md → Grilling). Send the answers back to the **same agent** — its context is
  intact; never relaunch fresh to deliver an answer. Other lanes keep running while one waits.
- **Questions batch, answers route.** An agent may return several questions in one stop, and
  several agents may be waiting at once: relay them as **one** card — each question named by its
  row, with its recommendation — never a ping per agent. Then route each answer set back to the
  agent that asked it, one send per agent carrying that agent's whole set: answer sets never mix,
  and a batch card never blocks the lanes that aren't waiting.
- Mid-flight captures and folds route as always (METHOD.md → Capture; go.md §3a) — a folded row
  lands with its `after`/`by` and launches when ready.

## Integrate (per landed row, in completion order — the row's real gate)

1. **Merge, uncommitted**: `git merge --no-commit {branch}`. Textual conflicts: resolve trivial
   ones yourself; real ones → `git merge --abort` and bounce back to the agent with the current
   HEAD (*"merge {sha}, re-run your check, recommit"*).
2. **Full suite on the merged tree — green before anything lands.** This is the only place
   cross-row drift shows (two rows green in isolation, red together): fix small drift in place;
   structural drift → `git merge --abort`, bounce to its agent. The batch branch never carries a
   red merge — an integration that can't go green now leaves no commit at all. A slow suite makes
   this guard expensive — say the cost out loud; never skip the guard.
3. **Review the diff against the done-when and the bar** — you are the one with full context:
   sweep for TODO/stub/mock leftovers, read the agent's self-accounting, name anything built that
   no row asked for (METHOD.md → Evidence).
4. **Route the report's candidates**: mint the `A-NN`s (decisions the agent took), `F-NN`s,
   `B-NN`s. An agent's decision is reported like your own — veto-style, with its why and way back
   (METHOD.md → Veto, not go-ahead).
5. **Record, then land it all as ONE commit**: row → `done`, Owned paths extended, the row's
   Delegations entry cleared, "Where to resume" updated; stage `.gg/` on top of the merge index
   and commit the merge and the record together, per policy (`gg(b{N}): row {K} — …`). One
   integration, one commit — never a merge commit and a record commit sharing a message.
6. **Clean up**: `git worktree remove` + delete the merged branch. Never leave debris.

Report per integration — one compact card (what landed, suite delta, decisions routed) — never per
agent event.

## Delegations — the in-flight ledger

Every launch writes its line in WORK "Where to resume → Delegations" (`row {K} → {branch}`); every
integration clears it. It is also the crash ledger: a session that orients into `exec: delegated`
with Delegations listed but no live agents inspects each branch — work that meets its done-when
(focused check passes) is integrated above; incomplete work is reported with the default read
**relaunch fresh from current HEAD**, the stale branch deleted only on a yes (it is committed
work — METHOD.md → Safety).

## What never changes

Shows are never delegated (`by: session` — the look is the user's; go.md §4a). Outward and
irreversible actions are the orchestrator's alone, per the policies — an agent that hits one stops
and returns it. The batch close is go.md §5, unchanged: the merged suite, the self-accounting, the
user's Try walk gating the sweep. And the bar is the bar: a delegated row ships at full quality or
comes back — delegation changes who types, never what "done" means.
