---
name: prod-bug-report
description: Walk real production user flows like a user, find bugs, and write each one up as a reproducible bug report (steps / expected behavior / actual behavior). Report-only — no code changes, no fixes, no PRs to the app.
---

# prod-bug-report

Exercise the app's **production** user flows end to end, like a real person, and
write up every bug you hit as a structured, reproducible report.

This is **report-only**. You do not fix anything, change the app, or open code
PRs — you produce bug reports.

## Run it safely (this is production)

You're touching the live app and real infrastructure. Be a good citizen:

- Prefer **read-mostly** paths. Avoid destructive or irreversible actions
  (deletes, payments, sending real emails/notifications, state changes that other
  users would see) unless you've been explicitly cleared to.
- Use a **dedicated test account** — never act on real users' data.
- Don't spam: minimal repetitions, and clean up any test data you create.
- If a step would charge money, notify real people, or mutate shared/other-user
  data, **stop and note it in the report** instead of doing it.

## What to exercise

Walk the full set of production flows like both a first-time and a returning
user: sign up / log in, the core features, settings and configuration,
navigation, empty states, error states, and the obvious edge cases. Drive it
through a real browser and watch for: broken UI, wrong data or numbers, dead
buttons, console/network errors, misleading or confusing messaging, and anything
that wouldn't match a reasonable user's expectation.

## Bug report format (use exactly this)

For every bug, write a report in this format:

```
steps:
1. starting at the homepage, do the thing step 1
2. do the thing step 2
3. do the thing step 3

expected behavior: what should have happened
actual behavior: what actually happened
```

Rules for good reports:

- **Steps always start from the homepage** and are concrete enough that anyone
  can follow them to reproduce — name the exact buttons, fields, and links, and
  the exact input you used.
- Keep steps **minimal**: the shortest path that triggers the bug.
- `expected behavior` and `actual behavior` are **one line each**, specific and
  observable.
- **One report per bug.** When filing several, put a short title line above each.
- Attach a screenshot or short clip when it helps a non-technical reader see it.

## Output

Collect the reports in `reports/<date>-prod-bugs.md` (one file, all bugs), or
file each as a GitHub issue if the team uses issues. Lead with a one-line
summary: how many flows you exercised and how many bugs you found.
