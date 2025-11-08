---
name: doc
description: Updates documentation in /docs based on changes in the current branch. Analyzes git modifications, understands existing documentation structure, and updates appropriate docs for Rails features, patterns, and integrations.
---

# Update Documentation

You are tasked with updating documentation in `/docs` based on changes made in the current branch, similar to how other commands work with git analysis.

## Rules
- **CRITICAL**: Read `.agent_session/context.md` for session context if available
- Analyze git changes to understand what needs documenting
- Follow existing documentation structure and patterns in `/docs`
- Use Italian for headings and descriptions, English for code and technical terms
- Update existing docs rather than create new ones when possible
- Always update `/docs/README.md` index when adding new files

## Process

### Step 1: Analyze Changes and Context

1. **Understand what was implemented:**
   - Read `.agent_session/context.md` for session context if available
   - If task description provided, analyze requirements and scope
   - Run `git status` to see current changes
   - Run `git diff HEAD~1..HEAD` or appropriate range to see committed changes
   - Run `git log --oneline -n 10` to see recent commit messages

2. **Identify documentation impact:**
   - **New Rails features** → `/docs/technical/features/`
   - **New patterns or architectural decisions** → `/docs/technical/patterns/`
   - **New integrations** → `/docs/technical/integrations/`
   - **API changes** → `/docs/api/`
   - **Business domain changes** → `/docs/business/`
   - **Development process updates** → `/docs/development/`

### Step 2: Documentation Structure Analysis

1. **Read existing documentation structure:**
   - Review `/docs/README.md` to understand navigation
   - Identify which existing documents might need updates
   - Check related documents in the appropriate directory

2. **Determine documentation approach:**
   - **Update existing docs**: If feature extends existing functionality
   - **Create new document**: If entirely new feature/pattern/integration
   - **Multiple updates**: If changes span multiple areas

### Step 3: Documentation Creation/Update

1. **Read related existing documentation:**
   - Read completely any existing docs that need updates
   - Understand current structure, style, and content depth
   - Identify gaps or outdated information

2. **Create or update documentation:**
   - Follow existing patterns and structure in `/docs`
   - Use Rails-specific examples with real domain objects (Contact, Account, etc.)
   - Include complete, functional code examples with English comments
   - Start with business context in Italian, then technical details

3. **Quality verification:**
   - Ensure Italian grammar is correct in descriptions
   - Verify English technical terms are used appropriately
   - Test code examples for accuracy
   - Cross-reference related documents
   - Update main index if new files are created

### Step 4: Present Documentation Plan

Before making changes, present your analysis and plan:
```
📚 Documentation Update Plan

**Changes Detected:**
- [List of key changes found in git]

**Documentation Impact:**
- [Existing docs to update: file paths]
- [New docs to create: file paths and rationale]

**Approach:**
- [Brief explanation of documentation strategy]

Shall I proceed with updating the documentation?
```

## Rails Documentation Structure

The `/docs` directory is organized as follows:

### Directory Structure
- **`/business/`** - Domain overview, features, user roles, glossary
- **`/architecture/`** - System architecture and design overview
- **`/technical/`** - Technical implementation details:
  - **`patterns/`** - Architectural patterns and best practices
  - **`features/`** - Specific feature implementations
  - **`integrations/`** - External service integrations
- **`/api/`** - REST API documentation
- **`/development/`** - Setup and coding conventions

### Documentation Mapping for Rails Changes

**Database changes** (migrations, schema):
- Update `/docs/technical/features/core-models.md`
- Update `/docs/technical/patterns/activerecord-patterns.md`

**New models or business logic**:
- Update `/docs/business/domain-overview.md`
- Create/update docs in `/docs/technical/features/`

**Controller and view changes**:
- Update `/docs/technical/patterns/hotwire-patterns.md`
- Update `/docs/technical/patterns/frontend-architecture.md`

**Background jobs**:
- Update `/docs/technical/patterns/background-jobs.md`

**External integrations**:
- Create/update docs in `/docs/technical/integrations/`

**API changes**:
- Update `/docs/api/rest-api.md`

**Multi-tenancy changes**:
- Update `/docs/technical/features/multi-tenancy-implementation.md`

### When to Document Changes

**Document these types of changes:**
- New Rails features or significant functionality
- New architectural patterns or design decisions
- External service integrations
- API endpoint additions or modifications
- Multi-tenancy implementation changes
- New background job patterns
- Hotwire/Turbo integration patterns

**Skip documentation for:**
- Standard Rails CRUD operations
- Framework-default behaviors
- Minor bug fixes
- Environment-specific configurations
- Temporary implementations

### Documentation Standards

1. **Language Usage:**
   - Italian for headings, descriptions, and business context
   - English for code, technical terms, file paths, and ALL code comments
   - Natural integration of technical English terms in Italian narrative

2. **Content Structure:**
   - Start with "Panoramica" (Overview) section
   - Business context first, then technical implementation
   - Include complete, functional code examples
   - Use real domain objects (Contact, Account, Integration)

3. **Code Examples:**
   - Complete implementations, not fragments
   - Proper syntax highlighting (ruby, javascript, erb, sql)
   - English comments in all code
   - Include error handling and edge cases

4. **Cross-referencing:**
   - Use relative paths: `[text](../folder/file.md)`
   - Meaningful link text
   - Update `/docs/README.md` when adding new files
   - Link to related documents bidirectionally

## Execution Guidelines

1. **Always get user confirmation** before making documentation changes
2. **Update existing documents** when possible rather than creating new ones
3. **Follow existing patterns** in the documentation structure
4. **Test code examples** for accuracy
5. **Save documentation** directly to appropriate `/docs` subdirectories

## Common Rails Documentation Patterns

### For New Features:
```markdown
# Nome Funzionalità

## Panoramica
[Business context in Italian]

## Implementazione Tecnica
[Technical details with code examples]

## Esempi di Utilizzo
[Real-world usage examples]

## Considerazioni
[Performance, security, multi-tenancy notes]
```

### For Integration Updates:
- Update existing integration docs with new endpoints or functionality
- Add authentication or configuration changes
- Include new error handling or retry logic

### For Pattern Documentation:
- Document the pattern with real Rails examples
- Explain when to use vs. when not to use
- Include performance and maintenance considerations

Remember: Documentation should help developers understand both the "what" and the "why" of implementations, using real project examples and maintaining consistency with existing docs.