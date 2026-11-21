# Agent team

I am using GitHub Copilot CLI in this Codespace to orchestrate a custom agent team for Mona's Project Pulse dashboard.

## Orchestrator

- **Model:** Claude Opus 5.5
- **Responsibility:** Coordinates the specialist agents, turns the implementation plan into phases, assigns non-overlapping file scopes, runs independent work in parallel, sequences dependent work, verifies integration, and reports the final result.
- **Definition:** `.github/agents/orchestrator.agent.md`

## Planner

- **Model:** Claude Opus 5.5
- **Responsibility:** Researches the repository and requirements, identifies dependencies and edge cases, and creates a practical implementation plan without writing code.
- **Definition:** `.github/agents/planner.agent.md`

## Designer

- **Model:** Gemini 3.7 Flash
- **Responsibility:** Designs the dashboard's usability, accessibility, information hierarchy, interactions, responsive behavior, and polished visual presentation.
- **Definition:** `.github/agents/designer.agent.md`

## Coder

- **Model:** GPT-5.5
- **Responsibility:** Implements the dashboard code and supporting configuration with clear structure, deterministic behavior, and testable output.
- **Definition:** `.github/agents/coder.agent.md`

## How the team builds Project Pulse

The Orchestrator starts by asking the Planner to research Project Pulse and produce an implementation plan with file assignments and dependencies. The Orchestrator then coordinates the Designer and Coder in appropriate phases, keeping their file scopes separate when work can run in parallel and sequencing overlapping work when it is dependent.

The Designer creates the dashboard's UI and styling expectations, while the Coder implements the application files and launch configuration. The Orchestrator verifies that the integrated dashboard works as Project Pulse and provides a clear handoff. These agent definitions live in `.github/agents/`, and no agent stages, commits, or pushes changes; repository git operations remain under the learner's control.
