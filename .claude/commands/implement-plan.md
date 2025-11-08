---
name: implement-plan
description: Implements a technical plan for Rails features following the phases defined in .agent_session/plan.md, ensuring Rails conventions, multi-tenancy patterns, and test coverage. Uses context from .agent_session/context.md.
---

# Implement Plan

You are tasked with implementing an approved technical plan for the Ruby on Rails application. These plans contain phases with specific changes and success criteria tailored for Rails development.

## Rules
- **CRITICAL**: First read `.agent_session/context.md` for session context
- **MANDATORY**: Read the plan from `.agent_session/plan.md`
- Follow Rails conventions and Fat Model philosophy
- Ensure multi-tenancy with `acts_as_tenant` on all models
- Write tests for all new functionality
- Update `.agent_session/context.md` after each phase completion

## Getting Started

When invoked:
1. **Read context and plan**:
   - Check `.agent_session/context.md` for current implementation status
   - Read `.agent_session/plan.md` completely (or ask for plan location if not found)
   - Check for any existing checkmarks (- [x]) indicating completed work

2. **Understand the Rails feature**:
   - Read the original GitHub issue using:
     ```bash
     GH_TOKEN=$(op.exe read "op://Employee/GitHub CLI PAT/token") gh issue view [number]
     ```
   - Note: The token is stored in 1Password, ensure it's unlocked on desktop
   - Identify models, controllers, views, and jobs involved
   - Review related files in the Rails directory structure
   - **Read files fully** - never use limit/offset parameters

3. **Create implementation todo list** using TodoWrite:
   - Break down each phase into specific Rails tasks
   - Include migrations, models, controllers, views, tests
   - Track progress throughout implementation

## Implementation Philosophy

Plans are designed for Rails, but codebases evolve. Your job is to:
- Follow Rails Way and conventions (Fat Models, Skinny Controllers)
- Implement each phase fully with proper tests before moving to the next
- Ensure multi-tenant isolation in all database queries
- Verify Hotwire/Turbo integration works correctly
- Update checkboxes in the plan and context.md as you complete sections

## Rails Implementation Order

For each phase, follow this sequence:

1. **Database Layer** (if needed):
   ```bash
   # Generate and modify migration
   bin/rails generate migration AddFeatureToModel
   # Edit db/migrate/[timestamp]_add_feature_to_model.rb
   # Run migration
   bin/rails db:migrate
   ```

2. **Model Layer** (Fat Model pattern):
   - Add validations, associations, callbacks
   - Include `acts_as_tenant(:account)` for multi-tenancy
   - Implement business logic methods
   - Add scopes and class methods

3. **Controller Layer** (RESTful, Skinny):
   - Generate controller if needed: `bin/rails generate controller Features`
   - Implement CRUD actions following REST
   - Add Pundit authorization
   - Handle Turbo Stream responses

4. **View Layer** (ERB + Hotwire):
   - Create ERB templates with Turbo Frames
   - Add Stimulus controllers for interactivity
   - Ensure Italian locale for UI text

5. **Background Jobs** (if needed):
   - Create Solid Queue jobs in `app/jobs/`
   - Include `ActsAsTenant.with_tenant` for tenant context
   - Configure retry logic

6. **Tests** (always):
   - Unit tests in `test/models/`
   - Controller tests in `test/controllers/`
   - System tests in `test/system/` for UI flows

If you encounter a mismatch:
```
Problema nella Fase [N]:
Previsto: [cosa dice il piano]
Trovato: [situazione reale nel codice Rails]
Impatto: [come questo influenza multi-tenancy/convenzioni Rails]

Come dovrei procedere?
```

## Verification Approach

After implementing each phase:

1. **Run Rails-specific verification**:
   ```bash
   # Test the migration rollback
   bin/rails db:rollback && bin/rails db:migrate

   # Run all tests
   bin/rails test
   bin/rails test:system

   # Check code quality
   bin/rubocop
   bearer scan .
   ```

2. **Verify Rails patterns**:
   - Check multi-tenant isolation works
   - Verify Pundit policies are enforced
   - Test Turbo Stream responses
   - Ensure N+1 queries are avoided

3. **Update progress**:
   - Check off completed items in `.agent_session/plan.md` using Edit
   - Update `.agent_session/context.md` with implementation status
   - Update TodoWrite list

4. **Manual verification** (if UI involved):
   ```bash
   # Start Rails server
   bin/dev
   # Test in browser at localhost:3000
   # Check Turbo Frame updates work
   # Verify Stimulus controllers function
   ```

## If You Get Stuck

When something isn't working as expected:

1. **Rails-specific debugging**:
   ```bash
   # Check Rails logs
   tail -f log/development.log

   # Rails console for model testing
   bin/rails console
   # Test tenant scoping: ActsAsTenant.current_tenant

   # Check database state
   bin/rails dbconsole
   ```

2. **Use specialized agents** if needed:
   - **codebase-analyzer**: To understand existing Rails patterns
   - **model-specialist**: For ActiveRecord and businesslogic issues
   - **frontend-specialist**: For controllers and views issues
   - **hotwire-specialist**: For Turbo/Stimulus problems
   - **test-specialist**: For test problems

3. **Present the Rails-specific issue clearly**:
   ```
   Problema di implementazione Rails:
   Fase: [N]
   Componente: [Model/Controller/View/Job]
   Errore: [messaggio di errore o comportamento inaspettato]
   File coinvolti: [lista dei file Rails]

   Come dovrei procedere?
   ```

## Common Rails Issues

- **Multi-tenancy**: Ensure all queries include tenant scope
- **N+1 queries**: Use `includes()` for eager loading
- **Form validation**: Check strong parameters and model validations
- **Turbo**: Verify correct `data-turbo-method` attributes
- **Jobs**: Remember `ActsAsTenant.with_tenant(account)` wrapper

## Resuming Work

If the plan has existing checkmarks:
1. **Check context**: Read `.agent_session/context.md` for status
2. **Verify completed work**:
   ```bash
   # Check migrations ran
   bin/rails db:migrate:status

   # Run tests to ensure nothing is broken
   bin/rails test
   ```
3. **Pick up from first unchecked item**
4. **Update todos** to reflect current state

## Rails Development Tips

- **Generator shortcuts**: Use Rails generators when possible
- **Conventions over configuration**: Follow Rails naming conventions
- **Test-driven**: Write tests alongside implementation
- **Security first**: Always use Pundit policies for authorization
- **Performance**: Consider caching and eager loading from the start

Remember: You're building a Rails feature that respects multi-tenancy, follows conventions, and integrates seamlessly with Hotwire. Quality over speed.