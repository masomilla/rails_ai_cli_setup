# GEMINI.md

You are the primary coding agent for this repository. Follow the guidance below when analyzing, editing, or documenting code. Maintain alignment with Rails conventions and the existing project architecture.

## Development Preferences
- **Language**: Write all inline comments, commit messages in **English**. Write PR descriptions in **Italian**.
- **Style**: Preserve existing code style and conventions.

## Core Commands

### Setup & Run
- `bin/setup`: Complete initial setup (dependencies, db, assets).
- `bin/dev`: Start development services (Rails, CSS/JS bundling, Solid Queue) via Overmind/Foreman.
- `overmind connect web`: Connect to the running Rails process.
- `overmind restart web`: Restart the Rails server.
- `overmind kill`: Stop all dev processes.

### Testing & Quality
- `bin/rails test`: Run unit/integration tests.
- `bin/rails test:system`: Run system tests (use sparingly).
- `bearer scan .`: Run security static analysis.

### Assets
- `yarn build`: Build JavaScript assets.
- `yarn build:css`: Build CSS assets.

## GitHub CLI Authentication
The workstation uses a custom `gh` helper that fetches the PAT from 1Password. Since the agent environment might not source `.bashrc`, explicitly set the token when using `gh`:

```bash
GH_TOKEN=$(op.exe read "op://Employee/GitHub CLI PAT/token") gh <command>
```
*Ensure 1Password is unlocked locally before running these commands.*

## Tech Stack Overview
- **Framework**: Rails 7.2 monolith.
- **Locale**: Italian default (`config.i18n.default_locale = :it`). New views default to Italian copy.
- **Tenancy**: Multi-tenant SaaS using `acts_as_tenant` (Account scoped).
- **Foundation**: Jumpstart Pro (Auth, Billing, Admin).
- **Database**: PostgreSQL, Solid Cache, Active Storage (S3).
- **Frontend**: Hotwire (Turbo + Stimulus), Tailwind CSS, esbuild, ActionText/Trix.
- **Background Jobs**: Solid Queue (in `app/jobs/`).
- **Timezone**: Europe/Rome.

## Rails Principles
- **Guidelines**: Adhere to `context/rails_design_prinicples.md`.
- **Architecture**: Favor thin controllers, rich models, and idiomatic RESTful routing.

## Application Architecture
- **Key Models**: `Account`, `User`, `Contact`, `Event`, `Task`, `IntegrationAccount`.
- **Locales**: Italian (default), English, Spanish, French, Dutch.

## Test Strategy
**Prefer integration and controller tests over system tests.**
1.  **Unit tests** (models, jobs, concerns) - Highest priority.
2.  **Controller/Integration tests** - Test request/response without browser overhead.
3.  **System tests** - Only for critical end-to-end flows that cannot be tested otherwise.

## Playwright MCP Commands
Use these helpers for UI validation via Playwright:

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

## Documentation
- Project documentation lives under `docs/`.
- Update or extend documentation when introducing features or significant changes.
