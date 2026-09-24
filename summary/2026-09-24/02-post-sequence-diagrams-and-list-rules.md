# Commit pending — Complete post sequence diagrams and add list/ID rules

## Why
- Finish the P0 post flows (④–⑦) that were left as TBD.
- Decisions made while drawing them:
  - Someone else's draft could still be opened by typing its URL, so the server must check it and answer 404.
  - Considered UUIDs for post IDs; kept plain numbers because the server check is what actually protects drafts, and numbers are simpler.
  - Chose page-number pagination; the frontend can build page buttons or infinite scroll on top of it.
  - Page size comes from a `size` parameter, limited to 10/30/50 to prevent huge requests.

## What changed
- `docs/sequence-diagrams/post.md`: added ④ Edit / Delete, ⑤ Post list, ⑥ Post detail, ⑦ Publish / unpublish.
- `docs/requirements.md`:
  - Posts: post IDs are plain numbers; someone else's draft returns 404.
  - Post list: `page` pagination, `size` = 10/30/50 (default 10), response includes total pages.
