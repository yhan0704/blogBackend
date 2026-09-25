# Commit pending — Mark authentication and authorization in post diagrams

## Why
- It was hard to tell which steps are "who are you?" and which are "are you allowed?" when following the diagrams by hand.
- Colored boxes make the two kinds of checks visible at a glance. No rules changed, but ⑥ Detail now shows the optional JWT check as its own step.

## What changed
- `docs/sequence-diagrams/post.md`
  - Legend explains 🔑 Authentication (blue, verify JWT) and 🛡️ Authorization (orange, may this user act on this post).
  - Notes that existence, input, and state checks are the same for everyone, so they are not authorization.
  - ③ Create, ⑧ My posts: authentication box only (any logged-in user can create; the query returns only my posts).
  - ④ Edit, ④ Delete, ⑦ Publish, ⑦ Unpublish: authentication box + authorization box around "Is the author me?".
  - ⑥ Detail: optional authentication box (`opt JWT was sent`) + authorization box around the draft check.
  - ⑤ List: notes there is no authentication or authorization.

## Discussed, not changed
- 404 vs 403 for others' drafts: with sequential IDs, 404 hides little. 403 would be simpler, but we keep 404 for now.

Review: fixed ⑧ query condition moved to the request arrow. Deferred: expired/invalid JWT on ⑥ Detail (same as M4, decide before coding sign up / log in).
