---
name: validate-plan
description: Validates that an implementation plan was correctly executed for Rails features, verifying all success criteria, checking multi-tenancy patterns, and ensuring Rails conventions are followed. Uses context from .agent_session/context.md to understand what was implemented.
---

# Validate Plan

You are tasked with validating that an implementation plan was correctly executed in the Ruby on Rails application, verifying all success criteria and identifying any deviations or issues.

## Rules
- **CRITICAL**: First check `.agent_session/context.md` for implementation context
- **MANDATORY**: Read the implementation plan from `.agent_session/plan.md` if exists
- Verify Rails conventions and multi-tenancy patterns are followed
- Check that all automated tests pass before manual verification

## Initial Setup

When invoked:
1. **Check context file** - Read `.agent_session/context.md` for session context
   - If existing: Review implementation status and decisions
   - If missing: Need to discover what was done through git and codebase analysis

2. **Locate the plan**:
   - First check `.agent_session/plan.md`
   - If not found, check for plan references in recent commits
   - Otherwise ask user for plan location

3. **Gather implementation evidence**:
   ```bash
   # Check recent commits
   git log --oneline -n 20
   git diff HEAD~N..HEAD  # Where N covers implementation commits

   # Run Rails-specific checks
   bin/rails test
   bin/rails test:system
   bearer scan .
   bin/rubocop
   ```

## Validation Process

### Step 1: Context Discovery

If starting fresh or need more context:

1. **Read the implementation plan** completely from `.agent_session/plan.md`
2. **Identify what should have changed**:
   - Rails models that should be created/modified
   - Controllers and actions implemented
   - Views and Hotwire components added
   - Background jobs (Solid Queue) created
   - Database migrations applied
   - Tests (unit, integration, system) added

3. **Spawn parallel research tasks** using specialized agents:
   ```
   Task 1 - Verify Rails models and database:
   Use codebase-analyzer to check if migrations were added in db/migrate/
   Verify schema.rb changes match plan specifications
   Check model validations, associations, and multi-tenant scoping
   Return: Comparison of planned vs actual database/model changes

   Task 2 - Verify controllers and views:
   Use codebase-locator to find all modified controllers in app/controllers/
   Check views in app/views/ and Stimulus controllers in app/javascript/
   Verify Pundit policies in app/policies/
   Return: List of implemented endpoints and UI components

   Task 3 - Verify tests and background jobs:
   Check test files in test/models/, test/controllers/, test/system/
   Verify job implementations in app/jobs/
   Run test suite and capture results
   Return: Test coverage report and job implementation status
   ```

### Step 2: Systematic Validation

For each phase in the plan:

1. **Check Rails-specific completion**:
   - Verify migrations ran: `bin/rails db:migrate:status`
   - Check model validations and associations work
   - Verify multi-tenant scoping with `acts_as_tenant`
   - Confirm controllers follow RESTful patterns

2. **Run Rails automated verification**:
   ```bash
   # Database and models
   bin/rails db:migrate
   bin/rails db:rollback && bin/rails db:migrate  # Test rollback

   # Tests
   bin/rails test
   bin/rails test:system

   # Code quality
   bin/rubocop
   bearer scan .
   ```

3. **Verify Rails patterns**:
   - Fat Model pattern: Business logic in models not controllers
   - Form Objects using Composable pattern where applicable
   - Proper use of concerns for shared behavior
   - Hotwire integration (Turbo Frames/Streams)

4. **Check multi-tenancy and security**:
   - All models have `acts_as_tenant(:account)`
   - Controllers use `set_current_tenant_account`
   - Pundit policies implemented and tested
   - No tenant data leakage in queries

### Step 3: Generate Validation Report

Create comprehensive validation summary:

```markdown
## Validation Report: [Feature Name]

### Implementation Status
✓ Fase 1: [Nome] - Completamente implementata
✓ Fase 2: [Nome] - Completamente implementata
⚠️ Fase 3: [Nome] - Parzialmente implementata (vedi problemi)

### Rails Automated Verification Results
✓ Migrazione database: `bin/rails db:migrate`
✓ Test unitari: `bin/rails test` (45 tests, 0 failures)
✓ Test di sistema: `bin/rails test:system` (12 tests, 0 failures)
✓ Sicurezza: `bearer scan .` (0 critical issues)
✗ Rubocop: `bin/rubocop` (3 warnings)

### Rails-Specific Review

#### Correttamente Implementato:
- Migrazione `db/migrate/[timestamp]_add_feature.rb` aggiunge tabella con indici
- Model `app/models/feature.rb` con validazioni e `acts_as_tenant(:account)`
- Controller RESTful `app/controllers/features_controller.rb` con 7 azioni
- Policy Pundit `app/policies/feature_policy.rb` per autorizzazioni
- Viste ERB con Turbo Frames in `app/views/features/`

#### Deviazioni dal Piano:
- Aggiunto concern `app/models/concerns/trackable.rb` non previsto (miglioramento)
- Usato Form Object in `app/forms/feature_form.rb` per form complesso

#### Problemi Potenziali:
- Manca callback `after_commit` per invalidare cache
- Job `app/jobs/feature_sync_job.rb` non gestisce tenant context
- Manca test system per caso limite con 1000+ record

### Test Manuali Richiesti:
1. Funzionalità UI:
   - [ ] Verificare form con Turbo Frame si aggiorna correttamente
   - [ ] Testare Stimulus controller per interazioni dinamiche
   - [ ] Verificare responsive design su mobile

2. Multi-tenancy:
   - [ ] Confermare isolamento dati tra account diversi
   - [ ] Testare con utente che appartiene a multipli account

3. Performance:
   - [ ] Verificare query N+1 con bullet gem
   - [ ] Testare con dataset di 10.000+ record

### Raccomandazioni:
- Correggere warning Rubocop prima del merge
- Aggiungere `ActsAsTenant.with_tenant` nel job
- Considerare eager loading per relazioni in index action
- Aggiungere documentazione in `/docs`
```

## Working with Existing Context

If you were part of the implementation:
- Review the conversation history
- Check your todo list for what was completed
- Focus validation on work done in this session
- Be honest about any shortcuts or incomplete items

## Important Guidelines

1. **Be thorough but practical** - Focus on what matters
2. **Run all automated checks** - Don't skip verification commands
3. **Document everything** - Both successes and issues
4. **Think critically** - Question if the implementation truly solves the problem
5. **Consider maintenance** - Will this be maintainable long-term?

## Rails Validation Checklist

Always verify:
- [ ] All phases marked complete are actually done
- [ ] Rails tests pass: `bin/rails test` and `bin/rails test:system`
- [ ] Migrations are reversible: `bin/rails db:rollback && bin/rails db:migrate`
- [ ] Models follow Fat Model pattern with business logic
- [ ] Controllers are RESTful and skinny
- [ ] Multi-tenancy with `acts_as_tenant` properly implemented
- [ ] Pundit policies secure all endpoints
- [ ] Hotwire/Turbo integration works correctly
- [ ] Background jobs handle tenant context
- [ ] No N+1 queries (check with bullet gem)
- [ ] Italian localization present (default locale)
- [ ] Documentation updated in `/docs`
- [ ] Security scan passes: `bearer scan .`

## Relationship to Other Commands

Recommended workflow:
1. `/implement_plan` - Execute the implementation
2. `/commit` - Create atomic commits for changes
3. `/validate_plan` - Verify implementation correctness
4. `/describe_pr` - Generate PR description

The validation works best after commits are made, as it can analyze the git history to understand what was implemented.

Remember: Good validation catches issues before they reach production. Be constructive but thorough in identifying gaps or improvements.