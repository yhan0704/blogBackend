# Sequence Diagrams — Post

> 🚧 **Draft** — P0 only.
> Related issue: 3. Sequence diagrams

Solid arrow (→) = request, dashed arrow (⇢) = response. `alt` boxes show branches. A response to the Frontend (⇢) ends the request; `✅ Continue` means the flow keeps going.

---

## ③ Create a post (saved as draft)

```mermaid
sequenceDiagram
    actor User
    participant Frontend
    participant Server
    participant DB

    User->>Frontend: Enter title, body and click "Save"
    Frontend->>Server: Title, body + JWT
    Server->>Server: Verify JWT (who is this?)
    alt JWT missing or expired
        Server-->>Frontend: ❌ "Login required"
    end
    Server->>Server: Are title and body empty?
    alt Title or body is empty
        Server-->>Frontend: ❌ "Title and body are required"
    end
    Server->>DB: Save post (author = me, status = draft)
    DB-->>Server: Saved (post id)
    Server-->>Frontend: Post id
    Frontend-->>User: Post created
```

## ④ Edit / delete a post

### Edit

```mermaid
sequenceDiagram
    actor User
    participant Frontend
    participant Server
    participant DB

    User->>Frontend: Edit and click "Save"
    Frontend->>Server: Post id, title, body + JWT
    Server->>Server: Verify JWT
    alt JWT missing or expired
        Server-->>Frontend: ❌ "Login required"
    end
    Server->>DB: Find the post with this id
    DB-->>Server: Post
    alt Post not found
        Server-->>Frontend: ❌ "Post not found"
    end
    Server->>Server: Is the author me?
    alt Not the author, post is a draft
        Server-->>Frontend: ❌ "Post not found" (don't reveal it exists)
    else Not the author, post is published
        Server-->>Frontend: ❌ "Only the author can edit this post"
    else I am the author
        Server->>Server: ✅ Continue
    end
    Server->>Server: Are title and body empty?
    alt Title or body is empty
        Server-->>Frontend: ❌ "Title and body are required"
    end
    Server->>DB: Update post
    DB-->>Server: Updated
    Server-->>Frontend: Edit success
    Frontend-->>User: Post updated
```

### Delete

```mermaid
sequenceDiagram
    actor User
    participant Frontend
    participant Server
    participant DB

    User->>Frontend: Click "Delete"
    Frontend->>Server: Post id + JWT
    Server->>Server: Verify JWT
    alt JWT missing or expired
        Server-->>Frontend: ❌ "Login required"
    end
    Server->>DB: Find the post with this id
    DB-->>Server: Post
    alt Post not found
        Server-->>Frontend: ❌ "Post not found"
    end
    Server->>Server: Is the author me?
    alt Not the author, post is a draft
        Server-->>Frontend: ❌ "Post not found" (don't reveal it exists)
    else Not the author, post is published
        Server-->>Frontend: ❌ "Only the author can delete this post"
    else I am the author
        Server->>Server: ✅ Continue
    end
    Server->>DB: Delete post
    DB-->>Server: Deleted
    Server-->>Frontend: Delete success
    Frontend-->>User: Post deleted
```

## ⑤ Post list

No login needed.

```mermaid
sequenceDiagram
    actor User
    participant Frontend
    participant Server
    participant DB

    User->>Frontend: Open post list
    Frontend->>Server: page, size
    Server->>Server: Is size 10, 30, or 50? (default 10)
    alt Invalid size
        Server-->>Frontend: ❌ "size must be 10, 30, or 50"
    end
    Server->>DB: Published posts only, newest published_at first, this page
    DB-->>Server: Posts + total count
    Server-->>Frontend: Posts, total pages
    Frontend-->>User: Show post list
```

## ⑥ Post detail

No login needed. JWT is sent if the user is logged in (needed to see their own drafts).

```mermaid
sequenceDiagram
    actor User
    participant Frontend
    participant Server
    participant DB

    User->>Frontend: Click a post
    Frontend->>Server: Post id (+ JWT if logged in)
    Server->>DB: Find the post with this id
    DB-->>Server: Post + author username
    alt Post not found
        Server-->>Frontend: ❌ "Post not found"
    end
    alt Draft and requester is not the author
        Server-->>Frontend: ❌ "Post not found" (don't reveal it exists)
    end
    Server-->>Frontend: Title, body, author, published date
    Frontend-->>User: Show post
```

## ⑦ Publish / unpublish

### Publish

```mermaid
sequenceDiagram
    actor User
    participant Frontend
    participant Server
    participant DB

    User->>Frontend: Click "Publish"
    Frontend->>Server: Post id + JWT
    Server->>Server: Verify JWT
    alt JWT missing or expired
        Server-->>Frontend: ❌ "Login required"
    end
    Server->>DB: Find the post with this id
    DB-->>Server: Post
    alt Post not found
        Server-->>Frontend: ❌ "Post not found"
    end
    Server->>Server: Is the author me?
    alt Not the author, post is a draft
        Server-->>Frontend: ❌ "Post not found" (don't reveal it exists)
    else Not the author, post is published
        Server-->>Frontend: ❌ "Only the author can publish this post"
    else I am the author
        Server->>Server: ✅ Continue
    end
    alt Already published
        Server-->>Frontend: ✅ Publish success (nothing changed, stop)
    end
    opt First time being published (published_at is empty)
        Server->>Server: Set published_at = now
    end
    Server->>DB: Set status = published
    DB-->>Server: Updated
    Server-->>Frontend: Publish success
    Frontend-->>User: Post published
```

### Unpublish

`published_at` is **not** cleared, so publishing again keeps the original date.

```mermaid
sequenceDiagram
    actor User
    participant Frontend
    participant Server
    participant DB

    User->>Frontend: Click "Unpublish"
    Frontend->>Server: Post id + JWT
    Server->>Server: Verify JWT
    alt JWT missing or expired
        Server-->>Frontend: ❌ "Login required"
    end
    Server->>DB: Find the post with this id
    DB-->>Server: Post
    alt Post not found
        Server-->>Frontend: ❌ "Post not found"
    end
    Server->>Server: Is the author me?
    alt Not the author, post is a draft
        Server-->>Frontend: ❌ "Post not found" (don't reveal it exists)
    else Not the author, post is published
        Server-->>Frontend: ❌ "Only the author can unpublish this post"
    else I am the author
        Server->>Server: ✅ Continue
    end
    alt Already a draft
        Server-->>Frontend: ✅ Unpublish success (nothing changed, stop)
    end
    Server->>DB: Set status = draft (keep published_at)
    DB-->>Server: Updated
    Server-->>Frontend: Unpublish success
    Frontend-->>User: Post is a draft again
```

## ⑧ My posts

Login required. Shows my own posts, both draft and published.

```mermaid
sequenceDiagram
    actor User
    participant Frontend
    participant Server
    participant DB

    User->>Frontend: Open "My posts"
    Frontend->>Server: page, size + JWT
    Server->>Server: Verify JWT
    alt JWT missing or expired
        Server-->>Frontend: ❌ "Login required"
    end
    Server->>Server: Is size 10, 30, or 50? (default 10)
    alt Invalid size
        Server-->>Frontend: ❌ "size must be 10, 30, or 50"
    end
    Server->>DB: My posts (draft + published), newest update first, this page
    DB-->>Server: Posts + total count
    Server-->>Frontend: Posts (with status), total pages
    Frontend-->>User: Show my posts
```
