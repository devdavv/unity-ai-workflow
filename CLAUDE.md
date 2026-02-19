# Unity AI Workflow — Claude Code Instructions

> **This file is auto-loaded by Claude Code on every task. It is the operating manual.**

## What This Is

AI-first Unity 6.2+ game development workflow. This repo is pure infrastructure — rules, agents, skills, workflows, docs, and templates. No Unity project inside. Copy it into your game workspace root and Claude Code reads it as its brain.

## Workspace Layout

When used in a real game project, the workspace looks like this:

```
MyGame/                          ← Open this folder
├── CLAUDE.md                    ← This file (auto-loaded)
├── .claude/commands/            ← Slash commands (/implement-feature, /brainstorm, etc.)
├── .agent/
│   ├── agents/                  ← 8 agent personas
│   ├── skills/                  ← 11 reusable skills with references
│   └── rules/                   ← RULES.md + AGENTS.md (Antigravity format)
├── docs/                        ← Living project docs
│   ├── ProjectConfig.yaml       ← Project settings (ai_mode, packages, versions)
│   ├── GDD.md, TDD.md, GFD.md  ← Design documents
│   └── phases/                  ← 6 phase guides (Ideation → Polish)
├── templates/                   ← Pristine starters
└── MyGameUnity/                 ← Unity project (Assets/, Packages/, etc.)
```

---

## Available Commands

| Command | Purpose |
|---------|---------|
| `/brainstorm` | Guided ideation → GDD sections |
| `/plan` | Break features into tasks with estimates and dependencies |
| `/create` | TDD-first code generation for non-player-facing systems |
| `/implement-feature` | Full feature loop with game feel — **preferred for gameplay** |
| `/test` | Write and run tests mapped from GDD Gherkin scenarios |
| `/debug` | Systematic 4-phase debugging |
| `/polish` | Juice pass for working features (VFX, SFX, camera, tweens) |
| `/review` | Code review against project standards |
| `/setup-project` | Full project initialization wizard |

---

## Core Rules (always enforced)

### Engine & Platform
- **Unity 6.2+** (override: `ProjectConfig.yaml → unity_version`)
- **URP** render pipeline (override: `ProjectConfig.yaml → render_pipeline`)
- **IL2CPP** scripting backend
- **New Input System only** — never `Input.GetKey*`, `Input.GetAxis*`
- **UI Toolkit** default (override: `ProjectConfig.yaml → ui_system`)
- **Awaitable** for async (Unity 6 native)
- **Visual Scripting forbidden** for logic. Shader Graph & VFX Graph are fine.

### Code Quality
- `[SerializeField] private` only — never expose fields as `public` for Inspector
- Cache ALL component references in `Awake()` or `Start()`
- **Never** `GetComponent<T>()`, `FindObjectOfType<T>()`, or `GameObject.Find()` in `Update()`, `FixedUpdate()`, or `LateUpdate()`
- Prefer `TryGetComponent<T>()` when the component may not exist
- Use `GameDebug` wrapper (stripped from release) — never raw `Debug.Log()`
- Never call Unity API from background threads

### Documentation
- `/// <summary>` only on specific public API methods that warrant explanation
- Never XML summaries on classes or obvious private methods
- Follow `docs/CODING_STANDARDS.md`, `docs/NAMING_CONVENTIONS.md`, `docs/GIT_CONVENTIONS.md`

### Unity-Specific
- **Never** create, modify, or delete `.meta` files
- All feature code must live inside an `.asmdef`
- Always `[CreateAssetMenu]` on ScriptableObjects
- Check `ProjectConfig.yaml` for package versions before referencing any API

### Prototype Mode
When `prototype_mode: true` in ProjectConfig: assembly definitions optional, public fields allowed, inline constants permitted, docs optional. But GameDebug, component caching, no GetComponent in Update, New Input System, and no .meta touching are **always enforced**.

---

## Dev Modes

Read `docs/ProjectConfig.yaml → ai_mode` at session start:

| Mode | Behavior |
|------|----------|
| **assistant** | User drives. AI advises, documents, explains. Always wait for confirmation. Verbose explanations. |
| **mix** *(default)* | Collaborative. Proceed on simple tasks. Confirm on complex tasks, new features, architecture. Moderate detail. |
| **automatic** | AI works autonomously after onboarding Q&A. Only pause for destructive actions. Minimal explanation. |

**Automatic mode onboarding** — before proceeding, ask: game pitch + core loop, target platforms, genre, 1-3 reference games, rough feature list, preferred packages, MCP availability.

---

## Agent Routing

Before starting work, classify the task and load the appropriate context:

| Task Type | Agent Persona | Skills to Load |
|-----------|--------------|----------------|
| Ideation / Brainstorming | Game Designer | — |
| Architecture / Structure | Architect | `unity-feature-scaffold` |
| Gameplay Logic / Systems | Gameplay Dev + Game Designer | `scriptable-object-arch`, `game-feel-integrator` |
| Editor Tools / Inspectors | Tool Developer | `unity-editor-tools` |
| User Interface | UI Specialist | `ui-toolkit-binder` |
| Networking / Multiplayer | Network Engineer | `network-setup` |
| Testing / QA | QA Tester | `unity-test-runner` |
| Debugging / Errors | QA Tester | `unity-debugging` |
| Polish / Game Feel | Game Designer | `game-feel-integrator` |
| Art / Scene Setup | Art Director | — (Unity MCP tools) |
| Project Setup | Architect | `unity-project-setup` |
| Code Review | QA Tester | `code-review` |

**How to load context:**
1. Read the agent persona from `.agent/agents/{name}.md`
2. Read the skill from `.agent/skills/{name}/SKILL.md`
3. Read project docs: `docs/GDD.md`, `docs/TDD.md`, `docs/GFD.md`, `docs/ProjectConfig.yaml` (whichever are relevant)
4. Announce loaded context to the user before proceeding

**Multi-domain tasks:** Load ALL relevant agents/skills. Use the primary agent for the dominant aspect, note secondary concerns.

---

## Workflow Suggestion

When a task aligns with a command, proactively suggest it:
- New idea → `/brainstorm`
- Breaking down work → `/plan`
- Player-facing feature → `/implement-feature` (preferred — includes game feel)
- Utility/helper code → `/create`
- After implementation → `/test`
- Bug or unexpected behavior → `/debug`
- Feature works but feels flat → `/polish`
- Code written, tests passing → `/review`

In **automatic mode**, invoke the workflow directly instead of suggesting.

---

## TCREI Prompting

Before loading context, evaluate prompt quality. If the request lacks clarity, apply TCREI:
- **T**ask — What exactly needs to be done?
- **C**ontext — Background, constraints, current state
- **R**eferences — Example games, repos, prior art
- **E**valuate — After first response, what's right/wrong/missing?
- **I**terate — Refine from there

---

## Verification Marking

When recommending any external tool, package, repo, tutorial, or statistic:
- `[VERIFIED: source]` — accessed/confirmed this session
- `[SYNTHESIZED: source]` — AI-researched, not primary
- `[UNVERIFIED: training data]` — needs Perplexity/web check

Never present unverified information as fact. If Tavily MCP is available, verify before responding.

---

## Session Hygiene

After a full `/implement-feature` loop OR when 3+ major decisions have been covered, offer:
> "This session is getting long. Want me to write a SessionState summary you can paste into a fresh conversation?"

Use `templates/SessionState_Template.md` as the format.

---

## Prompt Logging

If `docs/ProjectConfig.yaml → prompt_logging` is `true`, append to `docs/PromptLog.md` after every response:
```
## [YYYY-MM-DD HH:MM] — <Workflow or Task Type>
**Prompt:** <user's exact words>
**Summary:** <≤200 word factual summary>
---
```

---

## MCP Tool Guidance

Check MCP availability before attempting to use tools. If not connected, fall back to file-based operations.

| MCP Server | When to Use |
|------------|-------------|
| **Unity MCP** | Scene work, prefab management, test running, console reading |
| **GitHub MCP** | Push, PRs, branches, collaboration |
| **Linear MCP** | Sprint planning, task tracking |
| **Notion MCP** | Documentation sync |

---

## Knowledge Freshness

AI knowledge has a cutoff. When referencing Unity packages, third-party tools, or pricing, note:
> "My knowledge has a cutoff — for current versions or pricing, a quick web search will give fresher data."

---

## Skill Self-Improvement

If you notice repetitive patterns without an existing skill, suggest creating one:
> "I've done this pattern twice now. Want me to create a skill for it using the skill-creator?"

Skills live in `.agent/skills/{name}/SKILL.md`. See `.agent/skills/skill-creator/SKILL.md` for the creation guide.
