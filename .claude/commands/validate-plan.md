---
name: validate-plan
description: Validates that an implementation plan was correctly executed for Rails features, verifying all success criteria, checking multi-tenancy patterns, and ensuring Rails conventions are followed. Uses context from .agent_session/context.md to understand what was implemented.
---

Validate implementation plan execution, verify success criteria, check Rails conventions and multi-tenancy patterns.

## Core Rules

1. **Context First**: Read `.agent_session/context.md` and `.agent_session/plan.md`
2. **Automated Tests**: Run all checks before manual verification
3. **Rails Patterns**: Verify conventions and multi-tenancy
4. **Be Thorough**: Document successes and issues

## Initial Setup

**Check context**:
- Read `.agent_session/context.md` for implementation status
- Read `.agent_session/plan.md` for planned changes
- If missing, analyze git commits

**Gather evidence**:
```bash
git log --oneline -n 20
git diff HEAD~N..HEAD
bin/rails test && bin/rails test:system
bearer scan . && bin/rubocop
```

## Validation Process

### 1. Context Discovery

**Read plan** and identify expected changes:
- Models created/modified
- Controllers and actions
- Views and Hotwire components
- Background jobs
- Migrations applied
- Tests added

**Spawn research agents**:
- **codebase-analyzer**: Verify models, migrations, schema
- **codebase-locator**: Find controllers, views, policies
- **test-specialist**: Check test coverage and results

### 2. Systematic Validation

**Per phase, check**:
```bash
bin/rails db:migrate:status                    # Migrations
bin/rails db:rollback && bin/rails db:migrate  # Rollback test
bin/rails test                                  # Unit/integration
bin/rails test:system                           # System tests
bearer scan .                                   # Security
```

**Verify Rails patterns**:
- Fat Models (business logic in models)
- Skinny Controllers (RESTful, minimal logic)
- Concerns for shared behavior
- Hotwire integration (Turbo Frames/Streams)

**Check multi-tenancy**:
- `acts_as_tenant(:account)` on models
- `set_current_tenant_account` in controllers
- Pundit policies tested
- No tenant data leakage

### 3. Generate Report

```markdown
## Validation: [Feature]

### Status
✓ Fase 1: [Nome] - Completa
⚠️ Fase 2: [Nome] - Parziale (vedi problemi)

### Automated Checks
✓ Migration: `bin/rails db:migrate`
✓ Tests: `bin/rails test` (45 tests, 0 failures)
✓ Security: `bearer scan .` (0 issues)
✗ Rubocop: 3 warnings

### Rails Review

**Implemented**:
- Migration with indices
- Model with validations, `acts_as_tenant`
- RESTful controller
- Pundit policy
- ERB views with Turbo Frames

**Deviations**:
- Added concern (improvement)
- Used Form Object (not planned)

**Issues**:
- Missing cache invalidation callback
- Job lacks tenant context
- Missing edge case test

### Manual Testing
- [ ] Turbo Frame updates
- [ ] Stimulus interactions
- [ ] Mobile responsive
- [ ] Multi-tenant isolation
- [ ] Performance with 10k+ records

### Recommendations
- Fix Rubocop warnings
- Add `ActsAsTenant.with_tenant` in job
- Eager load relations
- Update `/docs`
```

## Rails Validation Checklist

- [ ] All phases complete
- [ ] Tests pass: `bin/rails test` & `test:system`
- [ ] Migrations reversible
- [ ] Fat Model pattern followed
- [ ] Controllers RESTful and skinny
- [ ] Multi-tenancy: `acts_as_tenant` implemented
- [ ] Pundit policies secure endpoints
- [ ] Hotwire/Turbo works correctly
- [ ] Jobs handle tenant context
- [ ] No N+1 queries
- [ ] Italian localization present
- [ ] Documentation updated
- [ ] Security scan passes

## Workflow

1. `/implement-plan` - Execute
2. `/commit` - Commit changes
3. `/validate-plan` - Verify
4. `/pr` - Create PR

**Be thorough**. Catch issues before production.