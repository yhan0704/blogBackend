# Commit pending — Add server autosave to P1 and clarify "save draft"

## Why
- Unsaved writing is lost if the user leaves the page or closes the browser.
- For P0 this is handled only in the frontend (warn before leaving + keep a local copy in the browser). The backend design stays the same.
- Server autosave (Medium style) is postponed to P1. It conflicts with the current "title and body are required" rule, so that rule must change when it's added.
- "Save draft" was hard to find because the flow was called "Create a post".

## What changed
- `docs/requirements.md`: added `Autosave (server)` to the P1 list, with a note about relaxing the empty title/body rule for drafts.
- `docs/sequence-diagrams/post.md`: renamed ③ to "Create a post (saved as draft)".
