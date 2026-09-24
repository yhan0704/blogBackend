# Commit pending — Split sequence diagrams into auth and post

## Why
- One file for every flow gets long and hard to find things in. Splitting by feature keeps each file small; new features (comments, tags) can get their own file later.
- Named `auth` instead of `login` because it also contains sign up.

## What changed
- `docs/sequence-diagrams.md` removed; content moved unchanged into:
  - `docs/sequence-diagrams/auth.md`: ① Sign up, ② Log in
  - `docs/sequence-diagrams/post.md`: ③ Create a post, ④–⑦ TBD
- `README.md`: Sequence Diagrams now links to the two files.
