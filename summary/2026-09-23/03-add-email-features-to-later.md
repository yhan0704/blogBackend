# Commit pending — Add email verification and forgot password to the later list

## Why
- While drawing the login sequence diagram, "find member by email" (a DB lookup) was confused with features that actually send emails.
- Email verification and forgot password both need a mail service, stored codes, and expiration handling, so they are out of P0.

## What changed
- `docs/requirements.md`: added `Email verification` and `Forgot password` to the P2 list in "5. Later". Names only; details will be decided later.
