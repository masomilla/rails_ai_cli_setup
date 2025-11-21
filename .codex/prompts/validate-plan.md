---
description: Confirm that the implementation plan was executed correctly and meets success criteria (Codex)
---

# Validate Plan Execution (Codex)

Check that delivered work matches the agreed plan and quality bar.

## Rules
- Read `.agent_session/plan.md` first.
- Verify Rails conventions, multi-tenancy, and localization expectations.
- Run automated checks before giving a green light.

## Process
1. **Gather evidence**:
   - Review plan phases and success criteria.
   - Inspect recent commits/diffs to see what changed (models/controllers/views/jobs/tests/migrations).
2. **Run checks**:
```bash
bin/rails test       # or narrower targets
bin/rails test:system  # only if UI flows were touched
bearer scan .        # security scan when relevant
```
   - Note failures or skipped steps with rationale.
3. **Compare plan vs reality**:
   - For each phase, confirm required files/behaviors exist.
   - Check tenant scoping, Pundit policies, Turbo/Stimulus behavior, and translations where applicable.
   - Ensure tests cover new behavior and pass.
4. **Manual verification** (as needed): exercise key UI flows, error states, and edge cases described in the plan.
5. **Report**: summarize findings with sections `Aligned`, `Deviations`, `Risks/Follow-ups`. Call out missing items or failing checks explicitly and suggest next steps.
