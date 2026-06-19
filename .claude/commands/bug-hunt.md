---
description: Walk the full app flow, find bugs, fix them, and ship each as a PR with before/after GIFs
---

Go through the full flow of the home-buyer app (signup, login, link with Plaid
or not, answer the advisor's questions, change assumptions, etc.) and find bugs.

When you find a bug, record a GIF of the bug. Once you have fixed the bug,
reproduce the same bug **not** happening in a UI e2e test (the Playwright e2e
harness in the app's `e2e/` directory is already set up). Then record the UI of that e2e
test as a GIF. The before and after GIFs must be embedded **inline** in the PR
for the bug, so non-technical people can review the bug and see that it's fixed.

Use subagents in different git worktrees / Conductor workspaces to parallelize
exploration and fixes. Each workspace produces its own copy of the site by
running the Conductor setup scripts (its own port, DB, and auth).

Definition of done per bug: one PR, on its own branch, with a failing-then-passing
e2e test committed, before+after GIFs inline in the PR body, and a plain-language
description of the symptom, root cause, and minimal fix.

Follow `docs/METHODOLOGY.md` for the exact GIF-recording and inline-embed
mechanics, and `PROMPT.md` for the full brief.
