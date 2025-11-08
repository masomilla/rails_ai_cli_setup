# AGENTS.md

## default
You are Codex, the primary coding agent for this repository. Follow the guidance below when analyzing, editing, or documenting code. Maintain alignment with Rails conventions and the existing project architecture.

### Development Preferences
- Write all inline comments, commit messages, and PR descriptions in English
- Preserve existing code style and conventions

### Core Commands
- Setup & install: `bin/setup`
- Start dev services: `bin/dev`
- Run unit/integration tests: `bin/rails test`
- Run system tests: `bin/rails test:system`
- Security scan: `bearer scan .`
- Build JavaScript assets: `yarn build`
- Build CSS assets: `yarn build:css`
- Connect to Rails process: `overmind connect web`
- Restart Rails server: `overmind restart web`
- Stop dev processes: `overmind kill`

#### GitHub CLI Authentication
The workstation defines a custom `gh` helper in `.bashrc` that fetches the PAT from 1Password. When using Codex tools that do not source `.bashrc`, set the token explicitly:

```bash
GH_TOKEN=$(op.exe read "op://Employee/GitHub CLI PAT/token") gh <command>
```
Ensure 1Password is unlocked locally before running these commands.

### Tech Stack Overview
- Rails 7.2 monolith with Italian default locale (`config.i18n.default_locale = :it`)
- Multi-tenant SaaS using `acts_as_tenant` (Account scoped)
- Jumpstart Pro foundation for auth, billing, admin
- PostgreSQL database, Solid Cache, Active Storage with S3 support
- Hotwire (Turbo + Stimulus), Tailwind CSS, esbuild bundling, ActionText/Trix
- New view doesn't has localization, the text is coded in Italian
- Rome timezone as default

### Rails Principles
- Adhere to the guidelines documented in `context/rails_design_prinicples.md`
- Favor thin controllers, rich models, and idiomatic RESTful routing

### Application Architecture Highlights
- **Key Models**: `Account`, `User`, `Contact`, `Event`, `Task`, `IntegrationAccount`
- **Background Jobs**: Solid Queue jobs in `app/jobs/` for email import, events, contacts, workflows
- **Frontend**: Turbo/Stimulus with Tailwind CSS and semantic ERB views
- **Testing**: Minitest with fixtures and WebMock; prefer integration/controller tests over system tests when feasible
- **Locales**: Italian default with English, Spanish, French, Dutch available; new views default to Italian copy
- **Timezone**: Europe/Rome
- **Security**: Use `bearer scan .` for static analysis; ensure GDPR compliance, CSP adherence

### Playwright MCP Commands
Use these helpers when invoking the Playwright MCP integration for UI validation:

```javascript
mcp__playwright__browser_navigate(url);
mcp__playwright__browser_take_screenshot();
mcp__playwright__browser_resize(width, height);
mcp__playwright__browser_click(element);
mcp__playwright__browser_type(element, text);
mcp__playwright__browser_hover(element);
mcp__playwright__browser_console_messages();
mcp__playwright__browser_snapshot();
mcp__playwright__browser_wait_for(text_or_element);
```

### Documentation Expectations
- Project documentation lives under `docs/`
- Update or extend documentation when introducing features or significant changes

### Test Strategy
**Prefer integration and controller tests over system tests.** System tests are slow and brittle. Use them sparingly only when absolutely necessary for critical user flows that cannot be tested otherwise.

Priority order:
1. **Unit tests** (models, jobs, concerns) - fastest, most focused
2. **Controller/Integration tests** - test request/response cycle without browser overhead
3. **System tests** - only for critical end-to-end flows (avoid when possible)

