# Mona's Project Pulse agent team

I am using GitHub Copilot CLI in a Codespace to orchestrate a four-agent team for Mona's Project Pulse dashboard. The Orchestrator turns the request into scoped phases, gets a plan first, coordinates safe handoffs between specialists, and verifies that the finished dashboard works as an integrated experience.

| Agent | Target model | Responsibility | Definition |
|---|---|---|---|
| Orchestrator | Claude Opus 4.7 (copilot) | Coordinates the team: requests the technical plan, assigns non-overlapping file scopes, sequences dependent work, and verifies the integrated result without implementing it directly. | `.github/agents/orchestrator.agent.md` |
| Planner | Claude Opus 4.7 (copilot) | Researches the repository and relevant documentation, identifies dependencies and edge cases, and produces an ordered implementation plan with file ownership and validation expectations. | `.github/agents/planner.agent.md` |
| Coder | GPT-5.5 (copilot) | Implements dashboard behavior and application logic with explicit errors and testable, deterministic code. When assigned runnable-app work, it can also add the Project Pulse launch configuration. | `.github/agents/coder.agent.md` |
| Designer | Gemini 3.1 Pro (copilot) | Designs the dashboard's UI/UX, accessibility, responsive layout, information hierarchy, and visual polish, including project cards, status badges, priority treatment, and clear styling hooks. | `.github/agents/designer.agent.md` |

The expected workflow is Planner → Orchestrator-managed implementation and design phases → integrated verification. Each specialist stays inside its assigned file scope, and none of the agents stages, commits, or pushes changes; the learner retains Git control through Copilot CLI prompts.
