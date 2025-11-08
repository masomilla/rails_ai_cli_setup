---
name: pr
description: Creates a new pull request for Rails features based on completed implementation, analyzing changes from git and context files to generate appropriate title and description. Follows Rails conventions and multi-tenancy patterns.
---

# Create Pull Request

You are tasked with creating a pull request for Ruby on Rails features that have been implemented, similar to how the commit command works for git commits.

## Process:

1. **Understand what was implemented:**
   - Read `.agent_session/context.md` to understand the feature implemented
   - Check `.agent_session/plan.md` if it exists for planned vs actual changes
   - Review conversation history to understand what was accomplished
   - Run `git status` to see current changes
   - Run `git diff HEAD~1..HEAD` or appropriate range to see committed changes
   - Run `git log --oneline -n 10` to see recent commits

2. **Verify current branch and PR status:**
   - Check current branch: `git branch --show-current`
   - Verify no PR exists yet:
     ```bash
     GH_TOKEN=$(op.exe read "op://Employee/GitHub CLI PAT/token") gh pr view --json url,number,title,state 2>/dev/null
     ```
   - If already on main, ask user to create feature branch first
   - If PR already exists, inform user and ask if they want to update it

3. **Analyze Rails changes implemented:**
   - **Database changes**: Look at recent migrations in `db/migrate/`
   - **Model layer**: Check new/modified models in `app/models/` with `acts_as_tenant`
   - **Controller layer**: Review controllers in `app/controllers/` for REST patterns
   - **View layer**: Check `app/views/` for ERB templates and Turbo integration
   - **JavaScript**: Review `app/javascript/controllers/` for Stimulus controllers
   - **Background jobs**: Check `app/jobs/` for Solid Queue jobs
   - **Tests**: Verify test coverage in `test/` directories
   - **Security**: Check Pundit policies in `app/policies/`
   - **Localization**: Check Italian translations in `config/locales/`

4. **Present PR plan to user:**
   - Show proposed PR title
   - Show target branch (usually `main`)
   - Present draft PR description with Rails-specific sections
   - Ask: "I plan to create a PR with this title and description. Shall I proceed?"

5. **Run verification checks:**
   - Run automated Rails checks to verify implementation:
     ```bash
     # Test suite
     bin/rails test
     bin/rails test:system

     # Code quality
     bin/rubocop
     bearer scan .
     ```
   - Update verification status in PR description:
     - `- [x]` if automated check passes
     - `- [ ]` with explanation if it fails
     - Note manual verification steps needed

6. **Execute upon confirmation:**
   - Push current branch to remote if needed: `git push -u origin [branch-name]`
   - Create PR using GitHub CLI with 1Password token:
     ```bash
     GH_TOKEN=$(op.exe read "op://Employee/GitHub CLI PAT/token") gh pr create --title "[Feature Title]" --body-file .agent_session/pr_description.md --base main
     ```
   - Note: Ensure 1Password is unlocked on desktop before running
   - Save PR description to `.agent_session/pr_description.md`
   - Update `.agent_session/context.md` with PR creation status
   - Show PR URL and remind user about manual verification steps if any

## Rails PR Description Template:

```markdown
## What this PR does
[Summary of the Rails feature implemented based on context.md]

## Database Changes
- [ ] New migration: `db/migrate/[timestamp]_[description].rb`
- [ ] Schema changes: [describe table/column modifications]
- [x] Migration rollback tested

## Rails Components Modified
- **Models**: `app/models/[model].rb` - [business logic added]
- **Controllers**: `app/controllers/[controllers].rb` - [REST actions]
- **Views**: `app/views/[views]/` - [ERB templates with Turbo]
- **Jobs**: `app/jobs/[job].rb` - [background processing]
- **Policies**: `app/policies/[policy].rb` - [authorization rules]

## Multi-tenancy & Security
- [x] All models include `acts_as_tenant(:account)`
- [x] Pundit policies implemented and tested
- [x] No tenant data leakage verified
- [x] Strong parameters configured

## Testing
- [x] Unit tests: `test/models/[model]_test.rb`
- [x] Controller tests: `test/controllers/[controller]_test.rb`
- [ ] System tests: Manual verification needed for UI flows

## Code Quality
- [x] Tests pass: `bin/rails test`
- [x] Rubocop passes: `bin/rubocop`
- [x] Security scan passes: `bearer scan .`

## Manual Verification Required
- [ ] Verify Turbo Frame updates work correctly
- [ ] Test multi-tenant data isolation
- [ ] Check responsive design on mobile
- [ ] Performance test with large datasets
```

## Important:
- **NEVER add co-author information or Claude attribution**
- PR should be authored solely by the user
- Use imperative mood in title ("Add feature" not "Added feature")
- Focus on Rails architecture, multi-tenancy, and user impact
- Base title and description on actual implemented changes, not plans