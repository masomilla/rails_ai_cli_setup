---
name: pr
description: Creates a new pull request for Rails features based on completed implementation, analyzing changes from git and context files to generate appropriate title and description. Follows Rails conventions and multi-tenancy patterns.
---

Create pull request for implemented Rails features. Analyze git changes, run verification checks, generate PR description in Italian language.

## Core Rules

1. **Context First**: Maintain `.agent_session/context.md` as the shared state file: read it fully before working. If missing, create it with the standard template.
2. **Update Log**: After each run, refresh the `Overview/Decisions/TODO` sections and append a new dated log entry.
3. **Plan Reference**: Read `.agent_session/plan.md` if available to understand scope; skip diffing against main if plan exists.
4. **Verify Branch**: Check current branch, ensure no existing PR
5. **Run Checks**: Execute automated tests before creating PR
6. **No Attribution**: Never add co-author or Claude attribution
7. **Imperative Mood**: "Add feature" not "Added feature"

## Process

### 1. Context Sync & Understand Implementation

**Context sync**: Ensure `.agent_session/context.md` exists (create with template below if not), read it closely, and capture prior decisions or TODOs before acting:
```markdown
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

**Read changes**:
```bash
git status
git diff HEAD~1..HEAD
git log --oneline -n 10
```

**Check files**: `.agent_session/plan.md` if available

**Analyze Rails changes**:
- Migrations: `db/migrate/`
- Models: `app/models/` (`acts_as_tenant`)
- Controllers: `app/controllers/` (REST)
- Views: `app/views/` (ERB, Turbo)
- JavaScript: `app/javascript/controllers/` (Stimulus)
- Jobs: `app/jobs/` (Solid Queue)
- Tests: `test/`
- Policies: `app/policies/` (Pundit)
- Locales: `config/locales/` (Italian)

### 2. Verify Branch Status

```bash
git branch --show-current
GH_TOKEN=$(op.exe read "op://Employee/GitHub CLI PAT/token") gh pr view 2>/dev/null
```

- If on main → ask to create feature branch
- If PR exists → ask about updating

### 3. Run Verification

```bash
bin/rails test
bin/rails test:system
bin/rubocop
bearer scan .
```

Update PR description with results:
- `[x]` if passes
- `[ ]` with explanation if fails

### 4. Present Plan

```
PR Title: [Feature Title]
Target: main

[Draft PR description]

Proceed?
```

### 5. Create PR

```bash
git push -u origin [branch]
GH_TOKEN=$(op.exe read "op://Employee/GitHub CLI PAT/token") gh pr create --title "[Title]" --body-file .agent_session/pr_description.md --base main
```

**Update `.agent_session/context.md`**:
- Document PR creation in Overview section
- Update TODO section with manual verification items
- Append dated log entry: `[YYYY-MM-DD HH:MM TZ] pr: created PR #[number], [url]`

Save description to `.agent_session/pr_description.md`

## PR Description Template (Italian)

```markdown
[Summary from context.md]

## Modifiche al Database
- [ ] Migration: `db/migrate/[timestamp]_[desc].rb`
- [ ] Schema: [modifiche]
- [x] Rollback testato

## Componenti Rails
- **Models**: `app/models/[model].rb` - [logica]
- **Controllers**: `app/controllers/[controller].rb` - [azioni REST]
- **Views**: `app/views/[views]/` - [ERB + Turbo]
- **Jobs**: `app/jobs/[job].rb` - [elaborazione]
- **Policies**: `app/policies/[policy].rb` - [autorizzazione]

## Multi-tenancy & Sicurezza
- [x] `acts_as_tenant(:account)` sui models
- [x] Policies Pundit testate
- [x] Nessun data leakage tra tenant
- [x] Strong parameters configurati

## Test
- [x] Unit: `test/models/`
- [x] Controller: `test/controllers/`
- [ ] System: Verifica manuale UI necessaria

## Qualità del Codice
- [x] Test passano: `bin/rails test`
- [x] Rubocop: `bin/rubocop`
- [x] Security: `bearer scan .`

## Verifica Manuale
- [ ] Aggiornamenti Turbo Frame funzionano
- [ ] Isolamento multi-tenant
- [ ] Design responsive
- [ ] Performance con dataset grandi
```

**Focus**: Architettura Rails, multi-tenancy, impatto utente. Basarsi sulle modifiche effettive, non sui piani.