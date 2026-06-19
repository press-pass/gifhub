# press-bot

An autonomous QA bug-hunt harness for a running web app.

You point a coding agent at the app; it walks the **entire user flow like a real
person** — sign up, log in, link a bank (or not), answer the advisor's questions,
change assumptions — and hunts for bugs. For every bug it finds it:

1. **records a GIF of the bug happening**,
2. **fixes it**,
3. **proves the fix with a UI end-to-end test** (the e2e harness is already set up),
4. **records a GIF of that e2e test passing**, and
5. **opens a PR for the bug with the before + after GIFs embedded inline**, so a
   non-technical reviewer can watch the bug, then watch it disappear — without
   reading a line of code.

Built for the **home-buyer** app — point the agent at a running copy of it.

## The debug step

The exact prompt to hand the agent lives in [`PROMPT.md`](PROMPT.md). It's also
wired up as a Claude Code slash command: [`/bug-hunt`](.claude/commands/bug-hunt.md).

The short version:

> Go through the full flow (signup, login, link with Plaid or not, answer
> questions, change assumptions, etc.) and find bugs. For each bug: record a GIF
> of it, fix it, reproduce the fix not-happening in a UI e2e test, record that
> e2e run as a GIF, and open a PR with the before + after GIFs inline so
> non-technical people can review.

## Parallelize it

Spawn subagents in **separate git worktrees / Conductor workspaces** to explore
and fix in parallel. Each workspace brings up its **own copy of the site** by
running the Conductor setup scripts (its own port, DB, and auth stack), so
agents never step on each other. One bug → one worktree → one PR.

## Quick start

```bash
# 1. bring up a copy of the home-buyer app (per worktree/workspace)
./provision-workspace.sh   # Conductor setup → unique port/DB/auth
npm run dev

# 2. hand the agent the debug step
#    paste PROMPT.md (or run /bug-hunt in Claude Code)
```

## How it actually works

The mechanics that make the GIFs reproducible and the e2e reliable are written
up in [`docs/METHODOLOGY.md`](docs/METHODOLOGY.md):

- one Playwright spec produces **both** GIFs — before (run against the buggy
  build, fails) and after (run against the fix, passes),
- GIFs embed **inline in the PR body even for private repos**, via committed
  `?raw=true` image URLs,
- favor **deterministic UI surfaces** for stable e2e (forms, inputs, toggles)
  over LLM-driven chat,
- **one bug per PR**, parallelized across worktrees.
