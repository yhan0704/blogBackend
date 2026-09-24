# Commit pending — Hide others' drafts everywhere, add My posts, sort by published_at

## Why
Review of the post sequence diagrams found three gaps:
1. Detail returned 404 for someone else's draft, but edit/delete/publish returned 403 — which revealed the draft exists.
2. A saved draft could not be found again: the post list only shows published posts, and detail needs the post id.
3. "Newest first" had no defined date. Sorting by creation date buries a draft that was written long ago and published today.

## What changed
- `docs/requirements.md`
  - Someone else's draft → 404 for view/edit/delete/publish; someone else's published post → 403.
  - `published_at` is set on first publish and kept on unpublish/republish (prevents bumping old posts to the top).
  - Post list sorted by newest `published_at`.
  - New feature ⑥ "My posts" (login required, drafts + published, newest update first, same page/size rules); added to milestone 6.
- `docs/sequence-diagrams/post.md`
  - ④ Edit / Delete and ⑦ Publish: split "not the author" into draft → 404 / published → 403.
  - ⑤ Post list: sort by `published_at`. ⑥ Detail: returns published date.
  - ⑦ Publish: set `published_at` only if empty; unpublish keeps it.
  - Added ⑧ My posts.
