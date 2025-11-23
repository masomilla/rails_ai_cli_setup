---
name: doc
description: Updates documentation in /docs based on changes in the current branch. Analyzes git modifications, understands existing documentation structure, and updates appropriate docs for Rails features, patterns, and integrations.
---

Update `/docs` documentation based on git changes. Analyze modifications, follow existing structure, use Italian for text and English for code.

## Usage

```bash
/doc [issue-id|description]
```

**Parameters:**
- `issue-id` (optional): GitHub issue number (e.g., `456`) - will fetch feature details using `gh issue view`
- `description` (optional): Free text description of the changes to document

**Examples:**
```bash
/doc 456                                  # Document changes from GitHub issue #456
/doc                                      # Auto-detect from context (plan.md or git diff)
/doc Add calendar sync feature           # Document changes from text description
```

**Information Source Priority:**
1. **GitHub Issue** - If issue ID provided: `GH_TOKEN=$(op.exe read "op://Employee/GitHub CLI PAT/token") gh issue view [number]`
2. **Text Description** - If free text provided: Use as changes description
3. **Implementation Plan** - Check `.agent_session/plan.md` AND `.agent_session/context.md` for feature details and context
4. **Git Diff** - Fallback: `git diff main...HEAD` to understand changes

## Core Rules

1. **Context First**: Maintain `.agent_session/context.md` as the shared state file: read it fully before working. If missing, create it with the standard template.
2. **Update Log**: After each run, refresh the `Overview/Decisions/TODO` sections and append a new dated log entry.
3. **Plan Reference**: Check `.agent_session/plan.md` if available to determine scope; skip diffing against main if plan exists.
4. **Update Over Create**: Prefer updating existing docs
5. **Language**: Italian (headings/descriptions), English (code/technical terms/comments)
6. **Index**: Update `/docs/README.md` when adding files
7. **Complete Examples**: Functional code with English comments

## Process

### 1. Context Sync & Analyze Changes

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

**Understand implementation**:
```bash
git status
git diff HEAD~1..HEAD
git log --oneline -n 10
```

**Check plan**: `.agent_session/plan.md` if available

**Map to documentation**:
- **Rails features** → `/docs/technical/features/`
- **Patterns/architectural** → `/docs/technical/patterns/`
- **Integrations** → `/docs/technical/integrations/`
- **API changes** → `/docs/api/`
- **Business domain** → `/docs/business/`
- **Development process** → `/docs/development/`

### 2. Structure Analysis

**Review existing**:
- Read `/docs/README.md`
- Identify docs needing updates
- Check related documents

**Determine approach**:
- Update existing (if extends functionality)
- Create new (if entirely new feature)
- Multiple updates (if spans areas)

### 3. Create/Update Documentation

**Read related docs** completely

**Write documentation**:
- Follow existing `/docs` patterns
- Rails examples with real domain objects (Contact, Account)
- Complete code with English comments
- Business context (Italian) → technical details

**Verify quality**:
- Italian grammar correct
- English technical terms appropriate
- Code examples accurate
- Cross-reference related docs
- Update `/docs/README.md` if new files

### 4. Present Plan & Update Context

```
📚 Documentation Plan

Changes:
- [git changes]

Impact:
- Update: [paths]
- Create: [paths + rationale]

Approach:
- [strategy]

Proceed?
```

**Update `.agent_session/context.md`** after completion:
- Document updated docs in Overview section
- Clear documentation TODOs
- Append dated log entry: `[YYYY-MM-DD HH:MM TZ] doc: updated [N] docs, [files list]`

## Documentation Structure

**Directories**:
- `/business/` - Domain, features, user roles, glossary
- `/architecture/` - System architecture
- `/technical/` - Implementation details
  - `patterns/` - Architectural patterns
  - `features/` - Feature implementations
  - `integrations/` - External services
- `/api/` - REST API
- `/development/` - Setup, conventions

**Change Mapping**:
- **Database** → `/docs/technical/features/core-models.md`, `/patterns/activerecord-patterns.md`
- **Models/business logic** → `/business/domain-overview.md`, `/technical/features/`
- **Controllers/views** → `/patterns/hotwire-patterns.md`, `/patterns/frontend-architecture.md`
- **Background jobs** → `/patterns/background-jobs.md`
- **Integrations** → `/technical/integrations/`
- **API** → `/api/rest-api.md`
- **Multi-tenancy** → `/features/multi-tenancy-implementation.md`

**Document when**:
- New Rails features
- Architectural patterns
- External integrations
- API modifications
- Multi-tenancy changes
- Background job patterns
- Hotwire integration

**Skip**:
- Standard CRUD
- Default behaviors
- Minor bug fixes
- Environment configs
- Temporary implementations

## Standards

**Language**:
- Italian: headings, descriptions, business context
- English: code, technical terms, file paths, ALL comments

**Structure**:
- Start with "Panoramica" (Overview)
- Business context → technical implementation
- Complete code examples
- Real domain objects (Contact, Account)

**Code**:
- Complete, not fragments
- Syntax highlighting (ruby, javascript, erb, sql)
- English comments
- Error handling, edge cases

**Cross-reference**:
- Relative paths: `[text](../folder/file.md)`
- Update `/docs/README.md` for new files

## Feature Documentation Template

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

**Goal**: Help developers understand "what" and "why" using real project examples.