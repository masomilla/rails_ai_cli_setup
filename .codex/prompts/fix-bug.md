---
description: Analyze a bug report, plan the fix, implement it, and add tests (Codex)
arguments:
  - name: issue
    description: Issue number (e.g., 123) or pasted issue text; optional.
---

# Fix Bug (Codex)

Handle bug reports end-to-end: understand, reproduce, fix, and verify.

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
- Read the issue and relevant files fully; prefer `rg` to locate code.
- Follow Rails conventions and multi-tenancy patterns; keep comments/tests in English.
- Add or update tests to cover the regression.

## Process
1. **Context sync**: ensure `.agent_session/context.md` exists (create with the template above if not), read it closely, and capture prior decisions or TODOs before acting.
2. **Understand the bug**: if the argument is a GitHub issue number, read it via `GH_TOKEN=$(op.exe read "op://Employee/GitHub CLI PAT/token") gh issue view [number]`; if free text, treat it as the report body. Gather steps, expected vs actual behavior, and scope/out-of-scope. If missing, ask clarifying questions.
3. **Reproduce**: run the minimal scenario (tests or manual) and capture failing test when possible. Share findings in your response.
4. **Diagnose**: inspect models/controllers/views/jobs involved; trace logs or console as needed.
5. **Plan the fix**: outline the change (data, logic, UI) and tests you will add. Confirm with user when impactful.
6. **Implement**: make targeted code changes, preserve tenants and auth checks, keep controllers thin and models rich.
7. **Test**: add/update unit/integration tests; run relevant suites (`bin/rails test` and narrower commands when possible).
8. **Validate**: re-run reproduction to confirm fix, note any remaining risks, and summarize the outcome to the user. Update `.agent_session/context.md` with outcomes and append a dated log entry.
