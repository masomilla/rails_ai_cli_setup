---
description: Update /docs based on recent changes, keeping structure and language conventions (Codex)
---

# Update Documentation (Codex)

Refresh documentation in `/docs` to match current work.

## Rules
- Analyze git changes to decide what to document.
- Use Italian for narrative/headings; keep code/technical terms and comments in English.
- Prefer updating existing docs; if adding files, update `/docs/README.md` index.

## Process
1. **Understand changes**: run `git status`, inspect diffs/commits, and review context/plan files.
2. **Identify targets** (examples):
   - Features → `/docs/technical/features/`
   - Patterns/architecture → `/docs/technical/patterns/`
   - Integrations → `/docs/technical/integrations/`
   - API → `/docs/api/`
   - Business domain → `/docs/business/`
3. **Review existing docs**: read relevant files fully to mirror tone and structure.
4. **Draft updates**: include overview, technical details with real examples (models/controllers/jobs/tests), and cross-links using relative paths.
5. **Quality checks**: ensure Italian text reads well, code samples compile conceptually, and indexes are updated when new files are added.
6. **Share plan/summary with user** before finalizing if scope is large.
