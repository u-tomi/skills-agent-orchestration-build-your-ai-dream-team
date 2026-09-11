# Agent team

I will use GitHub Copilot CLI in a Codespace to orchestrate a four-agent team for building Mona's Project Pulse dashboard:

| Agent | Target model | Responsibility | Definition |
| --- | --- | --- | --- |
| **Orchestrator** | Claude Opus 4.7 (copilot) | Coordinates the team, breaks the work into phases, assigns explicit file scopes, manages dependencies, and verifies that the integrated result works together. | [`.github/agents/orchestrator.agent.md`](../.github/agents/orchestrator.agent.md) |
| **Planner** | Claude Opus 4.7 (copilot) | Researches the repository and relevant documentation, identifies requirements, risks, dependencies, edge cases, and validation needs, then produces an implementation plan. | [`.github/agents/planner.agent.md`](../.github/agents/planner.agent.md) |
| **Coder** | GPT-5.5 (copilot) | Implements the dashboard logic and runnable application support with clear, deterministic, testable code, explicit errors, and validation. | [`.github/agents/coder.agent.md`](../.github/agents/coder.agent.md) |
| **Designer** | Gemini 3.1 Pro (copilot) | Defines and implements the Project Pulse user experience, including information hierarchy, accessibility, responsive behavior, visual clarity, project cards, status badges, and priority treatment. | [`.github/agents/designer.agent.md`](../.github/agents/designer.agent.md) |

The Orchestrator will have the Planner establish the approach first, then delegate implementation and design work to the Coder and Designer in parallel when their file scopes allow it, and integrate and validate the completed dashboard before reporting the result.
