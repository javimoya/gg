# Your first project with gg

In this tutorial we will build and ship a small product with gg — a one-page dice-roller web
app — from an empty folder to a landed change. Along the way we will meet gg's kickoff grilling,
the seeded record, and the `/gg:go` loop that turns one ask into one commit.

You need [Claude Code](https://claude.com/claude-code) installed and a terminal. Nothing else —
the product we build is a single HTML file, so there is no toolchain to set up. Expect the whole
run to take ten to fifteen minutes, most of it watching the agent work.

Because gg drives a live AI session, the *exact* words you see will differ from run to run. What
never differs is the shape: the questions, the files, and the commits land the same way every
time — and those are what we are here to learn.

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

Now type `/gg` — you should see three commands offered: `new`, `go`, `tidy`. If you don't, the
install didn't finish; run `/plugin` and check that `gg` is enabled.

## 3. Kick off the product

Run `/gg:new` and, when it asks what we're building, give it exactly this:

```
A single-file web page that rolls two dice with a button, shows the total,
and keeps a running history of rolls. No build tools — one index.html
I can open in a browser.
```

Now the **grilling** starts: gg asks one sharp question at a time — should the history persist
across reloads? is there a roll animation? — and every question leads with its recommendation.
For this first run, accept the recommendations: just answer "ok" each time. We are here to watch
the machine work; steering it comes naturally on your second project.

When it offers `git init`, say yes — git is gg's only history.

## 4. Look at what appeared

The kickoff ends with a commit and a concrete recommendation for the first slice to build.
Before following it, look at the `.gg/` folder that appeared:

- **`DESIGN.md`** — open it. The `## Product` section at the top is your product in a few lines,
  including what it is *not*. Below it, the design the grilling pinned.
- **`CONTEXT.md`** — the words your project uses (and the synonyms to avoid).
- **`RUNBOOK.md`** — how to run and verify it.
- **`BACKLOG.md`** — probably near-empty, with an id counter. Every idea you ever mention will
  get a stable `B-NN` block here, in two lines, without derailing whatever is being built.

These four files are how every future session — today or in three months — knows your project.

## 5. Build the first slice

Do what the breadcrumb recommends — something like:

```
/gg:go the page: two dice, the roll button, the total
```

Watch the rhythm: it orients from the record in seconds, builds, verifies (open the page
yourself when it invites you to — done means *observed*), and lands **one commit** whose title
cites the work. That commit carries the code *and* any record edits together — that's the core
gg habit: the record never drifts from the product, because they land in the same commit.

Repeat with the next slice (the roll history), and you've shipped the product.

## 6. Live with it — the loop

Roll the dice a few times. Something will itch — say the die faces are boring. That itch is the
whole workflow now:

```
/gg:go nicer die faces — proper pips, not numbers
```

A tweak like this gets **zero questions**. A real feature gets a short grilling. A bug gets a
failing test before any theory. Either way it ends the same: one commit, code + record together.

And if mid-build you have an idea you *don't* want now — "someday, sound effects" — just say it:
it becomes `B-01` in the backlog, two lines, and the work continues. Weeks later,
`/gg:go B-01` picks it up with full context.

## 7. What you have learned

One grilled kickoff, then a loop: bring one thing, land one commit. The record on disk stays
small and true; git holds every story. When months of work eventually make the record heavy,
`/gg:tidy` reports what went stale and prunes it on your yes — that's the entire maintenance
surface of gg.

More than the dice roller, *that rhythm* is what you've learned — it is the same at every scale,
from this toy to a real product. The [README](README.md) has the full picture.
