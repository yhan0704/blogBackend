# Commit pending — Move the reviewer into a dedicated agent file

## Why
- The reviewer was described in prose inside `CLAUDE.md`, so its instructions were rewritten on every push and could drift.
- A dedicated agent file gives a fixed prompt, and tool access is limited so it can't edit files.

## What changed
- New `.claude/agents/backend-reviewer.md`:
  - Role: senior backend engineer reviewing a junior's design and code.
  - Checklist: contradicting rules, documents that disagree, missing cases, broken end-to-end flows, real-service risks, code issues once code exists, Mermaid rendering.
  - Read-only tools: Read, Grep, Glob, and read-only Bash for `git diff`, `git status`, and `git log`.
  - Fixed report format, with junior-friendly "why it matters" explanations.
- `CLAUDE.md`: push step 3 now simply says to run `backend-reviewer`. The detailed checklist lives in the agent file.

Review: skipped (process rule only, no design content changed)
