# gifhub

An autonomous QA bug-hunt harness for **any web-app repo**.

Point a coding agent at your app; it walks the **entire user flow like a real
person**, hunts for bugs, and for every bug it finds it:

1. **records a GIF of the bug happening**,
2. **fixes it**,
3. **proves the fix with a UI end-to-end test**,
4. **records a GIF of that e2e test passing**, and
5. **opens a PR for the bug with the before + after GIFs embedded inline**, so a
   non-technical reviewer can watch the bug, then watch it disappear — without
   reading a line of code.

## Install as a Claude Code plugin

gifhub ships as a Claude Code plugin, so you can drop these skills into **any**
repo:

```
/plugin marketplace add press-pass/gifhub
/plugin install gifhub
```

That installs two skills — [`bug-hunt`](skills/bug-hunt/SKILL.md) (find + fix +
GIF PRs) and [`prod-bug-report`](skills/prod-bug-report/SKILL.md) (report-only) —
plus the [`/bug-hunt`](commands/bug-hunt.md) slash command. The skills
auto-trigger when you ask an agent to hunt for bugs. Run `/plugin update gifhub`
to pull changes. To try it without installing, from a clone:

```
claude --plugin-dir /path/to/gifhub
```

## Requirement: per-worktree environments

gifhub parallelizes by running subagents in **separate git worktrees**, each
with its **own isolated, running copy of the app**. So the one thing your repo
must provide is a way to **stand up an isolated environment per worktree** — a
command that boots the app on a unique port with isolated data / state / auth, so
parallel agents never collide.

[Conductor](https://conductor.build) provides this out of the box (per-workspace
provisioning); a plain `git worktree` plus your own setup script works too. If a
repo can't isolate environments per worktree, run gifhub single-threaded.

## Point it at your repo

Tell the agent three things (or bake them into the prompt):

1. **Env setup** — the command that brings up an isolated instance for a worktree.
2. **App URL** — how to reach the running app (e.g. `http://localhost:$PORT`).
3. **E2E** — the command that runs the UI e2e suite (Playwright, Cypress, …), and
   the fact that it can record video (needed to produce the GIFs).

## The debug step

The [`bug-hunt`](skills/bug-hunt/SKILL.md) skill is the full brief. With the
plugin installed it runs as the [`/bug-hunt`](commands/bug-hunt.md) slash command
or auto-triggers when you ask an agent to hunt for bugs. [`PROMPT.md`](PROMPT.md)
carries the same brief as a copy-paste prompt for agents without the plugin.

## Report-only mode

Want bug reports without fixes — e.g. run against **production** — instead of
fix-and-PR? The [`prod-bug-report`](skills/prod-bug-report/SKILL.md) skill
walks the live app's flows like a user and writes each bug up with reproduction
steps (`steps:` / `expected behavior:` / `actual behavior:`). No code changes; it
just produces reports.

## How it works

The mechanics that make the GIFs reproducible and the e2e reliable are in
[`METHODOLOGY.md`](skills/bug-hunt/METHODOLOGY.md), bundled with the `bug-hunt`
skill:

- one e2e spec produces **both** GIFs — before (run against the buggy build,
  fails) and after (run against the fix, passes),
- GIFs embed **inline in the PR body even for private repos**, via committed
  `?raw=true` image URLs,
- favor **deterministic UI surfaces** for stable e2e (forms, inputs, toggles)
  over async/AI-driven UI,
- **one bug per worktree → one PR**.
