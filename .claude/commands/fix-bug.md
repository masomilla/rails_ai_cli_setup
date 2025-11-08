---
name: fix-bug
description: Reads a GitHub issue or task description for a bug, creates an implementation plan, and immediately executes the fix. Combines planning and implementation in a streamlined workflow for bug fixes in Rails applications.
---

# Fix Bug

You are tasked with fixing bugs in the Ruby on Rails application by reading the issue, planning the fix, and implementing it in a single streamlined workflow.

## Rules
- **CRITICAL**: Before ANY work, check if `.agent_session/context.md` exists. If not, create it immediately
- **MANDATORY**: Update `.agent_session/context.md` throughout the process
- Follow Rails conventions and multi-tenancy patterns
- Write tests to verify the fix works
- All comments MUST be written in English
- Git messages MUST be written in English

## Process

### Step 1: Issue Analysis & Context Setup

When invoked:

1. **Create/Read context file**:
   - Check `.agent_session/context.md` for any existing context
   - Create it if missing with bug fix session information

2. **Read the bug issue**:
   - If GitHub issue ID provided, use:
     ```bash
     GH_TOKEN=$(op.exe read "op://Employee/GitHub CLI PAT/token") gh issue view [number]
     ```
   - Note: The token is stored in 1Password, ensure it's unlocked on desktop
   - If task description provided, analyze the bug description
   - Extract and analyze:
     * **Bug description**: What is broken and how it manifests
     * **Steps to reproduce**: How to trigger the bug
     * **Expected behavior**: What should happen instead
     * **Affected components**: Models, controllers, views, jobs involved
     * **User impact**: How this affects users and business logic
     * **Environment details**: Any specific conditions or data states

3. **Update context with bug information**:
   ```markdown
   # Bug Fix Session: [Bug Title]
   Last Updated: [timestamp]

   ## Bug Summary
   - **Issue ID**: #[number]
   - **Problem**: [one-line description of the bug]
   - **Impact**: [user/business impact]
   - **Severity**: [critical/high/medium/low]

   ## Steps to Reproduce
   1. [step 1]
   2. [step 2]
   3. [expected vs actual behavior]

   ## Affected Components
   - [list of Rails files/components involved]
   ```

### Step 2: Bug Investigation & Root Cause Analysis

1. **Use specialized agents for investigation**:
   ```
   Task 1 - Locate bug-related code:
   Use codebase-locator to find all files related to the bug symptoms.
   Focus on: app/models/, app/controllers/, app/views/, app/jobs/
   Look for: [specific components mentioned in the issue]
   Return: List of relevant files with descriptions

   Task 2 - Analyze current implementation:
   Use codebase-analyzer to understand how the broken functionality works.
   Trace the code path from user action to bug manifestation.
   Check: business logic, data flow, validations, multi-tenant scoping
   Return: Step-by-step analysis of what's happening vs what should happen
   ```

2. **Read all identified files completely**:
   - Read every file the agents identified as relevant
   - Understand the current implementation thoroughly
   - Identify the root cause of the bug

3. **Update context with findings**:
   ```markdown
   ## Root Cause Analysis
   - **Problem Location**: [file:line where the bug originates]
   - **Root Cause**: [technical explanation of why it's broken]
   - **Contributing Factors**: [any related issues or edge cases]

   ## Files Involved
   - **Primary**: `[file]` - [what's wrong here]
   - **Secondary**: `[file]` - [related code that may need adjustment]
   ```

### Step 3: Solution Planning

1. **Design the fix approach**:
   - Use specialized agents for deeper analysis if needed:
     ```
     Task 1 - Analyze similar implementations:
     Use codebase-locator to find similar functionality in the codebase.
     Search for: [similar features/patterns that work correctly]
     Return: Examples of correct implementations to model the fix after

     Task 2 - Understand architectural context:
     Use codebase-analyzer to understand how this component fits in the broader system.
     Analyze: data flow, dependencies, integration points
     Return: Architectural context and potential impact areas
     ```
   - Determine the minimal change needed to fix the bug
   - Consider edge cases and potential side effects
   - Ensure multi-tenant compatibility
   - Plan test strategy to verify the fix

2. **Create bug fix todo list**:
   - Break down the fix into specific tasks
   - Include tests to verify the fix works
   - Plan verification steps

3. **Update context with solution**:
   ```markdown
   ## Solution Plan
   - **Approach**: [high-level fix strategy]
   - **Changes Required**: [specific modifications needed]
   - **Testing Strategy**: [how to verify the fix]

   ## Implementation Tasks
   1. [task 1]
   2. [task 2]
   3. [test verification]
   ```

### Step 4: Implementation

1. **Get implementation guidance from specialists if needed**:
   ```
   Use specialized agents based on the component being fixed:

   - **model-specialist**: For ActiveRecord models, business logic, validations
   - **frontend-specialist**: For controllers, views, ERB templates
   - **hotwire-specialist**: For Turbo Frame/Stream or Stimulus issues
   - **job-specialist**: For Solid Queue background job problems
   - **test-specialist**: For test strategy and implementation
   ```

2. **Implement the fix following Rails order**:

   **Model Layer** (if needed):
   - Fix business logic, validations, associations
   - Ensure `acts_as_tenant(:account)` compatibility
   - Add/modify scopes or methods

   **Controller Layer** (if needed):
   - Fix action logic, parameter handling
   - Update Pundit authorization if needed
   - Handle Turbo Stream responses correctly

   **View Layer** (if needed):
   - Fix ERB templates, Turbo Frame issues
   - Update Stimulus controllers
   - Ensure responsive design

   **Background Jobs** (if needed):
   - Fix job logic, tenant context handling
   - Update retry and error handling

2. **Write/Update tests**:
   - Add test cases that reproduce the bug
   - Verify the fix resolves the issue
   - Add regression tests to prevent future occurrences
   - Test edge cases and multi-tenant scenarios

3. **Run verification**:
   ```bash
   # Test the fix
   bin/rails test
   bin/rails test:system

   # Code quality
   bin/rubocop
   bearer scan .

   # Manual verification
   bin/rails console
   # Test the specific bug scenario
   ```

4. **Update context throughout implementation**:
   - Mark tasks as completed
   - Note any deviations from the plan
   - Document verification results

### Step 5: Final Verification & Documentation

1. **Comprehensive testing**:
   - Verify the original bug is fixed
   - Test related functionality isn't broken
   - Confirm multi-tenant isolation still works
   - Check performance implications

2. **Update context with completion status**:
   ```markdown
   ## Implementation Status
   - [x] Root cause identified and fixed
   - [x] Tests added and passing
   - [x] Code quality checks pass
   - [x] Manual verification completed

   ## Verification Results
   - **Bug reproduction**: [can no longer reproduce the issue]
   - **Test coverage**: [new tests added at [files]]
   - **Side effects**: [none found / [specific items checked]]
   ```

3. **Present completion summary**:
   ```
   🐛 Bug Fix Complete!

   **Issue**: [brief description]
   **Root Cause**: [what was wrong]
   **Solution**: [what was fixed]
   **Files Modified**: [list of changed files]
   **Tests Added**: [test files created/updated]

   The bug has been fixed and verified. Ready for commit and PR creation.
   ```

## Bug Fix Guidelines

### For Rails-Specific Bugs:
- **Multi-tenancy**: Always verify tenant isolation isn't broken
- **Performance**: Check for N+1 queries or slow operations
- **Security**: Ensure Pundit policies are still enforced
- **Data integrity**: Verify database constraints and validations
- **Hotwire**: Test Turbo Frame/Stream functionality if UI-related

### Common Bug Types:
- **Logic errors**: Incorrect business rules or calculations
- **Data issues**: Wrong queries, missing scopes, tenant leakage
- **UI bugs**: Broken Turbo interactions, form submission issues
- **Performance**: Slow queries, memory leaks, inefficient algorithms
- **Security**: Authorization bypass, data exposure, XSS vulnerabilities

### Verification Commands:
```bash
# Reproduce the bug (before fix)
bin/rails console
# [steps to reproduce from issue]

# After fix - verify it's resolved
bin/rails test
bin/rails test:system

# Check for regressions
bin/rails test [related_test_files]

# Performance check (if relevant)
# Use rails console to test with realistic data volumes
```

## When You Get Stuck

During any step, if you encounter difficulties:

1. **Use specialized agents for help**:
   - **codebase-locator**: Find similar patterns or examples in the codebase
   - **codebase-analyzer**: Understand complex code flows or dependencies
   - **model-specialist**: For ActiveRecord and business logic issues
   - **frontend-specialist**: For controller and view problems
   - **hotwire-specialist**: For Turbo/Stimulus debugging
   - **job-specialist**: For background job issues
   - **test-specialist**: For testing strategy and implementation

2. **Rails-specific debugging**:
   ```bash
   # Check Rails logs for clues
   tail -f log/development.log

   # Use Rails console to reproduce and test
   bin/rails console
   # Test tenant scoping: ActsAsTenant.current_tenant
   # Reproduce bug conditions step by step

   # Check database state
   bin/rails dbconsole
   ```

3. **Present the issue clearly**:
   ```
   🐛 Bug Fix Assistance Needed:
   Step: [which step you're stuck on]
   Component: [Model/Controller/View/Job]
   Issue: [specific problem you're encountering]
   Attempted: [what you've tried so far]
   Files involved: [list of relevant files]

   What guidance do you need?
   ```

## Important Notes:
- Focus on minimal, targeted fixes rather than large refactors
- Always add tests that would catch this bug in the future
- Consider the impact on existing functionality
- Document any assumptions or limitations of the fix
- If the fix is complex, break it into smaller, testable parts
- Use agents proactively - they can provide valuable insights and examples

Remember: A good bug fix not only resolves the immediate issue but also prevents similar problems from occurring in the future through proper testing and defensive coding practices.