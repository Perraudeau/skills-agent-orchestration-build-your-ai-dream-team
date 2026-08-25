# Agent team

The Mona's Project Pulse dashboard will be built by a custom agent team orchestrated through GitHub Copilot CLI in a Codespace:

| Agent | Target model | Responsibility | Definition |
| --- | --- | --- | --- |
| **Orchestrator** | Claude Opus 4.7 (copilot) | Coordinates the team, delegates work, manages file scope and dependencies, and verifies the integrated result. | `.github/agents/orchestrator.agent.md` |
| **Planner** | Claude Opus 4.7 (copilot) | Researches the repository and requirements, identifies risks and edge cases, and produces the implementation plan. | `.github/agents/planner.agent.md` |
| **Coder** | GPT-5.5 (copilot) | Implements dashboard logic and runnable-app support, following clear, explicit, testable coding practices and validating the changes. | `.github/agents/coder.agent.md` |
| **Designer** | Gemini 3.1 Pro (copilot) | Defines and implements the dashboard's UI/UX, accessibility, information hierarchy, responsive behavior, and visual styling. | `.github/agents/designer.agent.md` |
