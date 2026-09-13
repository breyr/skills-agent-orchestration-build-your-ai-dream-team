# Project Pulse agent team

Mona's Project Pulse dashboard will be built by a coordinated team of custom GitHub Copilot CLI agents defined in `.github/agents/`.

## Team overview

| Agent | Model | Responsibility | Source file |
| --- | --- | --- | --- |
| Orchestrator | Claude Opus 4.7 (copilot) | Coordinates the workflow, breaks the request into phases, delegates work to the right specialist, and validates that the pieces fit together. | `.github/agents/orchestrator.agent.md` |
| Planner | Claude Opus 4.7 (copilot) | Researches the repo, identifies dependencies, defines file ownership, and creates a practical implementation plan before coding starts. | `.github/agents/planner.agent.md` |
| Coder | GPT-5.5 (copilot) | Implements the dashboard logic and files, with a focus on testable, maintainable static app code and validation. | `.github/agents/coder.agent.md` |
| Designer | Gemini 3.1 Pro (copilot) | Shapes the UX, information hierarchy, accessibility, and visual polish for the dashboard so it feels like a clean Project Pulse frontend. | `.github/agents/designer.agent.md` |

## Agent responsibilities

### Orchestrator

The Orchestrator is the project lead. It receives the request, asks the Planner for a plan, divides the work into phases, delegates design and build tasks to specialists, and confirms the final result hangs together. It does not implement the dashboard itself.

### Planner

The Planner creates the roadmap. It researches the repo, studies requirements, identifies dependencies, and defines the step-by-step plan for the dashboard, including what files belong to which specialist and what work can happen in parallel.

### Designer

The Designer focuses on the experience layer. It proposes the layout, hierarchy, styling direction, responsiveness, and accessibility decisions needed for a polished Project Pulse dashboard with project cards, status badges, and readable spacing.

### Coder

The Coder turns the plan into runnable app files. It builds `app/index.html`, `app/styles.css`, and `app/project-data.json`, and helps ensure the static dashboard loads correctly from the app directory and opens `index.html` in the preview flow.

## How the team will work together to build Project Pulse

1. The Orchestrator receives the Project Pulse request and identifies the work to be delegated.
2. The Planner researches the brief and creates an implementation plan with phases, file ownership, dependencies, and validation expectations.
3. The Designer guides the dashboard experience, defining how the UI should present project status, owner, activity, and priorities in a clear and polished way.
4. The Coder implements the actual static dashboard files and any associated launch configuration needed to preview the app from VS Code.
5. The Orchestrator reviews the output, checks that design and implementation align, and confirms the final Project Pulse dashboard is ready for handoff.

This team keeps responsibilities separate: strategy comes from the Planner, UX from the Designer, implementation from the Coder, and coordination from the Orchestrator. That separation makes it easier to build the Project Pulse dashboard in a structured, repeatable way using GitHub Copilot CLI.
