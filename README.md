# blogBackend

Backend for a Medium-style blog platform.

## Design Documents

1. [Requirements](docs/requirements.md)
2. [Milestones](https://github.com/yhan0704/blogBackend/milestones)
3. Sequence Diagrams (in progress)
   - [Auth](docs/sequence-diagrams/auth.md) — sign up, log in
   - [Post](docs/sequence-diagrams/post.md) — create, edit/delete, list, detail, publish/unpublish, my posts
4. ERD (TBD)
5. API Specification (TBD)

## Roadmap

Features are built in three rounds. Each round goes through design first (requirements → sequence diagrams → ERD → API spec), then code.

### 🟢 P0 — The core blog (current round)

| Feature | What it does |
|---|---|
| Sign up / Log in | Email + password. A successful login returns a JWT. |
| Post CRUD | Write, read, edit, and delete posts. |
| Post list | Published posts, newest first, page-number pagination. |
| Post detail | Title, body, author, date (published date if published). |
| Draft / Publish | Posts start as drafts and become public when published. |
| My posts | See my own drafts and published posts in one list. |

### 🟡 P1 — Make it feel like a blog

| Feature | What it does |
|---|---|
| Tags | Add tags to posts (e.g. `#spring`). |
| Comments | Comment on posts. |
| Likes | Like posts. |
| Profile | Bio and profile picture. |
| Image upload | Put images in posts. |
| Autosave (server) | Save in-progress writing to the server automatically. |

### 🔵 P2 — Run it like a real service

| Feature | What it does |
|---|---|
| Follow | Follow other writers. |
| Bookmarks | Save posts to read later. |
| Search | Search posts. |
| View count | Count how many times a post was viewed. |
| Slug URL | Readable URLs: `/posts/5` → `/posts/my-first-post`. Note: creating a post with a slug that is already taken must not reveal someone else's draft. |
| Preview link | Share a draft by link before publishing. |
| Notifications | Notify users about comments, likes, and follows. |
| Reports / Moderation | Report and manage harmful posts. |
| User suspension | Suspend users who break the rules. |
| Admin role | Admins can delete any post. |
| Email verification | Verify the email address at sign up. |
| Forgot password | Reset a forgotten password by email. |

New ideas go into P2 for now. P2 can be split into P3 later if it grows too large.
