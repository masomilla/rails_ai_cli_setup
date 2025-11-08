# Rails AI CLI Setup

> ⚠️ **Work in Progress**: This configuration is actively being developed and refined. Features, commands, and documentation may change as we improve the setup based on real-world usage and community feedback.

A curated configuration setup for Claude Code (Anthropic's AI CLI tool) optimized for Ruby on Rails application development following DHH's orthodox Rails principles.

## Overview

This repository contains a battle-tested configuration structure for using Claude Code with Rails applications. It's designed for developers who follow the "Rails Way" and want their AI assistant to understand and respect Rails conventions, particularly:

- Monolithic architecture ("Majestic Monolith")
- Server-rendered web applications
- Fat models over service objects
- Hotwire for frontend interactivity
- Convention over configuration

## What's Included

- **Custom Instructions**: Pre-configured prompts that teach Claude Code about Rails conventions and DHH's development philosophy
- **Context Files**: Structured documentation for your Rails stack (models, controllers, views, Hotwire patterns)
- **Subagent Configurations**: Specialized AI agents for different aspects of Rails development:
  - Domain logic and business rules
  - Backend/API development
  - Hotwire frontend patterns
- **Best Practices**: Guidelines for maintaining consistency in AI-assisted Rails development

## Who Is This For?

- Rails developers who want AI assistance that "gets" the Rails Way
- Teams looking to standardize their AI tooling for Rails projects
- Developers using Hotwire and wanting better AI support for Turbo/Stimulus patterns
- Anyone tired of fighting with AI tools that suggest un-Rails-like solutions

## Philosophy

This setup is opinionated by design. It guides Claude Code to:
- Suggest solutions that align with Rails conventions
- Avoid over-engineering (no unnecessary service objects, no microservices)
- Respect the monolithic architecture
- Understand Hotwire patterns and server-side rendering
- Follow DHH's pragmatic approach to web development

## Development Workflow

This setup is designed to augment, not replace, the developer. The developer evolves into an **analyst, project manager, and code reviewer**, while Claude Code acts as a **junior developer** executing well-defined tasks.

### The Process

1. **Requirements Definition** (Developer as Analyst)
   - Define clear functional specifications in a GitHub issue
   - Include acceptance criteria, edge cases, and constraints
   - Provide business context and user stories

2. **Technical Planning** (Developer as Project Manager)
   - Run `/create-plan #[issue-number]` to generate technical analysis
   - Claude researches the codebase using specialized agents
   - Iteratively refine the plan in `.agent_session/plan.md` through conversation
   - Continue iterations until the plan meets expectations and all questions are resolved
   - **Iterative Refinement**: Provide feedback and request changes until the technical plan fully meets your expectations

3. **Implementation** (Claude as Junior Developer)
   - Run `/implement-plan` to execute the approved plan
   - Claude implements following Rails conventions and multi-tenancy patterns
   - Progress tracked through todo lists and phase completion
   - **Continuous Rework**: Review implementation progress, provide feedback, and request corrections as needed until code quality meets standards

4. **Quality Assurance** (Developer as Reviewer)
   - Run `/validate-plan` to verify implementation correctness
   - Run `/ux-review` for UI/UX validation with automated testing
   - Review code changes and provide feedback
   - Ensure all success criteria are met
   - **Quality Iterations**: Request fixes and improvements based on validation results until all acceptance criteria are satisfied

5. **Delivery** (Collaborative)
   - Run `/commit` to create structured git commits
   - Run `/pr` to generate pull request with comprehensive description
   - Final human review before merge

### Key Principle

The developer maintains control and decision-making authority at every stage. Claude Code provides execution speed and consistency, but the developer ensures quality, architectural alignment, and business value.

### Context Management Strategy

To prevent AI hallucinations and maintain accuracy, this setup uses a **minimal context approach** with persistent file-based storage:

**Session Files Structure** (`.agent_session/` directory):
- `context.md` - Shared session context written by all commands
- `plan.md` - Technical implementation plan
- `model.md`, `frontend.md`, `hotwire.md`, `job.md`, `test.md` - Agent-specific analysis results

**Workflow Between Commands**:
1. Execute command (e.g., `/create-plan`)
2. Claude saves results to appropriate `.agent_session/*.md` files
3. Developer reviews the output files
4. **Clear the chat** with `/clear` to reset context
5. Execute next command (e.g., `/implement-plan`)
6. Claude reads necessary context from `.agent_session/*.md` files
7. Repeat cycle

**Benefits**:
- **Reduced hallucinations**: Compact context prevents AI confusion
- **Persistent memory**: Context survives across chat sessions
- **Token efficiency**: ~70% reduction in token consumption
- **Traceability**: All decisions and analysis documented in versioned files
- **Recovery**: Can resume work from any point by reading session files

The `.agent_session/` directory becomes the "memory" of your development session, allowing you to maintain clean, focused conversations while preserving all critical information.

## Available Slash Commands

This setup includes custom slash commands that streamline common Rails development workflows:

### Planning & Implementation
- **`/create-plan`** - Creates detailed implementation plans for GitHub issues through interactive analysis. Uses specialized agents to research the codebase and design solutions. Maintains context in `.agent_session/context.md` to reduce token consumption by 70%.

- **`/implement-plan`** - Implements technical plans following the phases defined in `.agent_session/plan.md`. Ensures Rails conventions, multi-tenancy patterns with `acts_as_tenant`, and comprehensive test coverage throughout the implementation process.

- **`/fix-bug`** - Streamlined workflow for bug fixes that reads a GitHub issue, creates an implementation plan, and immediately executes the fix. Combines planning and implementation in a single command for faster bug resolution.

### Quality Assurance
- **`/validate-plan`** - Validates that an implementation plan was correctly executed by verifying all success criteria, checking multi-tenancy patterns, and ensuring Rails conventions are followed. Uses context from `.agent_session/context.md`.

- **`/ux-review`** - Conducts comprehensive design review on UI changes using Playwright for automated testing. Reviews visual consistency, accessibility compliance (WCAG 2.1 AA), responsive design, and Rails-specific patterns like Turbo Frames and Stimulus controllers.

### Git & Documentation
- **`/commit`** - Creates well-structured git commits for changes made during the session with clear, descriptive messages that focus on the "why" rather than just the "what".

- **`/pr`** - Creates pull requests for Rails features based on completed implementation. Analyzes changes from git and context files to generate appropriate title and description following Rails conventions.

- **`/doc`** - Updates documentation in `/docs` based on changes in the current branch. Analyzes git modifications, understands existing documentation structure, and updates appropriate docs for Rails features, patterns, and integrations.

## Specialized Agents

This setup includes specialized AI agents that provide focused expertise for different aspects of Rails development:

### Codebase Analysis
- **`codebase-locator`** - Locates files, directories, and components relevant to a feature or task. Acts as a "Super Grep/Glob/LS tool" for finding where code lives in the Rails application following naming conventions and multi-tenant patterns.

- **`codebase-analyzer`** - Analyzes implementation details with precise file:line references. Traces data flow, identifies architectural patterns, and explains HOW code works, including multi-tenancy patterns, Hotwire integration, and Rails conventions.

### Rails Domain Specialists

- **`model-specialist`** - Rails model and database expert following DHH's fat model philosophy. Masters ActiveRecord, schema design, migrations, concerns, POROs, caching strategies, N+1 elimination, and performance optimization for monolithic Rails applications.

- **`frontend-specialist`** - Rails controller and view expert following DHH's philosophy. Masters RESTful controllers, ERB views, TailwindCSS, thin controllers, semantic HTML, Turbo/Stimulus integration, responsive design, and accessibility compliance.

- **`hotwire-specialist`** - Hotwire expert for dynamic real-time Rails applications. Masters Turbo Frames, Turbo Streams, and Stimulus controllers. Creates seamless user experiences with progressive enhancement and server-rendered architecture. Prioritizes Turbo Morphing over Streams for simpler implementations.

- **`job-specialist`** - Rails ActiveJob/Solid Queue expert following DHH's Rails Way. Designs idempotent background jobs with queue optimization, batch processing, performance monitoring, and tenant isolation for multi-tenant SaaS applications.

- **`test-specialist`** - Comprehensive testing specialist for Rails applications. Expert in Minitest, Fixtures, and testing strategies. Focuses on testing fat models, controllers, and Hotwire interactions. Prefers integration and controller tests over system tests for better performance.

### Research & Information
- **`web-researcher`** - Web research specialist for finding modern information, documentation, and solutions. Uses WebSearch and WebFetch to discover best practices, API documentation, and technical solutions from authoritative sources.

### Agent Usage

Agents are automatically invoked by slash commands when specialized expertise is needed. For example:
- `/create-plan` uses `codebase-locator` and `codebase-analyzer` to research the codebase
- `/implement-plan` uses domain specialists (`model-specialist`, `frontend-specialist`, etc.) for implementation guidance
- `/validate-plan` uses multiple specialists to verify implementation correctness

Each agent maintains context through `.agent_session/context.md` to reduce token consumption by ~70% and provides detailed analysis in dedicated session files.

## Getting Started

### Prerequisites

Before using this setup, you need to install Claude Code CLI, Playwright MCP server, and configure your Rails project.

### 1. Install Claude Code CLI

Follow the official installation guide at [docs.claude.com](https://docs.claude.com/en/docs/claude-code):

```bash
# macOS/Linux
curl -fsSL https://cli.anthropic.com/install.sh | sh

# Verify installation
claude --version
```

After installation, authenticate with your Anthropic API key:

```bash
claude auth
```

### 2. Install GitHub CLI

The GitHub CLI is required for creating pull requests and working with GitHub issues.

**Install GitHub CLI**:

```bash
# macOS
brew install gh

# Ubuntu/Debian
sudo apt install gh

# Windows (via winget)
winget install --id GitHub.cli

# Or download from https://cli.github.com/
```

**Authenticate with GitHub**:

```bash
gh auth login
```

**Special Configuration for Windows/WSL with 1Password**:

If you're using Windows/WSL with 1Password for credential management, see the special authentication configuration in `CLAUDE.md` under "GitHub CLI Authentication". This setup uses a custom approach to retrieve tokens from 1Password:

```bash
# Example from CLAUDE.md
GH_TOKEN=$(op.exe read "op://Employee/GitHub CLI PAT/token") gh pr create --title "Title" --body "Body"
```

This pattern is automatically used by `/create-plan`, `/pr`, and other commands that interact with GitHub.

### 3. Install Playwright MCP Server

The Playwright MCP server enables UI testing and validation capabilities used by `/ux-review`.

**Install Playwright MCP Server**:

```bash
# Using npm
npm install -g @playwright/mcp-server

# Or using your package manager
npx @playwright/mcp-server install
```

**Configure MCP Server** in `.claude/settings.local.json`:

```json
{
  "mcpServers": {
    "playwright": {
      "command": "npx",
      "args": ["-y", "@playwright/mcp-server"]
    }
  }
}
```

**Verify MCP Server**:

Start a Claude Code session and check that Playwright tools are available:
- `mcp__playwright__browser_navigate`
- `mcp__playwright__browser_take_screenshot`
- `mcp__playwright__browser_click`

### 4. Setup Your Rails Project

**Clone or copy this configuration** to your Rails project:

```bash
# Copy .claude directory to your Rails project
cp -r .claude /path/to/your/rails/project/

# Create agent session directory
mkdir -p .agent_session
```

**Add to `.gitignore`**:

```bash
# Add to your project's .gitignore
echo ".agent_session/" >> .gitignore
```

**Configure CLAUDE.md** (optional):

Review and customize `.claude/CLAUDE.md` with your project-specific:
- Tech stack details
- Custom development commands
- Domain models
- Multi-tenancy patterns
- Testing strategy

**Language Configuration**:

⚠️ **Note**: This setup is configured to generate artifacts (plans, analysis, documentation) in **Italian** by default, as the original author is Italian. The slash commands and agents use Italian for their output.

If you prefer **English** or another language:
1. Edit each command file in `.claude/commands/*.md`
2. Update the response templates and prompts to your preferred language
3. Edit agent files in `.claude/agents/*.md` to change output language
4. Modify `CLAUDE.md` development preferences section

Example changes needed:
- In `/create-plan`: Change "Ti aiuterò a creare..." to "I'll help you create..."
- In agents: Change "Analisi", "Raccomandazioni" headers to "Analysis", "Recommendations"
- In `CLAUDE.md`: Update any Italian instructions to your language

### 5. Verify Installation

Start Claude Code in your Rails project directory:

```bash
cd /path/to/your/rails/project
claude
```

Test that slash commands are available:

```bash
# In Claude Code session
/help

# You should see custom commands:
# /create-plan, /implement-plan, /fix-bug, etc.
```

### 6. First Use

1. Create a GitHub issue with clear functional specifications
2. Start your first planning session:
   ```bash
   /create-plan #[issue-number]
   ```
3. Follow the interactive workflow to generate your implementation plan

You're now ready to use Claude Code as your AI-powered junior developer!

## Contributing

Contributions are welcome, especially if they strengthen Rails orthodoxy and convention-following in AI-assisted development.

## License

MIT License

Copyright (c) 2025 Masomilla srl

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all
copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE
SOFTWARE.

---

**Fair Use Notice**: This configuration is provided freely for the Rails community.
While you're welcome to use it in your commercial projects, please don't package
and sell this configuration as a standalone commercial product. That's not cool.

---
