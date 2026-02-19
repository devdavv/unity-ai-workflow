# Unity AI Workflow

A comprehensive, agent-first toolkit for AI-assisted Unity development. Designed primarily for **Google Antigravity IDE**, this repository provides the "brain" (rules, agents, skills, and workflows) to turn your AI assistant into an expert Unity team.

> **No Unity project inside** — this repo is pure workflow infrastructure (rules, agents, skills, docs, templates). You copy it into your game's workspace and the AI reads it as its operating manual.

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
- **TCREI Prompting**: Agents use the Task-Context-References-Evaluate-Iterate methodology for structured, high-quality outputs.
- **Verification System**: Every AI recommendation is marked `[VERIFIED]`, `[SYNTHESIZED]`, or `[UNVERIFIED]` — no silent hallucinations.
- **Expert Skills**: Dedicated skills for UI Toolkit data binding, ScriptableObject architecture, Netcode, game feel, testing, debugging, and more.
- **Integrated Feature Loop**: `/implement-feature` combines code + game feel in one pass, with deep interrogation before any coding starts.
- **Session Handoff**: `SessionState` template ensures context survives across AI sessions.
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

## 📋 Roadmap

See **[ROADMAP.md](ROADMAP.md)** for the full version history and what's planned next.

---

## 🙏 Resources & Inspiration

This workflow was built with the help of these resources. Ratings reflect how much each influenced the final result.

### GitHub Repos

| Resource | Rating | Influence |
|----------|--------|-----------|
| [anthropics/skills](https://github.com/anthropics/skills) | ⭐⭐⭐⭐⭐ | Official skill format — canonical reference for skill structure |
| [sickn33/antigravity-awesome-skills](https://github.com/sickn33/antigravity-awesome-skills) | ⭐⭐⭐⭐⭐ | Core inspiration for Antigravity-native skill patterns |
| [Common-ka/ai-agent-unity-rules](https://github.com/Common-ka/ai-agent-unity-rules) | ⭐⭐⭐⭐ | Unity-specific AI rules — shaped our rules/agents structure |
| [vudovn/antigravity-kit](https://github.com/vudovn/antigravity-kit) | ⭐⭐⭐⭐ | Workflow/skill structure reference for `.agent/` layout |
| [obra/superpowers](https://github.com/obra/superpowers) | ⭐⭐⭐⭐ | Agent persona / superpowers philosophy — influenced agent routing |
| [gsd-build/get-shit-done](https://github.com/gsd-build/get-shit-done) | ⭐⭐⭐⭐ | Pragmatic workflow philosophy |
| [CoplayDev/unity-mcp](https://github.com/CoplayDev/unity-mcp) | ⭐⭐⭐⭐ | Unity MCP integration — directly referenced in MCP setup |
| [stillwwater/UnityStyleGuide](https://github.com/stillwwater/UnityStyleGuide) | ⭐⭐⭐⭐ | Naming conventions baseline |
| [justinwasilenko/Unity-Style-Guide](https://github.com/justinwasilenko/Unity-Style-Guide) | ⭐⭐⭐⭐ | Folder structure + conventions |
| [nextlevelbuilder/ui-ux-pro-max-skill](https://github.com/nextlevelbuilder/ui-ux-pro-max-skill) | ⭐⭐⭐ | UI skill patterns (adapted for UI Toolkit) |
| [affaan-m/everything-claude-code](https://github.com/affaan-m/everything-claude-code) | ⭐⭐⭐ | Claude Code ecosystem overview |
| [thedotmack/claude-mem](https://github.com/thedotmack/claude-mem) | ⭐⭐⭐ | Memory/session persistence patterns |
| [hesreallyhim/awesome-claude-code](https://github.com/hesreallyhim/awesome-claude-code) | ⭐⭐⭐ | Curated list — helped discover other repos |
| [automazeio/ccpm](https://github.com/automazeio/ccpm) | ⭐⭐⭐ | Claude Code project management patterns |

### YouTube Videos

| Video | Creator | Rating | Influence |
|-------|---------|--------|-----------|
| [Essential AI Skills For 2026](https://youtu.be/jm2jBW462bU) | Tina Huang | ⭐⭐⭐⭐⭐ | Introduced TCREI methodology and key tools we implemented |
| [You're Using Claude Code Wrong — 10 Tips](https://youtu.be/V9atNrDjnZs) | Simon Scrapes \| AI Automation | ⭐⭐⭐⭐½ | Inspired the verification/hallucination-checking system |
| [How I Use Claude Code (Meta Staff Engineer Tips)](https://youtu.be/mZzhfPle9QU) | John Kim | ⭐⭐⭐½ | Helpful Claude Code usage patterns |
| [Google's AI Prompt Engineering Course in 20 Min](https://youtu.be/p09yRj47kNM) | Tina Huang | ⭐⭐⭐ | Good prompting fundamentals |

### Web Resources

| Resource | Rating | Influence |
|----------|--------|-----------|
| [Unity — Organizing Your Project](https://unity.com/how-to/organizing-your-project) | ⭐⭐⭐⭐⭐ | Official guidance — directly shaped recommended folder structure |
| [TCREI Prompt Engineering Framework](https://medium.com/@datasciencedisciple/2-prompt-engineering-frameworks-to-become-top-2-ai-user-b366b0e7367a) | ⭐⭐⭐⭐⭐ | TCREI methodology is baked into AGENTS.md |
| [Loic Jacob — Game Feel Theory](https://loicjacob.notion.site/generate-strong-and-consistent-sensations-in-the-gameplay) | ⭐⭐⭐⭐⭐ | ADSR envelope, Three Pillars, Sensation Vocabulary |
| [skills.sh](https://skills.sh/) | ⭐⭐⭐⭐ | Skill discovery catalog |
| [Unity AI Features](https://unity.com/features/ai) | ⭐⭐⭐ | Context for Unity's AI direction |
| [Unity Sentis Documentation](https://docs.unity3d.com/Packages/com.unity.ai.inference@2.2/manual/) | ⭐⭐⭐ | Reference for in-engine AI inference |
| [Unity 6 AI Manual](https://docs.unity3d.com/6000.3/Documentation/Manual/unity-ai.html) | ⭐⭐⭐ | Future-facing Unity AI reference |

*Plus various AI-assisted research sessions (Perplexity, Google AI Studio) for game feel theory, shader patterns, and asset pipeline research.*

---

*Created by [David-GD13](https://github.com/David-GD13). Targeted for Unity 6.2+ and modern AI workflows.*
