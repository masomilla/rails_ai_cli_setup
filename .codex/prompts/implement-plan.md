description: Execute the approved plan in .agent_session/plan.md phase by phase with tests (Codex)
---

# Implement Plan (Codex)

Work through the agreed implementation plan for the Rails app.

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
- Read `.agent_session/plan.md` fully before coding.
- Update plan checkboxes as phases complete.
- Follow Rails conventions, multi-tenancy (`acts_as_tenant`), and AGENTS guidance.
- Add tests for new behavior; keep UI text in Italian where appropriate.

## Process
1. **Context sync**: ensure `.agent_session/context.md` exists (create with the template above if not), read it closely, and capture any relevant notes before acting.
2. **Review inputs**: confirm tasks and success criteria for the current phase. Note any existing checkmarks.
3. **Prepare worklist**: break the phase into actionable steps (migration/model/controller/view/job/tests) and order them DB → model → controller → view/Hotwire → jobs → tests.
4. **Implement**: apply changes incrementally, keeping controllers skinny and models rich. Respect tenant scoping and authorization.
5. **Verify**:
   - Run relevant tests (`bin/rails test`, targeted folders, `bearer scan .` when applicable).
   - Perform manual checks for UI/Turbo flows if the plan calls for it.
6. **Record progress**: tick items in `.agent_session/plan.md`, update `.agent_session/context.md` sections and append a dated log entry, note results or deviations in your response, and surface blockers/questions to the user.
7. **Iterate**: move to the next phase only after meeting success criteria or confirming adjustments with the user.
