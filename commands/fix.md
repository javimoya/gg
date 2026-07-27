---
description: "The fix-it-now lane — fix first, record after. For a small, decided, local change (a spurious bug, an operational failure, a one-screen tweak): locate it, fix it in this session with a pinning test when the defect class merits one, run the full suite green, and the record is ONE line in WORK's Fix log (plus a commit if the policy says so). Runs in any state, even mid-batch (it checks overlap with the in-flight row's owned paths first). Honesty valve: if the fix grows design weight, it stops, jots a B-NN, and offers the fold into the live batch (/gg:go) or routes to /gg:plan."
model: inherit
disable-model-invocation: true
argument-hint: "[what to fix]"
---

# /gg:fix — Fix now, record after

You fix **one small, decided, local thing in this session** — the lane for "arréglalo sobre la
marcha". No board, no design prose, no gate; the bar still holds (a real fix, a test when it merits
one, the suite green). You work in the project directory (cwd); state lives in `<cwd>/.gg/`. Method
and formats: `${CLAUDE_PLUGIN_ROOT}/gg-shared/METHOD.md` +
`${CLAUDE_PLUGIN_ROOT}/gg-shared/FORMATS.md` — read both now.

## 0. Precondition (light — this lane runs in any state)
- **No `.gg/`** → not a gg project; say so and route to `/gg:new` (or just help without gg).
- Read `WORK.md ## State`, `RUNBOOK.md`, and `NOTES.md ## Assumptions` (small by design — a defect
  is often a recorded default biting). **Mid-batch (`state: building`)**: check the fix's likely
  paths against the board row in flight and the Owned paths — **an overlap needs an explicit yes**
  before proceeding, and diff the overlapping file first: if it carries uncommitted changes the fix
  didn't make, they are not yours to touch or commit.
- The safety floor is unconditional (METHOD.md → Safety): an outward or destructive step still names
  its rollback and gets a yes; the user's dirty paths stay untouched.

## 1. Fix it
- First, note the intent in `WORK.md` "Where to resume → Notes" (*"fixing: {what}"* — cleared at §3)
  so an interrupted session leaves a trace, not unexplained dirty files.
- Locate the defect (`$ARGUMENTS` or what the user just showed you). Diagnose against the real code —
  read what you need; the open assumptions often point at the cause.
- Fix it **in this session**, at the bar: cover the case properly, no `// for now`. Add a **pinning
  test** when the defect class merits one (a logic bug: yes; a typo in a label: say why not and
  skip). Run the focused check, then the **RUNBOOK full suite** — **green, or red only where it was
  already red before the fix** (state which in the Fix log line).

## 2. The honesty valve
If mid-flight the "5-line fix" turns out to have design weight — it changes the shape, touches a
load-bearing seam, or keeps growing — **stop fixing**: jot it as a `B-NN` (METHOD.md → Capture) with
what you learned, restore any half-applied step, and offer the honest routes: **mid-batch, fold it
into the live batch** (`go.md` §3a — the usual answer to "arréglalo ya"); otherwise `/gg:plan`. The
lane is express because it's bounded, not because it skips the bar.

## 3. Record (one line) and close
- Append one line to `WORK.md ## Fix log`: `{date} — {what} — {test added?} — {suite result}`, and
  clear the §1 intent note. A fix that reversed a recorded default **deletes that `A-NN` block**
  (git keeps it) and says so in the Fix log line; an `F-NN` is recorded only if the fix *observed*
  something that changes live work. Nothing else — no board row, no batch, no other file beyond
  WORK and (in those two cases) NOTES.
- Commit per policy: `gg(fix): {what} — {root cause}` (pathspec-scoped: `.gg/` + the files the fix
  touched — never the user's dirty paths, never a file whose diff contains changes the fix didn't
  make).
- Breadcrumb: *"Fixed: {what} ({K} tests, {suite result}). Carry on."*
