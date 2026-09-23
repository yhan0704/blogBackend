# Requirements

> 🚧 **Draft (mock)** — not finalized yet. Will be revised while building the frontend.
> Related issue: #1 Requirements analysis

---

## 1. Service Definition

**A Medium-style blog platform.**
Anyone can sign up, write posts, read others' posts, and interact through comments, likes, and follows.
Admins can handle reported posts/comments and suspend problematic users.

## 2. User Roles

| Role | Description | Permissions |
|---|---|---|
| Guest | Visitor who is not logged in | Read published posts, browse/search, view profiles |
| USER | Registered user | Guest permissions + write posts, comment, like, bookmark, follow, report |
| ADMIN | Operator | USER permissions + delete others' posts/comments, handle reports, suspend users |

## 3. Features

Priority: **P0** not a blog without it / **P1** needed to be usable / **P2** nice to have

| # | Feature | Description | Priority | Milestone |
|---|---|---|---|---|
| 0 | Sign up / Log in | Email + password | P0 | 2 |
| 1 | Post CRUD | Create, read, update, delete | P0 | 3 |
| 2 | Post list | Newest first, pagination | P0 | 5 |
| 3 | Post detail | Title, body, author, date | P0 | 3 |
| 4 | Draft / Publish | Separate draft and published states | P0 | 6 |
| 5 | Editor | Markdown | P0 | (frontend) |
| 6 | Tags | Post ↔ Tag N:M, browse posts by tag | P1 | 8 |
| 7 | Comments | Post 1 : Comment N | P1 | 7 |
| 8 | Likes | Once per user per post | P1 | 9 |
| 9 | Profile | Bio, avatar, list of the user's posts | P1 | 10 |
| 10 | Follow | Subscribe to other authors | P2 | 10 |
| 11 | Bookmarks | Save for later | P2 | 9 |
| 12 | Search | By title / body / tag | P2 | 12 |
| 13 | View count | +1 when a post detail is opened | P2 | 12 |
| 14 | Image upload | Cover image, images in body | P1 | 11 |
| 15 | Slug URL | `/posts/my-first-post` | P2 | 12 |
| 16 | Preview link | Share a post before publishing | P2 | (TBD) |
| 17 | Notifications | Comments/likes on my posts | P2 | 13 |
| 18 | Reports / Moderation | Report → handled by ADMIN | P2 | 14 |
| 19 | User suspension | ADMIN only | P2 | 14 |

## 4. Business Rules

Each rule is recorded as **decision / reason**.

### Users
- Email must be unique.
  - Reason: used as the login ID.
- Passwords are stored hashed. Never store plain text.
- Username must be unique → used for the profile URL `/@username`.

### Posts
- Only the **author** can edit. The **author or an ADMIN** can delete.
- New posts start as **draft**. They become published only when the author publishes them.
- Draft posts are visible **only to the author**.
- A published post can be reverted to draft.
- Deleting does not remove the row; it is only marked as deleted (soft delete).
  - Reason: keep a record for handling reports.

### Post list
- Show only published posts, newest first.
- 10 posts per page.

### Comments
- Only logged-in users can comment.
- Replies go **one level deep only** (reply to a comment: yes, reply to a reply: no).
- A deleted comment is shown as "This comment has been deleted" (it may have replies).

### Tags
- Up to 5 tags per post.
- Stored in lowercase (`Spring` = `spring`).

### Likes / Bookmarks
- A user can like a post once. Clicking again cancels it.
- Bookmarks work the same way.

### View count
- +1 every time the post detail is opened (deduplication later).

### Admin
- When a report comes in, an ADMIN resolves it as **hide / delete / dismiss**.
- Suspended users can log in but cannot write posts or comments.

## 5. Out of Scope
- Social login (Google, GitHub, etc.)
- Paid subscriptions / payments
- Real-time notifications (initially visible on refresh)
- Multi-language support

## 6. Open Questions (TODO)
- [ ] Tech stack (backend / DB / frontend)
- [ ] Session vs token for keeping users logged in
- [ ] Where to store images (server folder vs cloud storage)
- [ ] Which milestone the preview link belongs to
- [ ] Search approach (simple contains search vs full-text search)
