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

**Milestone** = the [GitHub milestone](https://github.com/yhan0704/blogBackend/milestones) where the feature is built.

| # | Milestone | What gets built |
|---|---|---|
| 1 | Design | Requirements, sequence diagrams, ERD, API spec |
| 2 | Sign up / Log in | Sign up, log in, keep users logged in |
| 3 | Post CRUD | Create, read (detail), update, delete posts |
| 4 | Post ↔ Author | Link posts to their author, author-only edit/delete |
| 5 | Post list | List of posts with pagination |
| 6 | Draft / Publish | Draft state, publish, drafts visible only to the author |

## 4. Business Rules

Each rule is recorded as **decision / reason**.

### Users
- Log in with **email + password**.
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

### Post list
- Show only published posts, newest first.
- 10 posts per page.

## 5. Later (not in this round)

Only names are listed here so nothing is forgotten. Details will be decided when we get there.

- **P1:** Tags, Comments, Likes, Profile, Image upload
- **P2:** Follow, Bookmarks, Search, View count, Slug URL, Preview link, Notifications, Reports / Moderation, User suspension, Admin role

## 6. Open Questions (TODO)
- [ ] Tech stack (backend / DB)
- [ ] Session vs token for keeping users logged in
- [ ] Hard delete vs soft delete for posts
