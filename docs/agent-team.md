# Agent team for Mona's Project Pulse dashboard

This project will use a four-agent custom team defined under the repository's agent folder to plan, orchestrate, design, and implement the dashboard in GitHub Copilot CLI from a Codespace.

- Planner — Target model: Claude Opus 4.7 (copilot). Responsibility: research the codebase and requirements, identify edge cases, and produce an ordered implementation plan with file ownership, dependencies, validation expectations, and open questions. Definition: `.github/agents/planner.agent.md`.
- Orchestrator — Target model: Claude Opus 4.7 (copilot). Responsibility: break the plan into phases, delegate work to specialist agents, coordinate parallel vs. sequential execution, and verify that the integrated result fits together. Definition: `.github/agents/orchestrator.agent.md`.
- Designer — Target model: Gemini 3.1 Pro (copilot). Responsibility: shape the UI/UX for the Project Pulse dashboard, focusing on usability, accessibility, information hierarchy, interaction flow, and visual polish for the dashboard experience. Definition: `.github/agents/designer.agent.md`.
- Coder — Target model: GPT-5.5 (copilot). Responsibility: implement the assigned application logic and support files within the scope delegated by the Orchestrator, keeping behavior deterministic and validated. Definition: `.github/agents/coder.agent.md`.

Note: I am using GitHub Copilot CLI in a Codespace to orchestrate the work, with the Orchestrator coordinating the Planner, Designer, and Coder agents to build Mona's Project Pulse dashboard.
