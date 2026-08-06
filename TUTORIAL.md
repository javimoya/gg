# Your first project with gg

In this tutorial we will build and ship a small product with gg — a one-page dice-roller web
app — from an empty folder to a closed batch 0. Along the way we will meet gg's kickoff
grilling, the batch-0 board, the build loop, and the Try walk that closes a batch.

You need [Claude Code](https://claude.com/claude-code) installed and a terminal. Nothing else —
the product we build is a single HTML file, so there is no toolchain to set up. Expect the whole
run to take around twenty minutes, most of it watching the agent work.

Because gg drives a live AI session, the *exact* words you see will differ from run to run. What
never differs is the shape: the questions, the gates, the files and the breadcrumbs land the
same way every time — and those are what we are here to learn.

## 1. Start clean

Make an empty folder and open Claude Code inside it:

```
mkdir dice-roller
cd dice-roller
claude
```

We start empty so that nothing in the folder can distract the kickoff.

## 2. Install gg

Inside Claude Code, run:

```
/plugin marketplace add javimoya/gg
/plugin install gg@javimoya
```

Now type `/gg` — you should see five commands offered: `new`, `plan`, `go`, `fix`, `where`. If
you don't, the install didn't finish; run `/plugin` and check that `gg` is enabled.

## 3. Kick off the product

Run `/gg:new` and, when it asks what we're building, give it exactly this:

```
A single-file web page that rolls two dice with a button, shows the total,
and keeps a running history of rolls. No build tools — one index.html
I can open in a browser.
```

Now the **grilling** starts: gg asks one sharp question at a time — should the history persist
across reloads? is there a roll animation? — and every question leads with its recommendation.
For this first run, accept the recommendations: just answer "ok" each time. We are here to
watch the machine work; steering it comes naturally on your second project.

Notice that gg *also* writes down the questions it decided **not** to ask you — numbered
assumptions you can veto later. That paper trail is the point of the kickoff.

## 4. Answer the one-time policies

Near the end of the kickoff, gg asks its one-time configuration questions. Answer:

- **Commit policy** — `ask`
- **git init** — yes
- **Deploy policy** — `ask` (our product has no deploy; the answer just gets recorded)

## 5. Read the sign-off

gg now presents one consolidated summary: the vision in a paragraph, the design calls, the
assumptions it took, and the **batch-0 board** — the whole product cut into rows. Notice it does
not ask you to approve: the batch is already open, and the summary ends in an open veto window.
You could change anything here — nothing needs changing, so don't answer and move on.

The session ends with a breadcrumb that should look something like:

```
Batch 0 designed: 4 rows on the board. Next: /clear then /gg:go.
```

Before moving on, look at what appeared: a `.gg/` folder. Open `.gg/WORK.md` — that's the
board gg just opened, with one row marked as next. This file is how every future session
knows exactly where we are.

## 6. Build the board

Do what the breadcrumb says: run `/clear`, then `/gg:go`.

Watch the session orient itself — it reads `WORK.md`, announces which row it's building, builds
it with tests, and checkpoints. Each stop ends with a breadcrumb naming the next step. Repeat
the rhythm — `/clear`, then `/gg:go` — until the last row.

Notice that every session started from zero context and still knew exactly where it was. No
re-explaining, no drift: that's the checkpoint doing its job.

## 7. Walk the Try list — the close

On the last row, gg runs the full suite and then stops and hands you the **Try list**: the
product's load-bearing flows, by name. For us that means: open `index.html` in your browser,
click **Roll**, watch the dice land, roll a few more times, check the history grows.

Do it for real — the close waits for your verdict, and the verdict must come from your own
eyes, not the agent's claim. Then tell it what you saw. If you allowed commits, gg proposes the
close commit; say yes.

## 8. What you have built

You have shipped a working product with a design on disk, a numbered trail of every default the
AI took, tests, and a git history that can answer "why is it like this?" — and you have felt
the whole gg rhythm: one grilled kickoff, then breadcrumb → `/clear` → next command, until the
Try walk closes the batch.

More than the dice roller, *that rhythm* is what you've learned — it is the same at every scale,
from this toy to a real product.

Want to keep going? Roll the dice a few times, collect what you'd change — a nicer die face, a
"clear history" button — and bring the list to `/gg:plan`: that's batch 1, and it works exactly
like what you just did, minus the kickoff. The [README](README.md) has the full picture of when
to run each command.
