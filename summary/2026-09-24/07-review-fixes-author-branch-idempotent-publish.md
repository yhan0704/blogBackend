# Commit pending — Review fixes: author branch, idempotent publish, unpublish diagram

## Why
Fixes from the first reviewer pass that were cheap to do now:
- **L3**: the authorization `alt` blocks in ④ and ⑦ had no "I am the author" branch, so read literally the author was also blocked.
- **L5**: unpublish and "My posts" were missing from the roles table, the author-only rule, and the README.
- **M1**: publishing an already-published post (or unpublishing a draft) had no defined behavior. A double click or two tabs would hit this.

## What changed
- `docs/requirements.md`
  - Roles: USER can also unpublish and see their own posts list.
  - Posts: author-only rule covers publish/unpublish; 404/403 rules cover unpublish.
  - New rule: publish on a published post / unpublish on a draft succeeds without changes (idempotent).
- `docs/sequence-diagrams/post.md`
  - ④ Edit, ④ Delete, ⑦ Publish: added `else I am the author → continue`.
  - ⑦ Publish: "already published → success, nothing changed".
  - New ⑦ Unpublish diagram (keeps `published_at`; "already a draft → success").
- `README.md`: Post diagrams list includes publish/unpublish and my posts.

- `docs/sequence-diagrams/post.md`, `auth.md`: legend now says `alt` boxes are branches and a response to the Frontend ends the request; idempotent successes marked "stop".

Review: fixed legend (alt boxes are no longer only failures; marked idempotent successes as "stop")

Remaining review items are deferred: M2/M3/L4/L6 → ERD, M5/L1/L2/L7 → API spec, M4 → before coding sign up / log in.
