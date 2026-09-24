# Requirements

> 🚧 **Draft** — covers **P0 only** for now. P1/P2 will be added in later revisions.
> Related issue: #1 Requirements analysis

---

## 1. Service Definition

**A Medium-style blog platform.**
Anyone can sign up, write posts, and read others' posts.

## 2. User Roles

| Role | Description | Permissions |
|---|---|---|
| Guest | Visitor who is not logged in | Read published posts (list, detail) |
| USER | Registered user | Guest permissions + write, edit, delete, and publish their own posts |

## 3. Features (P0)

| # | Feature | Description | Milestone |
|---|---|---|---|
| 1 | Sign up / Log in | Email + password | 2 |
| 2 | Post CRUD | Create, read, update, delete | 3, 4 |
| 3 | Post list | Newest first, pagination | 5 |
| 4 | Post detail | Title, body, author, date | 3 |
| 5 | Draft / Publish | Separate draft and published states | 6 |
| 6 | My posts | List of my own posts, including drafts (login required) | 6 |

**Milestone** = the [GitHub milestone](https://github.com/yhan0704/blogBackend/milestones) where the feature is built.

| # | Milestone | What gets built |
|---|---|---|
| 1 | P0 Design | Requirements, sequence diagrams, ERD, API spec for P0 features |
| 2 | Sign up / Log in | Sign up, log in, keep users logged in |
| 3 | Post CRUD | Create, read (detail), update, delete posts |
| 4 | Post ↔ Author | Link posts to their author, author-only edit/delete |
| 5 | Post list | List of posts with pagination |
| 6 | Draft / Publish | Draft state, publish, drafts visible only to the author, my posts list |

## 4. Business Rules

Each rule is recorded as **decision / reason**.

### Users
- Log in with **email + password**.
- A successful login returns a **JWT**. The frontend sends it with every request that needs a logged-in user.
  - Reason: already used in a previous project (todoFullstack), so it's familiar.
- Email must be unique.
  - Reason: used as the login ID.
- Passwords are stored hashed. Never store plain text.
  - Reason: even if the DB leaks, the original passwords stay unknown.
- Username must be unique.
  - Reason: shown as the author name, and later used for profile URLs.

### Posts
- Only logged-in users can write posts.
- Only the **author** can edit or delete their post.
- Post body is stored as **Markdown text**.
- New posts start as **draft**. They become published only when the author publishes them.
- Draft posts are visible **only to the author**.
- A published post can be reverted to draft.
- Post IDs are plain numbers (1, 2, 3...).
  - Reason: simplest. Readable URLs come later with slug URLs (P2).
- Someone else's **draft** always returns **404 "Post not found"** — for viewing, editing, deleting, and publishing.
  - Reason: don't reveal that the post exists. The server always checks this; hiding it from the list is not enough.
- Editing, deleting, or publishing someone else's **published** post returns **403 "Only the author can ..."**.
  - Reason: published posts are public anyway, so 403 reveals nothing new.
- The first time a post is published, `published_at` is recorded. Unpublishing and publishing again **keeps the original date**.
  - Reason: prevents bumping an old post to the top of the list by unpublishing and republishing. Medium works the same way.

### Post list
- Show only published posts, newest `published_at` first.
  - Reason: a draft written a month ago and published today should appear at the top, not be buried by its creation date.
- Page-number pagination: `page` (1, 2, 3...).
  - Reason: simplest, and the frontend can build either page buttons or infinite scroll on top of it.
- Posts per page: `size` = **10, 30, or 50**. Default **10**. Any other value is rejected.
  - Reason: prevent huge requests (e.g. `size=1000000`) that could overload the server.
- The response includes the total number of pages.

### My posts
- Login required. Shows only my own posts, both **draft and published**, each with its status.
  - Reason: without this, a saved draft can't be found again after leaving the page.
- Newest first by last update. Same `page` / `size` rules as the post list.

## 5. Later (not in this round)

Only names are listed here so nothing is forgotten. Details will be decided when we get there.

- **P1:** Tags, Comments, Likes, Profile, Image upload, Autosave (server)
  - Autosave note: when added, change the rule to "drafts may be empty; title and body are required only when publishing".
- **P2:** Follow, Bookmarks, Search, View count, Slug URL, Preview link, Notifications, Reports / Moderation, User suspension, Admin role, Email verification, Forgot password

## 6. Open Questions (TODO)
- [ ] Tech stack (backend / DB)
- [ ] Hard delete vs soft delete for posts
