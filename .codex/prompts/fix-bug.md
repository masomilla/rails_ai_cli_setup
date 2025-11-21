---
description: Analyze a bug report, plan the fix, implement it, and add tests (Codex)
arguments:
  - name: issue
    description: Issue number (e.g., 123) or pasted issue text; optional.
---

# Fix Bug (Codex)

Handle bug reports end-to-end: understand, reproduce, fix, and verify.

## Rules
- Read the issue and relevant files fully; prefer `rg` to locate code.
- Follow Rails conventions and multi-tenancy patterns; keep comments/tests in English.
- Add or update tests to cover the regression.

## Process
1. **Understand the bug**: if the argument is a GitHub issue number, read it via `GH_TOKEN=$(op.exe read "op://Employee/GitHub CLI PAT/token") gh issue view [number]`; if free text, treat it as the report body. Gather steps, expected vs actual behavior, and scope/out-of-scope. If missing, ask clarifying questions.
2. **Reproduce**: run the minimal scenario (tests or manual) and capture failing test when possible. Share findings in your response.
3. **Diagnose**: inspect models/controllers/views/jobs involved; trace logs or console as needed.
4. **Plan the fix**: outline the change (data, logic, UI) and tests you will add. Confirm with user when impactful.
5. **Implement**: make targeted code changes, preserve tenants and auth checks, keep controllers thin and models rich.
6. **Test**: add/update unit/integration tests; run relevant suites (`bin/rails test` and narrower commands when possible).
7. **Validate**: re-run reproduction to confirm fix, note any remaining risks, and summarize the outcome to the user.
