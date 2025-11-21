---
name: create-plan
description: Creates a detailed implementation plan for a GitHub issue through interactive analysis, using specialized agents to research the codebase and design the solution. Maintains context in .agent_session/context.md to reduce token consumption by 70%.
---

Create detailed implementation plans through interactive analysis in this Rails application. Be skeptical, thorough, and collaborative.

## Core Rules

1. **Context Management**: Create/update `.agent_session/context.md` BEFORE any work with: issue summary, scope, discoveries, design decisions, open questions
2. **Read Fully**: Always read files completely (no limits/offsets) before making decisions
3. **No Unresolved Questions**: Final plan must have zero open questions - research or ask immediately
4. **Parallel Work**: Spawn multiple specialized agents concurrently for efficiency
5. **Track Progress**: Use TodoWrite to track planning tasks
6. **Service Object**: No service object, use insted Concern or PORO 

## Process

### 1. Context Setup & Research

**Create `.agent_session/context.md`**:
```markdown
# Session Context: [Feature Name]
Last Updated: [timestamp]

## Issue Summary
- **ID**: #[number]
- **Requirement**: [summary]
- **Scope**: [included]
- **Out of Scope**: [excluded]

## Key Discoveries
- **Existing Patterns**: [findings]
- **Files Involved**: [paths with purpose]
- **Current Implementation**: [how it works]

## Design Decisions
- **Approach**: [chosen]
- **Rationale**: [why]
- **Alternatives**: [other options]

## Open Questions
- [ ] [unresolved items]
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

### 4. Review

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