---
name: bug-hunt
description: Walk the full user flow of a running web app like a real person, find bugs, fix them, and ship each fix as its own PR with before/after GIFs embedded inline. Use when asked to bug-hunt, QA-and-fix, or produce reviewable GIF-proof bug PRs for an app repo.
---

# bug-hunt

Go through the full user flow of this app (sign up, log in, the core features,
settings / configuration, and the obvious edge cases) and find bugs.

When you find a bug, record a GIF of the bug. Once you have fixed the bug,
reproduce the same bug **not** happening in a UI e2e test (the e2e harness is
already set up). Then record the UI of that e2e test as a GIF. The before and
after GIFs must be embedded **inline** in the PR for the bug, so non-technical
people can review the bug and see that it's fixed.

## Before you start — determine three things for this repo

1. **Env setup** — the command that brings up an isolated instance for a worktree.
2. **App URL** — how to reach the running app (e.g. `http://localhost:$PORT`).
3. **E2E** — the command that runs the UI e2e suite (Playwright, Cypress, …),
   which must be able to record video (needed to produce the GIFs).

## Parallelize across worktrees

Use subagents in different git worktrees to parallelize exploration and fixes.
Each worktree brings up its own isolated copy of the app by running the repo's
per-worktree environment setup (its own port, DB/state, and auth) so parallel
agents never collide. If the repo can't isolate environments per worktree, run
single-threaded.

## Definition of done (per bug)

- **One PR per bug**, on its own branch.
- A UI **e2e test that fails on the buggy build and passes on the fix**, committed
  alongside the existing e2e suite (match its conventions).
- **`before.gif`** (the bug reproducing) and **`after.gif`** (the fix verified)
  embedded **inline in the PR body** — not as links. A non-technical reviewer
  should be able to scroll the PR and see both.
- A plain-language description: what the user sees go wrong, the root cause, and
  the minimal fix.

## Notes

- Prefer bugs on **deterministic surfaces** (forms, inputs, toggles, navigation)
  for stable e2e and clean GIFs.
- Keep fixes **minimal and atomic**. One bug, one fix, one PR.

The exact GIF-recording and inline-embed mechanics are in **`METHODOLOGY.md`**,
bundled alongside this skill. Read it before recording.
