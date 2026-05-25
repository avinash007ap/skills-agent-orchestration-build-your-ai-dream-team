# Agent team

This repository uses a four-agent team to build Mona's Project Pulse dashboard:

- Orchestrator — Model: Claude Opus 4.7 (copilot). Responsibility: coordinate Planner, Coder, and Designer; break requests into phases, assign explicit file scopes, manage parallel vs. sequential work, and verify integrated results. Definition: .github/agents/orchestrator.agent.md

- Planner — Model: Claude Opus 4.7 (copilot). Responsibility: research the codebase, produce ordered implementation steps, identify dependencies/edge cases, and provide file-level task breakdowns. Definition: .github/agents/planner.agent.md

- Coder — Model: GPT-5.5 (copilot). Responsibility: implement the code, create runnable app support (e.g., .vscode/launch.json), validate changes, and report risks. Definition: .github/agents/coder.agent.md

- Designer — Model: Gemini 3.1 Pro (copilot). Responsibility: UI/UX, accessibility, information hierarchy, responsive layout, and visual polish (CSS hooks, project cards, badges). Definition: .github/agents/designer.agent.md

All work is orchestrated from a GitHub Copilot CLI running in a Codespace; the Orchestrator agent delegates tasks, enforces file ownership, and reports progress back to the user.
