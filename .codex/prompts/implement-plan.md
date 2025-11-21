description: Execute the approved plan in .agent_session/plan.md phase by phase with tests (Codex)
---

# Implement Plan (Codex)

Work through the agreed implementation plan for the Rails app.

## Rules
- Read `.agent_session/plan.md` fully before coding.
- Update plan checkboxes as phases complete.
- Follow Rails conventions, multi-tenancy (`acts_as_tenant`), and AGENTS guidance.
- Add tests for new behavior; keep UI text in Italian where appropriate.

## Process
1. **Review inputs**: confirm tasks and success criteria for the current phase. Note any existing checkmarks.
2. **Prepare worklist**: break the phase into actionable steps (migration/model/controller/view/job/tests) and order them DB → model → controller → view/Hotwire → jobs → tests.
3. **Implement**: apply changes incrementally, keeping controllers skinny and models rich. Respect tenant scoping and authorization.
4. **Verify**:
   - Run relevant tests (`bin/rails test`, targeted folders, `bearer scan .` when applicable).
   - Perform manual checks for UI/Turbo flows if the plan calls for it.
5. **Record progress**: tick items in `.agent_session/plan.md`, note results or deviations in your response, and surface blockers/questions to the user.
6. **Iterate**: move to the next phase only after meeting success criteria or confirming adjustments with the user.
