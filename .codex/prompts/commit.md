---
description: Create git commits for changes made during the session with clear, descriptive messages (Codex)
---

# Commit Changes (Codex)

Use this command to group current work into one or more clean commits.

## Rules
- Maintain `.agent_session/context.md` as the shared state file: read it fully before working. If missing, create it with:
```
# Session Context
## Overview
- Current feature/issue
## Decisions
- 
## TODO
- 
## Log
- [YYYY-MM-DD HH:MM TZ] command: summary, tests, next steps
```
- After each run, refresh the `Overview/Decisions/TODO` sections and append a new dated log entry.
- Follow AGENTS.md guidance (English messages, preserve existing style).
- Never add co-authors or AI attribution; messages stay imperative.
- Do not amend existing commits unless the user asks.
- Stage files explicitly (no `git add .`).

## Process
1. Context sync: ensure `.agent_session/context.md` exists (create with the template above if not), read it closely, and capture relevant notes before acting.
2. Review the work: run `git status` and `git diff` to see what changed and how things group logically.
3. Propose a commit plan: list files per commit with proposed messages; ask the user to confirm before committing.
4. Execute after approval:
   - Stage only the intended files for each commit.
   - Run `git commit -m "..."` using the agreed message.
5. Show results: `git log --oneline -n 5` and remind about any remaining uncommitted files.
6. Update context: record commit outcomes and next steps in `.agent_session/context.md` and append a dated log entry.

## Commit Message Tips
- Focus on why and what ("Add task reminder email job"), not tooling.
- Keep related changes together; split unrelated changes.
- If tests were added or fixes made, mention them briefly in the body when helpful.
