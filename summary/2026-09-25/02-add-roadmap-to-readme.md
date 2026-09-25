# Commit pending — Add P0/P1/P2 roadmap to README

## Why
- P1 and P2 were only listed by name in `docs/requirements.md` ("Later"), so it was hard to tell what each feature means.
- The README is the first page on GitHub, so the full plan should be visible there.

## What changed
- `README.md`: new **Roadmap** section with P0, P1, and P2 tables. Each feature has a one-line description.
- Two notes from discussion were added:
  - Admin role: admins can delete any post.
  - Slug URL: creating a post with a taken slug must not reveal someone else's draft.
- There is no P3. New ideas go into P2 for now and can be split out later.
- `docs/requirements.md`: the same two notes added under P2, so requirements stays the single source of truth.

Review: fixed notes copied into requirements.md; Post detail says "date (published date if published)" because drafts have no published date.
