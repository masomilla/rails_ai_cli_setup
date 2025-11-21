---
description: Perform a focused UX/UI review of recent changes using Playwright for live checks (Codex)
---

# UX Review (Codex)

Evaluate UI changes in the Rails app with emphasis on real user experience.

## Rules
- Test against the running app when possible; favor real interaction over static diff reading.
- Follow existing Hotwire/Stimulus patterns and Italian UI language.
- Use Playwright MCP for navigation, interaction, and screenshots.
- Report findings with severity labels (Critical/Major/Minor/Nice-to-have) focusing on impact.

## Process
1. **Context**: read PR/issue details to know what changed. Scan diffs in views/controllers/js/css to target scenarios.
2. **Environment**: ensure server is running (`bin/dev`). Set viewport with `mcp__playwright__browser_resize({ width: 1440, height: 900 });` and navigate to affected pages.
3. **Flows to test**:
   - Primary user journeys touched by the change (create/edit/delete, filter, pagination).
   - Turbo/Turbo Stream behavior (no full reloads, proper partial updates).
   - Stimulus controllers: hovers, clicks, validation feedback, loading/disabled states.
   - Localization: Italian copy present and consistent.
4. **Responsiveness/accessibility**: resize for mobile (e.g., 390x844), check layout breaks, focus order, keyboard navigation on forms, color contrast, and aria/label presence for inputs where feasible.
5. **Document findings**: capture screenshots when helpful. Report per-item: severity, page/element, expected vs actual, and suggested fix if obvious. Group regressions first.
6. **Share summary**: provide a concise list of issues and residual risks or untested areas.
