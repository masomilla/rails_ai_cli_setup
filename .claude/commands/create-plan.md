---
name: create-plan
description: Creates a detailed implementation plan for a GitHub issue through interactive analysis, using specialized agents to research the codebase and design the solution. Maintains context in .agent_session/context.md to reduce token consumption by 70%.
---

Create detailed implementation plans through interactive analysis in this Rails application. Be skeptical, thorough, and collaborative.

## Usage

```bash
/create-plan [issue-id|description]
```

**Parameters:**
- `issue-id` (optional): GitHub issue number (e.g., `123`) - will fetch issue details using `gh issue view`
- `description` (optional): Free text description of the feature to implement

**Examples:**
```bash
/create-plan 456                           # Plan from GitHub issue #456
/create-plan                                # Interactive mode - will ask for details
/create-plan Add user profile export       # Plan from text description
```

## Core Rules

1. **Context Management**: Maintain `.agent_session/context.md` as the shared state file: read it fully before working. If missing, create it with the standard template.
2. **Update Log**: After each run, refresh the `Overview/Decisions/TODO` sections and append a new dated log entry.
3. **Read Fully**: Always read files completely (no limits/offsets) before making decisions
4. **No Unresolved Questions**: Final plan must have zero open questions - research or ask immediately
5. **Parallel Work**: Spawn multiple specialized agents concurrently for efficiency
6. **Track Progress**: Use TodoWrite to track planning tasks
7. **Service Object**: No service object, use instead Concern or PORO

## Process

### 1. Context Setup & Research

**Ensure `.agent_session/context.md` exists** (create with the template below if not), read it closely, and capture prior decisions or TODOs before acting:
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

**Spawn parallel research** (update context.md first):
- **codebase-locator**: Find related Rails files (models/controllers/views/jobs)
- **codebase-analyzer**: Understand current implementation
- **web-researcher**: Research patterns/gems if needed

**Read all identified files fully** into main context

### 2. Design

**Spawn design specialists** based on needs:
- **model-specialist**: ActiveRecord models, schema, associations
- **frontend-specialist**: ERB views, controllers, TailwindCSS
- **hotwire-specialist**: Turbo Frames/Streams, Stimulus
- **rails-specialist**: Background jobs, Solid Queue
- **test-specialist**: Minitest, fixtures, system tests

**Present findings**:
```
**Stato Attuale**: [current behavior]
**Opzioni**: [approaches with pros/cons]
**Domande**: [clarifications needed]
```

### 3. Write Plan

Write to `.agent_session/plan.md` in Italian language:

````markdown
# Piano di Implementazione [Nome]

## Panoramica
[Cosa e perché]

## Stato Attuale
[Cosa esiste, vincoli scoperti]

## Stato Finale Desiderato
[Comportamento atteso e verifica]

### Scoperte Chiave
- [file:line references]
- [Patterns to follow]

## Cosa NON Facciamo
[Out of scope explicit]

## Fase 1: [Nome]

### Modifiche
**File**: `path/to/file.rb`
```ruby
# Specific changes
```

### Criteri di Successo

#### Verifica Automatica
- [ ] `bin/rails db:migrate`
- [ ] `bin/rails test`
- [ ] `bearer scan .`

#### Verifica Manuale
- [ ] UI functionality works
- [ ] Performance acceptable
- [ ] Edge cases handled

---

## Fase 2: [Nome]
[Ripeti struttura]

---

## Strategia di Test
- **Unit**: [focus areas]
- **Integration**: [end-to-end scenarios]
- **Manual**: [specific steps]

## Riferimenti
- Issue: #[number]
- Similar: [file:line]
````

### 4. Update Context & Review

**Update `.agent_session/context.md`**:
- Summarize plan highlights in Overview section
- Document key decisions in Decisions section
- List open items in TODO section
- Append dated log entry: `[YYYY-MM-DD HH:MM TZ] create-plan: created plan for [feature], [N] phases, waiting approval`

**Present to user**:
```
Piano in `.agent_session/plan.md`

Verifica:
- Fasi ben definite?
- Criteri sufficientemente specifici?
- Dettagli tecnici corretti?
- Casi limite coperti?
```

Iterate until approved.

## Guidelines

**Be Skeptical**: Question vague requirements, identify issues early, verify with code
**Be Interactive**: Get buy-in at each step, allow corrections
**Be Thorough**: Research with parallel agents, include file:line references
**Be Practical**: Incremental changes, consider migration/rollback, list "NOT doing"

## Agent Best Practices

**When spawning agents**:
1. Update context.md section "For Sub-Agents" first
2. Spawn multiple tasks in parallel
3. Be specific about directories (`app/models/`, `app/controllers/`)
4. Request file:line references
5. Wait for completion before synthesizing
6. Verify results match codebase reality

**Agent prompts should include**: "IMPORTANTE: Leggi PRIMA `.agent_session/context.md`"

## Success Criteria Format

Always separate:
- **Automated**: Commands (`bin/rails test`), file existence, compilation
- **Manual**: UI/UX, performance, edge cases, user acceptance

## Common Patterns

**Database Changes**: migration → models → business logic → API → clients
**New Features**: research patterns → model → backend → controllers/views → Hotwire
**Refactoring**: document current → incremental changes → maintain compatibility