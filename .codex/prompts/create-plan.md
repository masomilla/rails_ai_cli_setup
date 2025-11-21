---
description: Draft a detailed implementation plan for a Rails task using repo research (Codex)
arguments:
  - name: issue
    description: Issue number (e.g., 123) or pasted issue text; optional.
---

# Create Implementation Plan (Codex)

Build a practical plan for a Rails change by gathering context, researching the codebase, and writing an actionable document.

## Rules
- Read referenced issues/docs/files fully; prefer `rg` for searches.
- Follow Rails conventions, multi-tenancy patterns, and AGENTS.md guidance.
- Use Italian for user-facing plan text when appropriate; keep code/comments in English.

## Default Prompt (when no details provided)
```
Ti aiutero a creare un piano di implementazione dettagliato. Dimmi:
1) Descrizione del task o id issue
2) Vincoli o requisiti rilevanti
3) File o contesto da considerare
```

## Process
1. **Intake**:
   - If the argument is a GitHub issue number, immediately read it via `GH_TOKEN=$(op.exe read "op://Employee/GitHub CLI PAT/token") gh issue view [number]` and capture key details (requirements, acceptance criteria, constraints). No prompt is needed.
   - If the argument is free text, treat it as the issue description and extract requirements/acceptance criteria.
   - If no issue is provided, ask for details using the default prompt.
2. **Research**: scan the repo (controllers/models/views/jobs/tests) using `rg` and read full files that match. Note existing patterns and risks.
3. **Design options**: outline current behavior vs desired outcome, list possible approaches with pros/cons, and resolve open questions with the user when needed.
4. **Write the plan** to `.agent_session/plan.md` using this structure:
```
# Piano di Implementazione [Titolo]

## Panoramica
- Obiettivo e valore
- Stato attuale sintetico

## Stato Finale Desiderato
- Comportamento atteso e come verificarlo

## Cosa NON facciamo
- Punti esplicitamente fuori scope

## Fasi
### Fase 1: [Nome]
- Modifiche previste (file e note chiave)
- Strategia tecnica
- Criteri di successo
  - Verifica automatica: es. `bin/rails test`, `bearer scan .`
  - Verifica manuale: scenari UI o edge case

[Ripeti per altre fasi]

## Strategia di Test
- Unit, integration, (system solo se necessario) con casi principali

## Rischi e Open Questions
- Rischi residui e decisioni aperte (idealmente vuoto al termine)
```
5. **Review with user**: share the plan location, call out assumptions/risks, and wait for approval before implementation.
