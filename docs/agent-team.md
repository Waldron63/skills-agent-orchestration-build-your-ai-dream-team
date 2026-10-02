# Agent team

I will use GitHub Copilot CLI in a Codespace to orchestrate this custom agent team for Mona's Project Pulse dashboard:

| Agent | Target model | Responsibility | Definition |
| --- | --- | --- | --- |
| **Orchestrator** | Claude Opus 4.7 (copilot) | Coordinates the team, breaks work into phases, delegates tasks with explicit file scopes, manages dependencies, and verifies the integrated result. | `.github/agents/orchestrator.agent.md` |
| **Planner** | Claude Opus 4.7 (copilot) | Researches the repository and relevant documentation, identifies dependencies and edge cases, and creates an ordered implementation plan with validation expectations. | `.github/agents/planner.agent.md` |
| **Coder** | GPT-5.5 (copilot) | Implements dashboard code and fixes, keeps behavior explicit and testable, creates assigned runnable-app support files, and validates changes. | `.github/agents/coder.agent.md` |
| **Designer** | Gemini 3.1 Pro (copilot) | Defines and implements the Project Pulse user experience, including information hierarchy, accessibility, responsive behavior, visual clarity, project cards, status badges, and priority treatment. | `.github/agents/designer.agent.md` |

The Orchestrator will first obtain a plan, then sequence or parallelize Coder and Designer work according to file ownership and dependencies, and finally verify that the completed dashboard works as a cohesive result. All four agents follow the repository's Git-control rule: they do not stage, commit, or push changes; those operations remain under my control through Copilot CLI prompts.
