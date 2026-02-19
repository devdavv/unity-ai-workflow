# AI Tool Strategy

> Which tool for which task — and how to keep token costs low.

---

## Tool Routing

| Task Type | Best Tool | Why |
|-----------|-----------|-----|
| Unity MCP operations (run tests, create assets, compile, scene editing) | **Antigravity** | Deep Unity integration via MCP — operates directly inside the editor |
| Complex reasoning, multi-file architecture, new features, debugging | **Claude Code** | Best at multi-step reasoning, large context, and code correctness |
| Inline edits, fast tab-completion, navigating large codebases | **Cursor** | IDE-native autocomplete; great for quick targeted edits inside a familiar file |
| Flow-state focused coding sessions, AI-first IDE feel | **Windsurf** | Minimal friction; good when you want the AI to "drive" with less back-and-forth |
| Long/repetitive tasks: doc drafts, boilerplate, data formatting | **Ollama / LM Studio (local)** | Free, private, no token cost; not suited for complex reasoning |
| Web-grounded research: package versions, pricing, tutorials | **Perplexity AI** | Always up-to-date; great for verifying knowledge before prompting a coding tool |
| Casual brainstorming before a formal session | **ChatGPT Free** | Good for quick ideation; hand off refined ideas to Claude Code or Antigravity |

---

## Token Optimization Strategy

Token costs add up fast. Here's how to route work efficiently:

- **Antigravity**: Use for workflow execution (this workflow), Unity MCP tasks, and feature loops. Its Unity context is unmatched.
- **Claude Code**: Use for architecture decisions, debugging hard problems, and tasks that span many files. Worth the tokens.
- **Cursor / Windsurf**: Use during production sprints for rapid file edits where you already know what to change. Low overhead, fast iteration.
- **Ollama / LM Studio (local)**: Use for anything repetitive and low-stakes — generating doc templates, renaming tables, formatting data, writing first-draft comments. Free and private.
- **Perplexity**: Use as a pre-flight check before prompting a coding AI about tools, packages, or ecosystem choices. Costs nothing on the free tier.

---

## When to Use Cursor vs. Windsurf

Both are AI-first IDEs that embed LLMs directly in your editor. The difference is subtle:

| | Cursor | Windsurf |
|-|--------|----------|
| **Strength** | Powerful tab-completion, large codebase navigation, multi-file edits | Smooth AI-first UX, "Cascade" agentic mode, great for focused single sessions |
| **Best for** | Navigating unfamiliar code, refactoring across files, fast autocomplete | Flow-state sprints, letting the AI take the wheel on a defined task |
| **Model options** | GPT-4o, Claude, Gemini (switchable) | Claude, GPT-4o (Windsurf-native) |
| **Unity workflow fit** | Good for editing C# files during production; no Unity MCP | Same — editor-side only, no Unity MCP |

**Recommendation**: Pick one and get fluent in it. Both are comparable. Use it for production coding edits; use Antigravity for Unity-specific operations.

---

## Open Source AI Options

Use these to preserve paid API tokens on tasks that don't require cutting-edge reasoning.

### Ollama (Local)
Run open-weight models on your own machine. Free, private, no internet required.
- **Install**: [ollama.com](https://ollama.com)
- **Good models**: `llama3.2`, `mistral`, `phi3`, `codellama`
- **Best for**: Doc drafting, simple code scaffolding, formatting data, summarizing files
- **Not suited for**: Complex multi-file reasoning, Unity-specific knowledge, architecture decisions

### LM Studio
GUI wrapper for running local models. Easier to get started than Ollama.
- **Install**: [lmstudio.ai](https://lmstudio.ai)
- **Best for**: One-off tasks, experimenting with models, offline use

### Perplexity AI
Web search + AI reasoning. Free tier available.
- **Best for**: Verifying current package versions, pricing, Unity forum answers, finding tutorials
- **Not a coding assistant**: Use it for research, then bring the results into Claude Code or Antigravity

---

## Unity-Specific AI

Unity's first-party AI suite (**Unity AI**, currently open beta as of Unity 6.2):

> **⚠️ Pre-release warning**: Unity AI is not yet production-ready. Unity's ToS currently limits use to test projects only until GA.

### Unity AI: Assistant
In-editor contextual AI. Answers questions about your project, generates code, resolves console errors, and runs batch operations (rename GameObjects, create NPC variants). Runs in Unity Cloud.
- **Use for**: Quick in-editor questions, resolving errors without leaving Unity, batch operations
- **Not a replacement for**: Deep multi-file architecture work (use Claude Code for that)

### Unity AI: Generators
Asset generation for sprites, textures, animations, and audio. Cloud-based, uses Unity's models + third-party APIs.
- **Use for**: Rapid asset prototyping, placeholder art before final assets
- **Not for**: Final production art (quality varies; always verify outputs)

### Unity AI: Inference Engine *(formerly Sentis)*
Local AI model inference at runtime. Free. Runs entirely on-device — no data leaves the machine.
- **Package**: `com.unity.ai.inference` (renamed from `com.unity.sentis`)
- **Use for**: In-game ML models — gesture recognition, custom NPC decision-making, procedural behavior
- **Not a coding assistant**: This is for shipping AI *inside your game*, not for development assistance
- **Migrating from Sentis?**: Update the package reference only. The functionality is identical.

### Unity Muse *(retired)*
Officially discontinued. Do not use for new projects. Replaced by Unity AI.

### ML Agents
For training in-game AI via reinforcement learning. Completely separate from the above — this is for NPC behavior training, not a development assistant.

---

## Reference: This Workflow's Tool Stack

| Phase | Primary Tool | Why |
|-------|-------------|-----|
| Ideation / Brainstorm | Antigravity | Runs `/brainstorm`, updates GDD/GFD |
| Pre-Production | Antigravity | Fills ProjectConfig, guides questionnaire |
| Technical Design | Antigravity + Claude Code | Architecture reasoning benefits from Claude Code |
| Project Setup | Antigravity | Unity MCP runs package install, folder creation |
| Production (feature loop) | Antigravity | Full `/implement-feature` loop with Unity MCP |
| Quick file edits | Cursor / Windsurf | Inline edits during sprints |
| Debugging | Antigravity | MCP reads console; Claude Code for hard logic bugs |
| Polish | Antigravity | Game feel integrator + Unity MCP |

---

## Claude Code Skills Ecosystem

Claude Code is now a **first-class supported tool** for this workflow. The repo includes `CLAUDE.md` (auto-loaded instructions) and `.claude/commands/` (9 slash commands). In-repo skills live in `.agent/skills/` and are shared between Claude Code and Antigravity. External skills supplement when a task falls outside the built-in scope.

### Verified Unity-Relevant External Skills

| Skill | Source | Purpose |
|-------|--------|---------|
| `unity-developer` | [fastmcp.me](https://fastmcp.me/Skills/Details/839/unity-developer) | Symbol-based code editing, scene manipulation, Unity MCP integration |
| `game-developer-specialist` | [mcpmarket.com](https://mcpmarket.com/tools/skills/game-development-specialist) | Unity 6 LTS, URP/HDRP pipelines, cross-platform deployment |
| TheOne Studio Unity skills | [github.com/The1Studio/theone-training-skills](https://github.com/The1Studio/theone-training-skills) | VContainer, SignalBus, UniTask, Data Controllers (training-enforced patterns) |

> In-repo skills (`.agent/skills/`) take priority. External skills fill gaps. Check `skills.sh` [UNVERIFIED: rate-limited during research] for additional options.

### Reference Repos
These repos informed the design of this workflow — check them for workflow patterns and tool ideas:

| Repo | Key Insight |
|------|-------------|
| [superpowers](https://github.com/obra/superpowers) | Composable skill format; design → plan → execute → QA phases |
| [everything-claude-code](https://github.com/affaan-m/everything-claude-code) | 40+ skills, session lifecycle hooks, instinct-based learning patterns |
| [GSD](https://github.com/gsd-build/get-shit-done) | STATE.md for session continuity; wave-based parallel execution |
| [awesome-claude-code](https://github.com/hesreallyhim/awesome-claude-code) | Curated list; session restore tools (Recall, claude-code-tools) |
| [ccpm](https://github.com/automazeio/ccpm) | GitHub Issues as task database when Linear isn't used |

### Optional Advanced MCPs
For complex math validation: **Wolfram Alpha MCP** — useful when implementing physics systems (trajectories, procedural generation). Low priority for most features; connect on demand.
For real-time Unity API lookup: **Tavily MCP** — see `MCP_SETUP.md`.
