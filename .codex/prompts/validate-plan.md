---
description: Confirm that the implementation plan was executed correctly and meets success criteria (Codex)
---

# Validate Plan Execution (Codex)

Check that delivered work matches the agreed plan and quality bar.

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
- Read `.agent_session/plan.md` first.
- Verify Rails conventions, multi-tenancy, and localization expectations.
- Run automated checks before giving a green light.

## Process
1. **Context sync**: ensure `.agent_session/context.md` exists (create with the template above if not), read it closely, and capture prior decisions or TODOs before acting.
2. **Gather evidence**:
   - Review plan phases and success criteria.
   - Inspect recent commits/diffs to see what changed (models/controllers/views/jobs/tests/migrations).
3. **Run checks**:
```bash
bin/rails test       # or narrower targets
bin/rails test:system  # only if UI flows were touched
bearer scan .        # security scan when relevant
```
   - Note failures or skipped steps with rationale.
4. **Compare plan vs reality**:
   - For each phase, confirm required files/behaviors exist.
   - Check tenant scoping, Pundit policies, Turbo/Stimulus behavior, and translations where applicable.
   - Ensure tests cover new behavior and pass.
5. **Manual verification** (as needed): exercise key UI flows, error states, and edge cases described in the plan.
6. **Report**: summarize findings with sections `Aligned`, `Deviations`, `Risks/Follow-ups`. Call out missing items or failing checks explicitly and suggest next steps. Update `.agent_session/context.md` with validation results and append a dated log entry.
