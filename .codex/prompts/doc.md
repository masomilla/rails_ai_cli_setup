---
description: Update /docs based on recent changes, keeping structure and language conventions (Codex)
---

# Update Documentation (Codex)

Refresh documentation in `/docs` to match current work.

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
- Analyze git changes to decide what to document.
- Use Italian for narrative/headings; keep code/technical terms and comments in English.
- Prefer updating existing docs; if adding files, update `/docs/README.md` index.

## Process
1. **Context sync**: ensure `.agent_session/context.md` exists (create with the template above if not), read it closely, and capture prior decisions or TODOs before acting.
2. **Understand changes**:
   - If `.agent_session/plan.md` exists, use it to determine scope; no need to diff against main.
   - Otherwise, detect branch:
     - On a feature branch: `git fetch origin` then inspect `git diff --stat origin/main...HEAD` and `git log --oneline origin/main...HEAD` to see what changed.
     - On `main`: inspect recent commits (e.g., `git log -5 --oneline`) to find what was merged.
   - Also run `git status` to catch unstaged changes.
3. **Identify targets** (examples):
   - Features → `/docs/technical/features/`
   - Patterns/architecture → `/docs/technical/patterns/`
   - Integrations → `/docs/technical/integrations/`
   - API → `/docs/api/`
   - Business domain → `/docs/business/`
4. **Review existing docs**: read relevant files fully to mirror tone and structure.
5. **Draft updates**: include overview, technical details with real examples (models/controllers/jobs/tests), and cross-links using relative paths.
6. **Quality checks**: ensure Italian text reads well, code samples compile conceptually, and indexes are updated when new files are added.
7. **Update context**: capture documentation changes, decisions, and remaining TODOs in `.agent_session/context.md` and append a dated log entry.
8. **Share plan/summary with user** before finalizing if scope is large.
