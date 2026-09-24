# Sequence Diagrams — Auth

> 🚧 **Draft** — P0 only.
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
