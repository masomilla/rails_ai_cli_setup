---
name: fix-bug
description: Reads a GitHub issue or task description for a bug, creates an implementation plan, and immediately executes the fix. Combines planning and implementation in a streamlined workflow for bug fixes in Rails applications.
---

Fix bugs in Rails by reading issue, investigating root cause, and implementing tested solution in streamlined workflow.

## Core Rules

1. **Context First**: Create/update `.agent_session/context.md` throughout process
2. **Rails Patterns**: Follow conventions, ensure multi-tenancy
3. **Test Coverage**: Add tests that prevent regression
4. **Minimal Fix**: Targeted changes, not refactoring
5. **Track Progress**: Use TodoWrite for bug fix tasks

## Process

### 1. Issue Analysis

**Read bug issue**:
- GitHub: `GH_TOKEN=$(op.exe read "op://Employee/GitHub CLI PAT/token") gh issue view [number]`
- Extract: bug description, steps to reproduce, expected behavior, affected components

**Create context**:
```markdown
# Bug Fix: [Title]
## Bug Summary
- **Issue**: #[number]
- **Problem**: [description]
- **Impact**: [severity]
- **Components**: [models/controllers/views/jobs]

## Steps to Reproduce
1. [steps]
```

### 2. Investigation

**Spawn research agents**:
- **codebase-locator**: Find bug-related files
- **codebase-analyzer**: Trace code path, identify root cause

**Read identified files fully** and update context:
```markdown
## Root Cause
- **Location**: [file:line]
- **Cause**: [technical explanation]
- **Impact**: [scope]
```

### 3. Solution Planning

**Design fix**:
- Minimal change to resolve issue
- Edge cases and side effects
- Multi-tenant compatibility
- Test strategy

**Create todos**: Break fix into tasks using TodoWrite

**Update context**:
```markdown
## Solution
- **Approach**: [strategy]
- **Changes**: [modifications]
- **Tests**: [verification]
```

### 4. Implementation

**Fix following Rails order**:
- **Models**: Business logic, validations, `acts_as_tenant`
- **Controllers**: Actions, params, Pundit, Turbo Streams
- **Views**: ERB, Turbo Frames, Stimulus
- **Jobs**: Logic, tenant context, retry handling

**Write tests**:
- Reproduce bug scenario
- Verify fix works
- Prevent regression
- Test edge cases

**Run verification**:
```bash
bin/rails test && bin/rails test:system
bearer scan .
```

### 5. Final Check

**Comprehensive testing**:
- Original bug fixed
- No broken functionality
- Multi-tenant isolation intact
- Performance acceptable

**Present summary**:
```
🐛 Bug Fixed!
Issue: [description]
Root Cause: [what was wrong]
Solution: [what changed]
Files: [modified files]
Tests: [added tests]
```

## Common Bug Types

- **Logic**: Incorrect business rules
- **Data**: Wrong queries, missing scopes, tenant leakage
- **UI**: Turbo interactions, form issues
- **Performance**: N+1 queries, slow operations
- **Security**: Authorization bypass, data exposure

## Debugging Tools

```bash
tail -f log/development.log           # Rails logs
bin/rails console                      # Test models/tenant scope
bin/rails dbconsole                    # Database state
```

## When Stuck

**Spawn specialist agents**:
- **model-specialist**: ActiveRecord/business logic
- **frontend-specialist**: Controllers/views
- **hotwire-specialist**: Turbo/Stimulus
- **rails-specialist**: Background jobs
- **test-specialist**: Test strategy

**Report issue**:
```
🐛 Need Help:
Step: [current step]
Component: [Model/Controller/View/Job]
Issue: [problem]
Tried: [attempts]
Files: [paths]
```

## Guidelines

- Focus on minimal, targeted fixes
- Add regression tests
- Consider impact on existing code
- Update context throughout
- Use agents for insights

**Quality over speed**. Fix correctly with tests.