# Unity AI Workflow

A comprehensive, agent-first toolkit for AI-assisted Unity development. Designed primarily for **Google Antigravity IDE**, this repository provides the "brain" (rules, agents, skills, and workflows) to turn your AI assistant into a group of expert Unity team members.

## 🚀 Quick Start

1. **Copy the Toolkit**: Copy the `.agent/` folder from this repository into the root of your Unity project.
2. **Configure your Project**: Copy `templates/ProjectConfig_Template.yaml` to your project's `docs/ProjectConfig.yaml` and fill out your project details.
3. **Connect MCPs**: Enable the recommended MCP servers in Antigravity (Agent panel > ... > MCP Servers):
   - **Unity MCP**: For in-editor scene/prefab/asset management.
   - **GitHub MCP**: For automated git operations.
   - **Linear/Notion MCP**: For task and doc syncing.

## 🧠 How it Works

This toolkit uses Antigravity's native capabilities to load specialized context based on your task:

- **Global Constitution**: `RULES.md` enforces Unity 6.2+ best practices, performance standards, and thread safety.
- **Auto-Routing**: `AGENTS.md` automatically loads the right agent personas (Gameplay Dev, UI Specialist, etc.) and skills based on what you're working on.
- **Expert Skills**: Dedicated skills for UI Toolkit data binding, Netcode for GameObjects, ScriptableObject logic, and more.
- **Guided Workflows**: Slash commands like `/brainstorm`, `/plan`, and `/polish` to guide you through every project phase.

## 📅 Project Phases

Follow the guided guides in `docs/phases/` for a standardized development journey:

1. **[00: Ideation](docs/phases/00_Ideation.md)** - From "I have an idea" to a defined GDD.
2. **[01: Pre-Production](docs/phases/01_PreProduction.md)** - Technical choices, stack definition, and naming conventions.
3. **[02: Technical Design](docs/phases/02_TechnicalDesign.md)** - Architecture, Assembly Definitions, and patterns.
4. **[03: Project Setup](docs/phases/03_ProjectSetup.md)** - Automated folder scaffolding and package installation.
5. **[04: Production](docs/phases/04_Production.md)** - Iterative sprints (Plan -> Create -> Test -> Debug).
6. **[05: Polish](docs/phases/05_Polish.md)** - The "Juice Pass" using the Game Feel Document (GFD).

## 🛠️ Configuration

The AI assistant reads `docs/ProjectConfig.yaml` to stay in sync with your versions, packages, and architectural choices. Always keep this file updated!

---
*Created by David-GD13. Targeted for Unity 6.2+ and Modern AI workflows.*