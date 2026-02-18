# Unity AI Workflow

A comprehensive, agent-first toolkit for AI-assisted Unity development. Designed primarily for **Google Antigravity IDE**, this repository provides the "brain" (rules, agents, skills, and workflows) to turn your AI assistant into an expert Unity team.

## 🎮 Dev Modes

Choose how much the AI does vs. how much you do:

| Mode | Who Builds | Best For |
|------|-----------|---------|
| **Assistant** | You build. AI documents, advises, and explains. | Learning, full creative control |
| **Mix** *(default)* | Collaborative. AI suggests; you confirm key decisions. | Most projects |
| **Automatic** | AI builds after a short onboarding Q&A. | Rapid prototyping, jam games |

Set your mode in `docs/ProjectConfig.yaml → ai_mode`. You can change it at any time.

**Automatic mode success story**: *"I told the AI my game idea, answered a few setup questions, and it created the scripts, integrated assets, and built a working prototype — minimal manual setup."*

---

## 🧃 Core Philosophy: Game Feel is Not Optional Later

Every feature is built complete using `/implement-feature`:

```
interrogation → GDD/GFD docs → TDD implementation → game feel integration → commit
```

The AI asks about VFX, SFX, camera reactions, and haptics **before writing a single line of code**. Polish is iterative, not a phase. A feature isn't "done" when it compiles — it's done when it plays well.

---

## 📂 Recommended Workspace Structure

> [!IMPORTANT]
> **Don't open the Unity project directly** in Antigravity. Instead, create a **parent workspace folder** containing both your Unity project and docs. This lets the AI read design documents, templates, AND Unity code simultaneously.

```
MyGame/                          ← Open THIS folder in Antigravity
├── .agent/                      ← AI brain (copied from this repo)
│   ├── rules/
│   ├── agents/
│   ├── skills/
│   └── workflows/
├── docs/                        ← Living documents (filled during pre-prod)
│   ├── ProjectConfig.yaml
│   ├── GDD.md
│   ├── TDD.md
│   ├── GFD.md
│   └── SprintPlan.md
├── templates/                   ← Pristine starters (for reference)
└── MyGameUnity/                 ← Unity project (created via Unity Hub)
    ├── Assets/
    ├── Packages/
    └── ProjectSettings/
```

---

## 🚀 Quick Start

1. **Create your workspace**: Create a parent folder for your game (e.g. `MyGame/`).
2. **Copy the toolkit**: Copy all three folders from this repo into your workspace root:
   - `.agent/` — The AI brain (rules, agents, skills, workflows)
   - `templates/` — Pristine document starters
   - `docs/` — Reference documentation and phase guides
3. **Create your Unity project**: Inside the workspace via Unity Hub (e.g. `MyGame/MyGameUnity/`).
4. **Open the workspace**: Open the **parent folder** (not the Unity folder) in Antigravity.
5. **Run `/setup-project`**: The AI will ask your dev mode preference and walk you through full setup.
6. **Connect MCPs** (Agent panel > ... > MCP Servers):
   - **Unity MCP**: For in-editor scene/prefab/asset management
   - **GitHub MCP**: For automated git operations
   - **Linear/Notion MCP**: For task and doc syncing

---

## 🧠 How it Works

- **Global Constitution**: `RULES.md` enforces Unity 6.2+ best practices, performance standards, and thread safety.
- **Auto-Routing**: `AGENTS.md` automatically loads the right agent personas and skills based on your task and `ai_mode`.
- **Expert Skills**: Dedicated skills for UI Toolkit data binding, ScriptableObject architecture, Netcode, game feel, testing, debugging, and more.
- **Integrated Feature Loop**: `/implement-feature` combines code + game feel in one pass, with deep interrogation before any coding starts.
- **Guided Workflows**: Slash commands for every phase of development.

---

## 📅 Project Phases

Follow the phase guides in `docs/phases/` for a standardized development journey:

1. **[00: Ideation](docs/phases/00_Ideation.md)** — From rough idea to GDD + GFD.
2. **[01: Pre-Production](docs/phases/01_PreProduction.md)** — Technical choices, stack definition, naming conventions.
3. **[02: Technical Design](docs/phases/02_TechnicalDesign.md)** — Architecture, Assembly Definitions, patterns.
4. **[03: Project Setup](docs/phases/03_ProjectSetup.md)** — Automated folder scaffolding and package installation.
5. **[04: Production](docs/phases/04_Production.md)** — Feature-by-feature loop (interrogate → implement → feel → commit).
6. **[05: Polish](docs/phases/05_Polish.md)** — Final tuning pass, visual refinement, performance profiling.

---

## 🛠️ Configuration

The AI reads `docs/ProjectConfig.yaml` to stay in sync with your versions, packages, and architectural choices. Key settings:

```yaml
ai_mode: "mix"          # assistant | mix | automatic
prototype_mode: false   # Relaxes architecture rules for rapid iteration
auto_confirm: false     # Skip confirmation prompts (overridden by automatic mode)
```

---

*Created by David-GD13. Targeted for Unity 6.2+ and modern AI workflows.*
