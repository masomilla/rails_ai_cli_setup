---
name: fix-bug
description: Reads a GitHub issue or task description for a bug, creates an implementation plan, and immediately executes the fix. Combines planning and implementation in a streamlined workflow for bug fixes in Rails applications.
---

Fix bugs in Rails by reading issue, investigating root cause, and implementing tested solution in streamlined workflow.

## Usage

```bash
/fix-bug [issue-id|description]
```

**Parameters:**
- `issue-id` (optional): GitHub issue number (e.g., `123`) - will fetch issue details using `gh issue view`
- `description` (optional): Free text description of the bug to fix

**Examples:**
```bash
/fix-bug 789                                    # Fix bug from GitHub issue #789
/fix-bug                                         # Interactive mode - will ask for details
/fix-bug Contact form validation not working    # Fix bug from text description
```

## Core Rules

1. **Context First**: Maintain `.agent_session/context.md` as the shared state file: read it fully before working. If missing, create it with the standard template.
2. **Update Log**: After each run, refresh the `Overview/Decisions/TODO` sections and append a new dated log entry.
3. **Rails Patterns**: Follow conventions, ensure multi-tenancy
4. **Test Coverage**: Add tests that prevent regression
5. **Minimal Fix**: Targeted changes, not refactoring
6. **Track Progress**: Use TodoWrite for bug fix tasks

## Process

### 1. Issue Analysis & Context Sync

**Ensure `.agent_session/context.md` exists** (create with template below if not), read it closely, and capture prior decisions or TODOs before acting:
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

**Read bug issue**:
- GitHub: `GH_TOKEN=$(op.exe read "op://Employee/GitHub CLI PAT/token") gh issue view [number]`
- Extract: bug description, steps to reproduce, expected behavior, affected components

**Update context Overview** with bug summary

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

**Update `.agent_session/context.md`**:
- Document fix outcomes in Overview section
- Clear bug-related items from TODO section
- Append dated log entry: `[YYYY-MM-DD HH:MM TZ] fix-bug: fixed #[issue], [test results]`

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