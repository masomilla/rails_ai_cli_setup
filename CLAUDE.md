# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Development preference
- All the comments in the code MUST be written in English
- Git messages MUST be written in English
- Github PR MUST be written in Italian

## Development Commands

### Setup and Installation
- `bin/setup` - Complete initial setup including dependencies, database, and asset compilation
- `bin/dev` - Start development server using Overmind/Foreman (Rails server, CSS/JS bundling, Solid Queue)

### Testing and Quality
- `bin/rails test` - Run Rails unit/integration tests
- `bin/rails test:system` - Run system tests with Capybara/Selenium
- `bearer scan .` - Run static code analysis for security vulnerabilities

### Asset Building
- `yarn build` - Build JavaScript assets with esbuild
- `yarn build:css` - Build CSS assets with Tailwind CSS

### Development Tools
- `overmind connect web` - Connect to Rails process for debugging (disconnect with Ctrl+B then D)
- `overmind restart web` - Restart Rails server without affecting other processes
- `overmind kill` - Stop all development processes

### GitHub CLI Authentication
When using `gh` commands for pull requests and GitHub operations, the token is stored in 1Password. To use gh commands properly:

```bash
# The user has a custom gh function in .bashrc that retrieves the token from 1Password
# Since the Bash tool doesn't load .bashrc, you need to manually set the token:

# Get the token from 1Password and set it as environment variable
GH_TOKEN=$(op.exe read "op://Employee/GitHub CLI PAT/token") gh <command>

# Example: Create a pull request
GH_TOKEN=$(op.exe read "op://Employee/GitHub CLI PAT/token") gh pr create --title "Title" --body "Body"

# Example: Check PR status  
GH_TOKEN=$(op.exe read "op://Employee/GitHub CLI PAT/token") gh pr status
```

**Note**: Make sure 1Password is unlocked on the desktop before running these commands.

## Tech Stack

Framework: Ruby on Rails (7.2)
Database: PostgreSQL
Frontend: Hotwire (Stimulus + Turbo)
Styling: Tailwind CSS
Architecture: Monolithic Rails

## Rails principles

Follow the principles described in context/rails_design_prinicples.md

## Application Architecture

### Core Framework
- **Rails 7.2** application with Italian locale as default (`config.i18n.default_locale = :it`)
- **Multi-tenant SaaS** architecture using `acts_as_tenant` gem with Account-based tenancy
- **Jumpstart Pro** framework providing authentication, billing, and admin features

### Key Domain Models
- **Account** - Tenant entity with owner, users, and all business data
- **User** - Authentication entity belonging to multiple accounts
- **Contact** - Core business entity with products, interactions, labels, and custom fields
- **Event** - Calendar events with recurring patterns, attendees, and integrations
- **Task** - Task management with templates, workflows, and assignments
- **Integration Account** - External service connections (Google, Microsoft Graph, CalDAV)

### Background Jobs
- **Solid Queue** for job processing (replaces Sidekiq)
- Jobs for email imports, event synchronization, contact processing, and workflow automation
- Located in `app/jobs/` with comprehensive test coverage

### Frontend Stack
- **Hotwire** (Turbo + Stimulus) for modern Rails frontend
- **Tailwind CSS** for styling with PostCSS pipeline
- **esbuild** for JavaScript bundling
- **ActionText** with Trix editor for rich text content

### Database and Storage
- **PostgreSQL** as primary database
- **Active Storage** for file uploads with S3 support
- **Solid Cache** for caching layer

### Testing Strategy
- Add tests to cover all changes and new features
- **Minitest** for unit and integration tests
- **Webmock** for HTTP request stubbing
- Fixtures in `test/fixtures/` for test data
- If possible don't use system tests, prefer integration and controller
- **Capybara + Selenium** for system tests
- System tests require Chrome browser

### Multi-language Support
- Available locales: Italian (default)
- Translation files in `config/locales/` with nested organization
- New view doesn't has localization, the text is coded in Italian
- Rome timezone as default

### Security and Compliance
- **Bearer** for static security analysis
- GDPR compliance features with data export/deletion
- Content Security Policy configured

### Playwright MCP Integration

#### Essential Commands for UI Testing

```javascript
// Navigation & Screenshots
mcp__playwright__browser_navigate(url); // Navigate to page
mcp__playwright__browser_take_screenshot(); // Capture visual evidence
mcp__playwright__browser_resize(
  width,
  height
); // Test responsiveness

// Interaction Testing
mcp__playwright__browser_click(element); // Test clicks
mcp__playwright__browser_type(
  element,
  text
); // Test input
mcp__playwright__browser_hover(element); // Test hover states

// Validation
mcp__playwright__browser_console_messages(); // Check for errors
mcp__playwright__browser_snapshot(); // Accessibility check
mcp__playwright__browser_wait_for(
  text / element
); // Ensure loading
```

## Documentation
- documentation is under /docs
- Add or update documentation for any changes and new feature
