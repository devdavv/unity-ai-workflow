# Unity AI Workflow

A comprehensive, skills-first toolkit for AI-assisted Unity development, built for **Claude Code**. This repository provides the "brain" (skills, commands, docs, and templates) to turn Claude Code into an expert Unity team.

> **No Unity project inside** — this repo is pure workflow infrastructure. You copy it into your game's workspace and Claude Code reads it as its operating manual.

## Dev Modes

Choose how much the AI does vs. how much you do:

| Mode | Who Builds | Best For |
|------|-----------|---------|
| **Guided** *(default)* | Collaborative. AI suggests; you confirm key decisions. | Most projects |
| **Autonomous** | AI builds after a short onboarding Q&A. | Rapid prototyping, jam games |

Set your mode in `docs/ProjectConfig.yaml -> ai_mode`. Enable Claude Code auto mode alongside autonomous for best results.

---

## Core Philosophy: Game Feel is Not Optional Later

Every feature is built complete using `/uw-cmd-implement-feature`:

```
interrogation -> GDD/GFD docs -> TDD implementation -> game feel integration -> commit
```

The AI asks about VFX, SFX, camera reactions, and haptics **before writing a single line of code**. Polish is iterative, not a phase. A feature isn't "done" when it compiles — it's done when it plays well.

---

## Recommended Workspace Structure

> [!IMPORTANT]
> **Don't open the Unity project directly.** Create a **parent workspace folder** containing both your Unity project and docs. This lets Claude Code read design documents, templates, AND Unity code simultaneously.

```
MyGame/                          <- Open THIS folder in Claude Code
├── CLAUDE.md                    <- Auto-loaded instructions (minimal, ~80 lines)
├── .claude/
│   ├── commands/                <- Slash commands (/uw-cmd-implement-feature, /uw-cmd-brainstorm, etc.)
│   ├── skills/                  <- 13 reusable knowledge modules
│   └── settings.json            <- Hooks & permissions
├── docs/                        <- Living documents (filled during development)
│   ├── ProjectConfig.yaml
│   ├── GDD.md, TDD.md, GFD.md
│   └── phases/                  <- 6 phase guides
├── templates/                   <- Pristine starters (for reference)
└── MyGameUnity/                 <- Unity project (created via Unity Hub)
    ├── Assets/
    ├── Packages/
    └── ProjectSettings/
```

---

## Quick Start

1. **Create your workspace**: Create a parent folder for your game (e.g., `MyGame/`).
2. **Copy the toolkit**: Copy everything from this repo into your workspace root.
3. **Create your Unity project**: Inside the workspace via Unity Hub (e.g., `MyGame/MyGameUnity/`).
4. **Open the workspace** in your terminal or VS Code with Claude Code.
5. `CLAUDE.md` is **auto-loaded** — rules and workflow suggestions are built in.
6. **Run `/uw-cmd-setup-project`**: Claude Code walks you through dev mode selection and full project initialization.

---

## How It Works

- **Minimal CLAUDE.md**: ~80 lines of non-discoverable Unity gotchas — only what the AI consistently gets wrong. No bloat. Based on research showing verbose context files reduce AI performance.
- **Skills 2.0**: 13 reusable knowledge modules in `.claude/skills/`, using Anthropic's latest skill format with progressive disclosure (metadata -> body -> references).
- **Slash Commands**: 9 workflows as Claude Code commands — `/uw-cmd-brainstorm`, `/uw-cmd-plan`, `/uw-cmd-create`, `/uw-cmd-implement-feature`, `/uw-cmd-test`, `/uw-cmd-debug`, `/uw-cmd-polish`, `/uw-cmd-review`, `/uw-cmd-setup-project`.
- **Verification System**: Every AI recommendation is marked `[VERIFIED]`, `[SYNTHESIZED]`, or `[UNVERIFIED]` — no silent hallucinations.
- **Integrated Feature Loop**: `/uw-cmd-implement-feature` combines code + game feel in one pass, with deep interrogation before any coding starts.
- **Session Handoff**: `SessionState` template ensures context survives across conversations.
- **Hooks**: Claude Code hooks in `.claude/settings.json` for automated workflows.

---

## Skills

| Skill | Purpose |
|-------|---------|
| `uw-game-feel-integrator` | Inject juice (VFX, SFX, camera, tweens, haptics) using Rule of Three |
| `uw-scriptable-object-arch` | SO-first data architecture (default pattern) |
| `uw-dependency-injection` | DI-first using Reflex (opt-in) |
| `uw-state-machine` | Interface-based FSMs for game flow and characters |
| `uw-ui-toolkit-binder` | Unity 6+ Runtime Data Binding (MVVM) |
| `uw-network-setup` | Package-agnostic networking boilerplate |
| `uw-unity-project-setup` | Full project initialization wizard |
| `uw-unity-feature-scaffold` | Feature folder + asmdef + test assembly |
| `uw-unity-test-runner` | NUnit test generation from Gherkin scenarios |
| `uw-unity-debugging` | GameDebug wrapper with conditional compilation |
| `uw-unity-editor-tools` | Custom Inspectors, Gizmos, EditorWindows |
| `uw-code-review` | Unity-specific pre-commit quality checklist |
| `uw-skill-creator` | Official Anthropic skill-creator 2.0 — create, test, and optimize new skills |

---

## Project Phases

Follow the phase guides in `docs/phases/` for a standardized development journey:

1. **[00: Ideation](docs/phases/00_Ideation.md)** — From rough idea to GDD + GFD.
2. **[01: Pre-Production](docs/phases/01_PreProduction.md)** — Technical choices, stack definition, naming conventions.
3. **[02: Technical Design](docs/phases/02_TechnicalDesign.md)** — Architecture, Assembly Definitions, patterns.
4. **[03: Project Setup](docs/phases/03_ProjectSetup.md)** — Automated folder scaffolding and package installation.
5. **[04: Production](docs/phases/04_Production.md)** — Feature-by-feature loop (interrogate -> implement -> feel -> commit).
6. **[05: Polish](docs/phases/05_Polish.md)** — Final tuning pass, visual refinement, performance profiling.

---

## Configuration

The AI reads `docs/ProjectConfig.yaml` to stay in sync with your versions, packages, and architectural choices. Key settings:

```yaml
ai_mode: "guided"        # guided | autonomous
prototype_mode: false     # Relaxes architecture rules for rapid iteration
```

---

## Roadmap

See **[ROADMAP.md](ROADMAP.md)** for the full version history and what's planned next.

---

## Resources & Inspiration

This workflow was built with the help of these resources. Ratings reflect how much each influenced the final result.

### Key Influences

| Resource | Rating | Influence |
|----------|--------|-----------|
| [anthropics/skills](https://github.com/anthropics/skills) | ***** | Official skill format — canonical reference, skill-creator 2.0 |
| [Common-ka/ai-agent-unity-rules](https://github.com/Common-ka/ai-agent-unity-rules) | **** | Unity-specific AI rules — shaped rules structure |
| [obra/superpowers](https://github.com/obra/superpowers) | **** | Composable skill format, agent routing philosophy |
| [gsd-build/get-shit-done](https://github.com/gsd-build/get-shit-done) | **** | Pragmatic workflow philosophy, session state |
| [CoplayDev/unity-mcp](https://github.com/CoplayDev/unity-mcp) | **** | Unity MCP integration |
| [stillwwater/UnityStyleGuide](https://github.com/stillwwater/UnityStyleGuide) | **** | Naming conventions baseline |
| [PracticAPI — The Perfect Unity Project](https://www.patreon.com/posts/perfect-unity-132380321) | ***** | Reference architecture for production Unity 6 |
| [Loic Jacob — Game Feel Theory](https://loicjacob.notion.site/generate-strong-and-consistent-sensations-in-the-gameplay) | ***** | ADSR envelope, Three Pillars, Sensation Vocabulary |
| [TCREI Prompt Engineering Framework](https://medium.com/@datasciencedisciple/2-prompt-engineering-frameworks-to-become-top-2-ai-user-b366b0e7367a) | ***** | TCREI methodology baked into commands |

### Research That Shaped v2.0

| Resource | Key Insight |
|----------|-------------|
| [Addy Osmani — AGENTS.md](https://addyosmani.com/blog/agents-md/) | Verbose context files reduce AI success rates. Only non-discoverable info belongs. |
| [Zazen Codes — Stop using AGENTS.md](https://zazencodes.substack.com/p/stop-using-agentsmd-and-claudemd) | Keep files short, eliminate duplication, agent-specific instructions only |
| [Claude Blog — Auto Mode](https://claude.com/blog/auto-mode) | Native auto mode replaces custom autonomous dev modes |
| [Claude Blog — Code Review](https://claude.com/blog/code-review) | Multi-agent parallel review, GitHub App integration |
| [Claude Blog — Hooks](https://claude.com/blog/how-to-configure-hooks) | 8 hook types for workflow automation |
| [Claude Blog — Skills](https://claude.com/blog/improving-skill-creator-test-measure-and-refine-agent-skills) | Evaluation-driven skill development with quantitative benchmarks |

---

*Created by [devdavv](https://github.com/devdavv). Targeted for Unity 6.2+ and modern AI workflows.*
