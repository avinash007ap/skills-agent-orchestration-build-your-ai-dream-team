# Project Pulse — Implementation Plan

## Summary
Project Pulse is a small static dashboard that renders project cards and status badges powered by app/project-data.json. This plan breaks work into phased tasks, assigns file ownership to the four-agent team (Orchestrator/Planner/Designer/Coder), and gives concrete validation steps (including using Codespaces and .vscode/launch.json).

## Scope and goals
- Deliver a responsive single-page static dashboard (app/index.html + app/styles.css) that reads app/project-data.json and renders one card per project with a colored status badge.
- Provide a .vscode/launch.json so contributors can Run/Debug in Codespaces.
- Keep implementation accessible, resilient to bad/missing data, and easy to extend.

## File assignments (explicit)
- app/index.html — Coder (implementation) / Designer (visual assets & markup guidance)
- app/styles.css — Designer (visual design) / Coder (responsive fixes)
- app/project-data.json — Planner (seed content) / Coder (parsing validations)
- .vscode/launch.json — Orchestrator (coordination + canonical config) / Coder (ensures it works locally)

Other meta owners:
- Planner: produces this plan only.
- Orchestrator: coordinates phases and verifies merges.

## Designer responsibilities
- app/index.html: provide final visual mock snippets (HTML structure guidance, accessible ARIA roles).
- app/styles.css: deliver color palette, badge styles, typography, responsive breakpoints, and hover/focus states.
- Provide image assets or SVG icons (if any) and specify exact CSS classes to style badges and cards.

## Coder responsibilities
- app/index.html: implement static page, load and parse app/project-data.json via fetch, render DOM cards, attach basic event handlers.
- app/styles.css: implement responsive layout per designer spec; ensure accessible contrasts.
- app/project-data.json: validate JSON parsing errors and provide fallback UI for empty or invalid data.
- .vscode/launch.json: create a launch configuration named "Launch Project Pulse" that starts a static server (or launches index.html) for Codespaces debugging.

## Ordered implementation steps (phases) with ownership
1. Phase 1 — Seed & Config (Owner: Planner + Orchestrator)
   - Files: app/project-data.json (Planner), .vscode/launch.json (Orchestrator)
2. Phase 2 — Visual Design (Owner: Designer)
   - Files: app/index.html (design snippets), app/styles.css (design spec)
3. Phase 3 — Static Markup + CSS (Owner: Coder + Designer)
   - Files: app/index.html (Coder implements markup per design), app/styles.css (Coder implements styles)
4. Phase 4 — Data Integration & Error Handling (Owner: Coder)
   - Files: app/index.html (JS to fetch & render), app/project-data.json (validate)
5. Phase 5 — Debug Config Validation & Docs (Owner: Orchestrator + Coder)
   - Files: .vscode/launch.json (finalize), docs/project-pulse-plan.md (Planner)

## Dependencies between steps
- Phase 1 blocks Phase 4 (needs data & launch config).
- Phase 2 must complete before Phase 3 starts (design spec drives implementation).
- Phase 3 must complete before Phase 4 (structure and styles exist for dynamic injection).

## Work that can run in parallel (and why)
- Phase 1 (data + launch config) and Phase 2 (visual design) can run in parallel — they target separate files and concerns.
- During Phase 3, Coder can implement HTML skeleton while Designer finalizes CSS riffing; final integration merges later.
- Phase 5 (docs) can begin as soon as Phase 1 and 2 are underway.

## Work that must run sequentially
- Data integration (Phase 4) must come after HTML/CSS skeleton (Phase 3).
- Final launch/debug validation (Phase 5) after .vscode/launch.json is present.

## Edge cases to handle
- Empty or malformed app/project-data.json — show a friendly “No projects” or error banner.
- Missing fields on a project (no status, no name) — use placeholders.
- Very long titles — truncate with tooltip.
- Large number of projects — responsive grid that paginates or scrolls smoothly.
- Color blindness — choose accessible palette & add icons for status.

## Validation expectations and acceptance criteria
Local test in Codespace:
1. Open Codespace workspace.
2. Start debug via VS Code Run → Start Debugging (F5) using .vscode/launch.json "Launch Project Pulse". The config should start a simple static server and open the app URL in the Codespace preview or forwarded port.
   - Alternative: run `python -m http.server --directory app 8000` and forward port in Codespace.
3. Use GitHub Copilot CLI if available to assist (optional). If using Copilot CLI, ensure server process is launched in the Codespace terminal and port is forwarded.

UI checks:
- Number of project cards equals entries in app/project-data.json.
- Each card shows title, short description, and a colored status badge whose color matches the status mapping from Designer.
- Responsive behavior: grid collapses from 3 → 2 → 1 columns at published breakpoints.
- Accessibility: badges have aria-labels, keyboard focus states visible.

Data checks:
- Modify app/project-data.json to include an invalid JSON — app displays error banner (not crash).
- Remove a field (e.g., status) — app displays "Unknown" and default badge style.
- Add 50 entries — page remains usable (no layout break).

## Open questions
- Preferred status-to-color mapping (e.g., green/yellow/red/blue)?
- Do we need pagination or infinite scroll for >50 projects?
- Should images per project be supported (and where stored)?

Planner: produce plan only. Orchestrator: schedule phases. Designer/Coder: implement per ownership above.