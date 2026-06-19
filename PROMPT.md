# The debug step

Paste everything between the rules to the agent. This is the canonical
bug-hunt prompt.

---

I want you to go through the full flow (signup, login, link with Plaid (or not),
answer questions, change assumptions etc.) and find bugs.

When you find a bug, record a gif of the bug. Once you have fixed the bug,
reproduce the same bug not happening in a ui e2e test (e2e tests are already
setup). Then record the UI of that e2e test as a gif. The before and after gifs
should be in the PR for the bug so that non-technical people can review the bug
and see that it's fixed.

You can use subagents in different git workspaces to parallelize your
exploration + fixes. Workspaces can produce their own copies of the site by
running the conductor setup scripts.

---

## Definition of done (per bug)

- **One PR per bug**, on its own branch.
- A UI **e2e test that fails on the buggy build and passes on the fix**, committed
  under `e2e/` (the Playwright harness is already set up — match its conventions).
- **`before.gif`** (the bug reproducing) and **`after.gif`** (the fix verified)
  embedded **inline in the PR body** — not as links. A non-technical reviewer
  should be able to scroll the PR and see both.
- A plain-language description: what the user sees go wrong, the root cause, and
  the minimal fix.

## Notes

- Prefer bugs on **deterministic surfaces** (forms, inputs, toggles, navigation)
  for stable e2e and clean GIFs. LLM-driven chat is slow and flaky — only build
  e2e around it when the bug is genuinely there.
- Keep fixes **minimal and atomic**. One bug, one fix, one PR.
- The exact recording + inline-embed mechanics are in
  [`docs/METHODOLOGY.md`](docs/METHODOLOGY.md). Read it before recording.
