# Sequence Diagrams — Post

> 🚧 **Draft** — P0 only. ③ done, ④–⑦ in progress.
> Related issue: 3. Sequence diagrams

Solid arrow (→) = request, dashed arrow (⇢) = response. `alt` boxes show failure cases.

---

## ③ Create a post

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
TBD

## ⑤ Post list
TBD

## ⑥ Post detail
TBD

## ⑦ Publish
TBD
