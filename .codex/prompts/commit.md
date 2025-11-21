---
description: Create git commits for changes made during the session with clear, descriptive messages (Codex)
---

# Commit Changes (Codex)

Use this command to group current work into one or more clean commits.

## Rules
- Follow AGENTS.md guidance (English messages, preserve existing style).
- Never add co-authors or AI attribution; messages stay imperative.
- Do not amend existing commits unless the user asks.
- Stage files explicitly (no `git add .`).

## Process
1. Review the work: run `git status` and `git diff` to see what changed and how things group logically.
2. Propose a commit plan: list files per commit with proposed messages; ask the user to confirm before committing.
3. Execute after approval:
   - Stage only the intended files for each commit.
   - Run `git commit -m "..."` using the agreed message.
4. Show results: `git log --oneline -n 5` and remind about any remaining uncommitted files.

## Commit Message Tips
- Focus on why and what ("Add task reminder email job"), not tooling.
- Keep related changes together; split unrelated changes.
- If tests were added or fixes made, mention them briefly in the body when helpful.
