# CLAUDE.md

This file is a project guideline that Claude Code automatically reads at the start of a session in this project.

## Required procedure before `git push`

Always follow these steps before running `git push`.

1. Check the `summary/<coding date, YYYY-MM-DD>/` folder. If today's date folder doesn't exist, create it.
2. Inside that folder, add a file describing what's being committed this time.
   - Filename: `NN-short-kebab-case-description.md` (NN is the next sequence number within that date folder, 2-digit zero-padded, and must match the commit order).
   - At the top of the file, write the title `# Commit <hash or "pending"> — <commit message summary>`.
   - Summarize what was changed and why in short bullet points. Even if the background explanation gets long, write it all in this file (don't scatter it elsewhere).
3. Run a review before pushing: spawn a separate reviewer agent that has no access to this conversation and only reads the repo files and the pending changes.
   - The reviewer acts as a **senior backend engineer reviewing a junior's design/code**.
   - For each issue, explain **why it is a problem** in a way a junior can understand.
   - Prioritize problems that actually break in real services: concurrent requests, data consistency, security, and operations.
   - Check that rules don't contradict each other (e.g. detail returns 404 but edit returns 403 for the same case).
   - Check that documents agree with each other (`docs/requirements.md` ↔ `docs/sequence-diagrams/`, later the ERD and API spec).
   - Check for missing cases: authentication, authorization, input validation, existence, duplicates/state.
   - Check that features actually work end to end (e.g. a saved draft can be found again).
   - Once code exists, also review the code for bugs, security issues, and missing tests.
   - Tiny changes (typos, links only) may skip the review.
4. If the reviewer finds problems, report them to the user in Korean and decide together. Do not fix them on your own.
   - Add one line to the summary file: `Review: passed`, `Review: fixed <what>`, or `Review: skipped (<reason>)`.
5. Then create the commit and push.

This procedure is performed automatically every time you push, without the user needing to request it each time.

## Language rules

- Converse with the user in Korean.
- Everything that goes to GitHub is written in English: repo files (README, `docs/`, `summary/`), commit messages, issue/milestone titles and bodies, PR descriptions.
  - Translate what was discussed in Korean into English before pushing.

## Discussion vs. implementation

- When the user is just discussing, asking questions, or confirming something ("대화하자", "대화먼저하자", questions about how something works), only explain/discuss — do not create or edit files.
- Only start implementing (creating/editing files) when the user explicitly asks for it (e.g., "구현해줘", "적용해줘", "고쳐줘", "코딩해줘", "push해줘").
- If it's unclear whether a short reply ("네", "ok") is just agreeing with an explanation or authorizing implementation, ask before writing any code.

## Working style

- The user is learning backend. Go one small step at a time and explain what each step does and why.
- Design comes before code: requirements → sequence diagrams → ERD → API spec. Do **P0 only** first; add P1/P2 in later rounds.
- Never delete GitHub issues; close them instead ("Close as not planned" for mistakes).
