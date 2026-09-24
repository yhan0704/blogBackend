# Commit pending — Add a reviewer step before push

## Why
- Mistakes are hard to see in your own work. A separate reviewer that doesn't know the conversation reads only the files, like a coworker giving a second look.
- Earlier, an outside review caught real gaps (others' drafts revealed by 403, drafts impossible to find again, undefined "newest" order). This makes that kind of check part of every push.

## What changed
- `CLAUDE.md`: push procedure now has a review step before committing:
  - A separate reviewer agent checks for contradicting rules, docs that disagree, missing cases (auth, permission, input, existence, duplicates/state), and broken end-to-end flows; later also code bugs, security, and tests.
  - Problems are reported to the user in Korean and decided together, not fixed silently.
  - Tiny changes (typos, links) may skip it.
  - Each summary file records the result: `Review: passed` / `fixed <what>` / `skipped (<reason>)`.

Review: skipped (process rule only, no design content changed)
