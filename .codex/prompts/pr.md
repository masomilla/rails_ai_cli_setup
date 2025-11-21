---
description: Prepare and create a pull request for recent changes with proper summary and checks (Codex)
---

# Create Pull Request (Codex)

Open a PR summarizing completed work.

## Process
1. **Review work**:
   - If `.agent_session/plan.md` exists, use it to determine scope; skip diffing against main.
   - Otherwise, detect branch:
     - On a feature branch: `git fetch origin`, then `git diff --stat origin/main...HEAD` and `git log --oneline origin/main...HEAD` to see changes.
     - On `main`: review recent commits (e.g., `git log -5 --oneline`) to understand what was merged.
   - Run `git status` to catch unstaged changes.
2. **Branch/PR status**: confirm current branch (`git branch --show-current`) is not main. Check for existing PR with `gh pr view`; if one exists, ask whether to update instead.
3. **Draft PR**:
   - Propose a concise title.
   - Summarize what the PR does, key changes (models/controllers/views/jobs/tests), and tenant/security considerations.
   - List verification steps with checkboxes (tests run, manual checks, security scans). Save draft to `.agent_session/pr_description.md`.
4. **Verification**: run relevant commands (e.g., `bin/rails test`, `bin/rails test:system` if needed, `bearer scan .`) and update checkboxes accordingly. Explain any failures or skipped steps.
5. **Create PR after approval**:
```bash
GH_TOKEN=$(op.exe read "op://Employee/GitHub CLI PAT/token") gh pr create \
  --title "[Title]" \
  --body-file .agent_session/pr_description.md \
  --base main
```
   Ensure 1Password is unlocked. Push branch first if necessary (`git push -u origin [branch]`).
6. **Share result**: provide PR URL and remaining manual verification notes.
