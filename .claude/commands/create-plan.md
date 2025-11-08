---
name: create-plan
description: Creates a detailed implementation plan for a GitHub issue through interactive analysis, using specialized agents to research the codebase and design the solution. Maintains context in .agent_session/context.md to reduce token consumption by 70%.
---

You are tasked with creating detailed implementation plans through an interactive, iterative process in the Ruby on Rails application. You should be skeptical, thorough, and work collaboratively with the user to produce high-quality technical specifications.

## Rules
- **CRITICAL**: Before ANY work, check if `.agent_session/context.md` exists. If not, create it immediately
- **MANDATORY**: Update `.agent_session/context.md` BEFORE spawning ANY sub-agent task
- The context file MUST contain:
  - issue summary and requirements
  - Key discoveries from research
  - Design decisions made
  - Current implementation status
  - Open questions and blockers
- This reduces token consumption by 70% as agents don't need to re-read everything

## Context File Structure

The `.agent_session/context.md` file MUST follow this structure:

```markdown
# Session Context: [Feature Name]
Last Updated: [timestamp]

## issue Summary
- **ID**: #[number]
- **Requirement**: [one-line summary]
- **Scope**: [what's included]
- **Out of Scope**: [what's NOT included]

## Key Discoveries
- **Existing Patterns**: [what we found]
- **Files Involved**: [list of key files with purpose]
- **Current Implementation**: [how it works now]

## Design Decisions
- **Approach**: [chosen approach]
- **Rationale**: [why this approach]
- **Alternatives Considered**: [other options]

## Open Questions
- [ ] [question needing answer]
- [ ] [another question]
```

## Initial Response

When this command is invoked:

1. **Check if parameters were provided**:
   - If a Github ID issue reference was provided as a parameter, skip the default message
   - Immediately read any provided files FULLY
   - Begin the research process

2. **If no parameters provided**, respond with:
```
Ti aiuterò a creare un piano di implementazione dettagliato. Iniziamo capendo cosa stiamo costruendo.

Per favore fornisci:
1. La descrizione del task/issue (o riferimento a una Github issue)
2. Qualsiasi contesto rilevante, vincoli o requisiti specifici
3. Link a ricerche correlate o implementazioni precedenti

Analizzerò queste informazioni e lavorerò con te per creare un piano completo.

Suggerimento: Puoi anche invocare questo comando direttamente con un issue ID: `#1008`
Per un'analisi più approfondita, prova: `/create_plan think deeply about issue #1008`
```

Then wait for the user's input.

## Process Steps

### Step 1: Context Gathering & Initial Analysis

1. **Read all mentioned files immediately and FULLY**:
   - Github issue using: `GH_TOKEN=$(op.exe read "op://Employee/GitHub CLI PAT/token") gh issue view [number]`
   - Note: The token is stored in 1Password, ensure it's unlocked on desktop
   - Extract and analyze:
     * **Functional requirements**: What the feature must do
     * **Domain entities**: Models, controllers, and services involved
     * **User interactions**: UI flows, forms, and user journey
     * **Data flow**: How data moves through the system
     * **Integration points**: External services, APIs, or background jobs
     * **Non-functional requirements**: Performance, security, multi-tenancy constraints
     * **Acceptance criteria**: How to verify the feature works correctly
   - Research related documentation and context files
   - **IMPORTANT**: Use the Read tool WITHOUT limit/offset parameters to read entire files
   - **CRITICAL**: DO NOT spawn sub-tasks before reading these files yourself in the main context
   - **NEVER** read files partially - if a file is mentioned, read it completely

2. **Spawn initial research tasks to gather context**:
   Before asking the user any questions, use specialized agent to research in parallel:

   - Use the **codebase-locator** agent to find all files related to the issue/task
   - Use the **codebase-analyzer** agent to understand how the current implementation works
   
   These agent will:
   - Find relevant source files, configs, and tests
   - Identify the specific directories to focus on
   - Trace data flow and key functions
   - Return detailed explanations with file:line references

3. **Read all files identified by research tasks**:
   - After research tasks complete, read ALL files they identified as relevant
   - Read them FULLY into the main context
   - This ensures you have complete understanding before proceeding

4. **Analyze and verify understanding**:
   - Cross-reference the issue requirements with actual code
   - Identify any discrepancies or misunderstandings
   - Note assumptions that need verification
   - Determine true scope based on codebase reality

### Step 2: Design

After getting initial clarifications:

1. **If the user corrects any misunderstanding**:
   - DO NOT just accept the correction
   - Spawn new research tasks to verify the correct information
   - Read the specific files/directories they mention
   - Only proceed once you've verified the facts yourself

2. **Create a design todo list** using TodoWrite to track exploration tasks

3. **Spawn parallel sub-tasks for comprehensive design**:
   - Create multiple Task agents to design different aspects concurrently
   - Use the right agent for each type of design:

   **For deeper investigation:**
   - **codebase-locator** - To find Rails files (models, controllers, views, jobs) related to a feature or component
   - **codebase-analyzer** - To understand implementation details (e.g., "analyze how [system] works")
   - **web-researcher** - To research modern Rails patterns, gem documentation, or technical solutions

   **For architecture design:**
   - **model-specialist** - To design ActiveRecord models, associations, and database schema following Fat Model philosophy
   - **frontend-specialist** - To design ERB views, controllers, and TailwindCSS following Rails conventions
   - **hotwire-specialist** - To design Turbo Frames/Streams and Stimulus controllers for dynamic UI
   - **job-specialist** - To design Solid Queue background jobs with proper tenant isolation and retry logic

   **For quality assurance:**
   - **test-specialist** - To design Minitest tests, fixtures, and system tests following Rails testing patterns
   
   Each agent knows how to:
   - Find the right files and code patterns
   - Identify conventions and patterns to follow
   - Look for integration points and dependencies
   - Return specific file:line references
   - Find tests and examples

3. **Wait for ALL sub-tasks to complete** before proceeding

4. **Present findings and design options**:
   ```
   Basandomi sulla mia ricerca, ecco cosa ho trovato:

   **Stato Attuale:**
   - [Scoperta chiave sul codice esistente]
   - [Pattern o convenzione da seguire]

   **Opzioni di Design:**
   1. [Opzione A] - [pro/contro]
   2. [Opzione B] - [pro/contro]

   **Domande Aperte:**
   - [Incertezza tecnica]
   - [Decisione di design necessaria]

   Quale approccio si allinea meglio con la tua visione?
   ```

### Step 3: Detailed Plan Writing

1. **Write the plan** to `.agent_session/plan.md`
2. **Use this template structure**:

````markdown
# Piano di Implementazione [Nome Feature/Task]

## Panoramica

[Breve descrizione di cosa stiamo implementando e perché]

## Analisi dello Stato Attuale

[Cosa esiste ora, cosa manca, vincoli chiave scoperti]

## Stato Finale Desiderato

[Una specifica dello stato finale desiderato dopo il completamento di questo piano e come verificarlo]

### Scoperte Chiave:
- [Scoperta importante con riferimento file:riga]
- [Pattern da seguire]
- [Vincolo entro cui lavorare]

## Cosa NON Stiamo Facendo

[Elencare esplicitamente gli elementi fuori scope per prevenire l'ampliamento del progetto]

## Approccio all'Implementazione

[Strategia di alto livello e ragionamento]

## Fase 1: [Nome Descrittivo]

### Panoramica
[Cosa realizza questa fase]

### Modifiche Richieste:

#### 1. [Componente/Gruppo di File]
**File**: `path/to/file.ext`
**Modifiche**: [Riassunto delle modifiche]

```[linguaggio]
// Codice specifico da aggiungere/modificare
```

### Criteri di Successo:

#### Verifica Automatica:
- [ ] La migrazione si applica correttamente: `bin/rails db:migrate`
- [ ] I test passano: `bin/rails test`
- [ ] I test di sistema passano: `bin/rails test:system`
- [ ] La verifica di sicurezza statica del codice passa: `bearer scan .`

#### Verifica Manuale:
- [ ] La funzionalità funziona come previsto quando testata via UI
- [ ] Le prestazioni sono accettabili sotto carico
- [ ] La gestione dei casi limite verificata manualmente
- [ ] Nessuna regressione nelle funzionalità correlate

---

## Fase 2: [Nome Descrittivo]

[Struttura simile con criteri di successo sia automatici che manuali...]

---

## Strategia di Test

### Test Unitari:
- [Cosa testare]
- [Casi limite chiave]

### Test di Integrazione:
- [Scenari end-to-end]

### Passi di Test Manuali:
1. [Passo specifico per verificare la funzionalità]
2. [Altro passo di verifica]
3. [Caso limite da testare manualmente]

## Considerazioni sulle Prestazioni

[Eventuali implicazioni sulle prestazioni o ottimizzazioni necessarie]

## Note sulla Migrazione

[Se applicabile, come gestire dati/sistemi esistenti]

## Riferimenti

- issue originale: `github issue #XXXX`
- Ricerca correlata: `.agent_session/[pertinente].md`
- Implementazione simile: `[file:riga]`
````

### Step 4: Review

1. **Present the draft plan location**:
   ```
   Ho creato il piano di implementazione iniziale in:
   `.agent_session/plan.md`

   Per favore, revisionalo e fammi sapere:
   - Le fasi sono ben definite nel loro ambito?
   - I criteri di successo sono abbastanza specifici?
   - Ci sono dettagli tecnici che necessitano di aggiustamenti?
   - Mancano casi limite o considerazioni?
   ```

3. **Iterate based on feedback** - be ready to:
   - Add missing phases
   - Adjust technical approach
   - Clarify success criteria (both automated and manual)
   - Add/remove scope items

4. **Continue refining** until the user is satisfied

## Important Guidelines

1. **Be Skeptical**:
   - Question vague requirements
   - Identify potential issues early
   - Ask "why" and "what about"
   - Don't assume - verify with code

2. **Be Interactive**:
   - Don't write the full plan in one shot
   - Get buy-in at each major step
   - Allow course corrections
   - Work collaboratively

3. **Be Thorough**:
   - Read all context files COMPLETELY before planning
   - Research actual code patterns using parallel sub-tasks
   - Design using parallel sub-tasks
   - Include specific file paths and line numbers
   - Write measurable success criteria with clear automated vs manual distinction

4. **Be Practical**:
   - Focus on incremental, testable changes
   - Consider migration and rollback
   - Think about edge cases
   - Include "what we're NOT doing"

5. **Track Progress**:
   - Use TodoWrite to track planning tasks
   - Update todos as you complete research
   - Mark planning tasks complete when done

6. **No Open Questions in Final Plan**:
   - If you encounter open questions during planning, STOP
   - Research or ask for clarification immediately
   - Do NOT write the plan with unresolved questions
   - The implementation plan must be complete and actionable
   - Every decision must be made before finalizing the plan

## Success Criteria Guidelines

**Always separate success criteria into two categories:**

1. **Automated Verification** (can be run by execution agents):
   - Commands that can be run: `bin/rails test`, `bin/rails test:system`, etc.
   - Specific files that should exist
   - Code compilation/type checking
   - Automated test suites
   - Security scan code: `bearer scan .`

2. **Manual Verification** (requires human testing):
   - UI/UX functionality
   - Performance under real conditions
   - Edge cases that are hard to automate
   - User acceptance criteria

**Format example:**
```markdown
### Criteri di Successo:

#### Verifica Automatica:
- [ ] La migrazione del database viene eseguita con successo: `bin/rails db:migrate`
- [ ] Tutti i test unitari passano: `bin/rails test`
- [ ] Tutti i test di sistema passano: `bin/rails test:system`
- [ ] Sicurezza statica: `bearer scan .`


#### Verifica Manuale:
- [ ] La nuova funzionalità appare correttamente nell'UI
- [ ] Le prestazioni sono accettabili con 1000+ elementi
- [ ] I messaggi di errore sono user-friendly
- [ ] La funzionalità funziona correttamente su dispositivi mobili
```

## Common Patterns

### For Database Changes:
- Start with schema/migration
- Add store methods
- Update business logic
- Expose via API
- Update clients

### For New Features:
- Research existing patterns first
- Start with data model (migration)
- Build backend logic (models)
- Implement controllers and views
- Enhance with Hotwire

### For Refactoring:
- Document current behavior
- Plan incremental changes
- Maintain backwards compatibility
- Include migration strategy

## Sub-task Spawning Best Practices

When spawning research sub-tasks:

1. **Spawn multiple tasks in parallel** for efficiency
2. **Each task should be focused** on a specific area
3. **Provide detailed instructions** based on agent type:
   **For search/analysis agents (codebase-locator, codebase-analyzer)**:
   - Exactly what to search for
   - Which directories to focus on
   - What information to extract
   - Expected output format with file:line references

   **For design specialist agents (model-specialist, frontend-specialist, hotwire-specialist, job-specialist, test-specialist)**:
   - The specific requirement from the issue
   - Existing patterns to follow
   - **MUST**: Update `.agent_session/context.md` section "For Sub-Agents" before spawning
4. **Be EXTREMELY specific about directories**:
   - If the issue mentions "models", specify `app/models/` directory
   - If it mentions "controllers", specify `app/controllers/` directory
   - If it mentions "background jobs", specify `app/jobs/` directory
   - If it mentions "views" or "UI", specify `app/views/` and `app/javascript/controllers/`
   - Include the full Rails path context in your prompts
5. **Specify read-only tools** to use
6. **Request specific file:line references** in responses
7. **Wait for all tasks to complete** before synthesizing
8. **Verify sub-task results**:
   - If a sub-task returns unexpected results, spawn follow-up tasks
   - Cross-check findings against the actual codebase
   - Verify multi-tenant isolation patterns are followed
   - Check that Rails conventions are respected

## Example Interaction Flow

```
User: /create_plan
Assistant: Ti aiuterò a creare un piano di implementazione dettagliato...

[FIRST ACTION: Create/Update .agent_session/context.md]

User: Dobbiamo aggiungere una feature per l'export dei contatti. Vedi github issue #1234
Assistant: Fammi prima leggere completamente il issue...

[Reads issue]
[Updates context.md with issue summary]

Ora invoco gli agent specialisti per analizzare il codebase e progettare la soluzione...

[Updates context.md section "For Sub-Agents" with specific instructions]
[Spawns multiple agents with: "IMPORTANTE: Leggi PRIMA .agent_session/context.md per il contesto completo"]

[Agents read context.md instead of re-reading everything, saving 70% tokens]

[Interactive process continues...]
```