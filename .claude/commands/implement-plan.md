---
name: implement-plan
description: Implements a technical plan for Rails features following the phases defined in .agent_session/plan.md, ensuring Rails conventions, multi-tenancy patterns, and test coverage. Uses context from .agent_session/context.md.
---

Implement approved technical plans for Rails features. Follow conventions, ensure multi-tenancy, write tests.

## Core Rules

1. **Context First**: Maintain `.agent_session/context.md` as the shared state file: read it fully before working. If missing, create it with the standard template.
2. **Update Log**: After each run, refresh the `Overview/Decisions/TODO` sections and append a new dated log entry.
3. **Plan Reference**: Read `.agent_session/plan.md` fully before coding and update checkboxes as phases complete.
4. **Rails Way**: Follow Fat Models, Skinny Controllers, RESTful routes
5. **Multi-tenancy**: `acts_as_tenant(:account)` on all models
6. **Test Coverage**: Write tests for all functionality
7. **Track Progress**: Use TodoWrite and update checkboxes in plan/context

## Getting Started

**Context sync**: Ensure `.agent_session/context.md` exists (create with template below if not), read it closely, and capture any relevant notes before acting:
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

**Read files**:
- `.agent_session/plan.md` - phases and success criteria
- Check existing checkmarks `- [x]` for completed work

**Understand feature**:
- Read GitHub issue: `GH_TOKEN=$(op.exe read "op://Employee/GitHub CLI PAT/token") gh issue view [number]`
- Identify Rails components: models, controllers, views, jobs
- Read related files fully (no limits/offsets)

**Create todos**: Break phases into specific tasks using TodoWrite

## Implementation Order

Follow this sequence for each phase:

### 1. Database (if needed)
```bash
bin/rails generate migration AddFeatureToModel
# Edit db/migrate/[timestamp]_*.rb
bin/rails db:migrate
```

### 2. Models (Fat Model)
- Validations, associations, callbacks
- `acts_as_tenant(:account)` for multi-tenancy
- Business logic methods, scopes

### 3. Controllers (Skinny, RESTful)
- `bin/rails generate controller Features`
- CRUD actions, Pundit authorization
- Turbo Stream responses

### 4. Views (ERB + Hotwire)
- ERB templates with Turbo Frames
- Stimulus controllers
- Italian locale for UI

### 5. Background Jobs (if needed)
- Create in `app/jobs/`
- Wrap with `ActsAsTenant.with_tenant(account)`
- Configure retry logic

### 6. Tests (always)
- `test/models/` - unit tests
- `test/controllers/` - controller tests
- `test/system/` - UI flows (only if necessary)

## Verification Per Phase

**Run checks**:
```bash
bin/rails db:rollback && bin/rails db:migrate  # Test migration
bin/rails test                                  # All tests
bin/rails test:system                           # System tests
bearer scan .                                   # Security
```

**Verify Rails patterns**:
- Multi-tenant isolation works
- Pundit policies enforced
- No N+1 queries (use `includes()`)
- Turbo Streams functional

**Update progress**:
- Check items in `.agent_session/plan.md`
- Update `.agent_session/context.md` sections (Overview/Decisions/TODO)
- Append dated log entry with phase results
- Mark todos complete

**Manual testing** (if UI):
```bash
bin/dev  # Start server, test at localhost:3000
```

## When Stuck

**Debug**:
```bash
tail -f log/development.log           # Rails logs
bin/rails console                      # Test models
bin/rails dbconsole                    # Database
```

**Spawn agents** if needed:
- **codebase-analyzer**: Understand existing patterns
- **model-specialist**: ActiveRecord/business logic
- **frontend-specialist**: Controllers/views
- **hotwire-specialist**: Turbo/Stimulus
- **test-specialist**: Test issues

**Report issues**:
```
Problema Fase [N]:
Componente: [Model/Controller/View/Job]
Errore: [message/behavior]
File: [paths]
Come procedere?
```

## Common Pitfalls

- **Multi-tenancy**: All queries must include tenant scope
- **N+1 queries**: Eager load with `includes()`
- **Strong params**: Verify controller parameters
- **Turbo**: Check `data-turbo-method` attributes
- **Jobs**: Wrap with `ActsAsTenant.with_tenant(account)`

## Resuming Work

If plan has checkmarks:
1. Read `.agent_session/context.md` for status
2. Verify: `bin/rails db:migrate:status` and `bin/rails test`
3. Start from first unchecked item
4. Update todos

## Philosophy

Build Rails features that:
- Respect multi-tenancy (tenant isolation)
- Follow conventions (naming, structure)
- Integrate with Hotwire seamlessly
- Have comprehensive test coverage
- Use Pundit for authorization

**Quality over speed**. Implement fully before moving to next phase.