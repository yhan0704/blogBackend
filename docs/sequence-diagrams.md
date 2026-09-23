# Sequence Diagrams

> 🚧 **Draft** — P0 only. ①–③ done, ④–⑦ in progress.
> Related issue: 3. Sequence diagrams

Solid arrow (→) = request, dashed arrow (⇢) = response. `alt` boxes show failure cases.

---

## ① Sign up

```mermaid
sequenceDiagram
    actor User
    participant Frontend
    participant Server
    participant DB

    User->>Frontend: Click "Sign up"
    Frontend->>Server: Email, password, username
    Server->>DB: Does this email exist?
    DB-->>Server: Result
    alt Email already exists
        Server-->>Frontend: ❌ "Email is already registered"
    end
    Server->>DB: Does this username exist?
    DB-->>Server: Result
    alt Username already exists
        Server-->>Frontend: ❌ "Username is already taken"
    end
    Server->>Server: Hash password
    Server->>DB: Save user
    DB-->>Server: Saved
    Server-->>Frontend: Sign-up success
    Frontend-->>User: Show "Sign-up complete"
```

## ② Log in

```mermaid
sequenceDiagram
    actor User
    participant Frontend
    participant Server
    participant DB

    User->>Frontend: Click "Log in"
    Frontend->>Server: Email, password
    Server->>DB: Find the user with this email
    DB-->>Server: User info
    Server->>Server: Check password
    alt Email not found or wrong password
        Server-->>Frontend: ❌ "Email or password is incorrect"
    end
    Server->>Server: Create JWT
    Server-->>Frontend: JWT
    Frontend->>Frontend: Store JWT
    Frontend-->>User: Logged in
```

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
