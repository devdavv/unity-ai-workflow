# AI Tool Strategy

> Claude Code is the primary development tool for this workflow. Other AI tools can supplement specific tasks.

---

## Claude Code — Primary Tool

Claude Code handles all phases of development through this workflow:

| Phase | Workflow | How |
|-------|---------|-----|
| Ideation | `/uw-cmd-brainstorm` | TCREI-guided ideation, GDD/GFD population |
| Pre-Production | `/uw-cmd-setup-project` | ProjectConfig, folder scaffold, package install, Git |
| Technical Design | Manual | Architecture reasoning, TDD authoring |
| Production | `/uw-cmd-implement-feature` | Full feature loop: interrogate -> TDD -> feel -> commit |
| Testing | `/uw-cmd-test` | NUnit test generation from Gherkin scenarios |
| Debugging | `/uw-cmd-debug` | 4-phase systematic debugging |
| Polish | `/uw-cmd-polish` | Game feel tuning via GFD Feedback Matrix |
| Code Review | `/uw-cmd-review` | Unity-specific pre-commit quality checklist |

### Key Features Used
- **Auto mode**: Classifies tool calls as safe/risky automatically. Enable for autonomous dev mode.
- **Slash commands**: 9 workflows in `.claude/commands/`
- **Skills**: 13 knowledge modules in `.claude/skills/` with progressive disclosure
- **Hooks**: Automated workflows via `.claude/settings.json`
- **MCP servers**: Unity, GitHub, Linear, Notion integrations
- **Native code review**: Multi-agent parallel PR review via GitHub App (supplements `/uw-cmd-review`)

---

## Supplementary Tools

| Task | Tool | Why |
|------|------|-----|
| Unity editor operations (scenes, prefabs, tests) | **Unity MCP** | Direct in-editor manipulation |
| Web-grounded research (package versions, tutorials) | **Perplexity AI** | Always up-to-date verification |
| Casual brainstorming before a formal session | **ChatGPT / Claude.ai** | Quick ideation, then hand off to Claude Code |
| Local LLM for repetitive tasks | **Ollama / LM Studio** | Free, private, no token cost |

---

## Unity-Specific AI

### Unity AI: Assistant
In-editor contextual AI (open beta as of Unity 6.2). Resolves console errors, batch operations.
- Not a replacement for deep multi-file architecture work.

### Unity AI: Inference Engine *(formerly Sentis)*
Local AI model inference at runtime (`com.unity.ai.inference`). For shipping AI *inside your game*, not development assistance.

---

## Skills 2.0

This workflow uses Anthropic's Skills 2.0 format. Skills live in `.claude/skills/` with progressive disclosure:

1. **Metadata** (name + description) — always in context (~100 words)
2. **SKILL.md body** — loaded when triggered (<500 lines ideal)
3. **References/** — loaded only when the skill determines it's needed

The `uw-skill-creator` skill (`.claude/skills/uw-skill-creator/`) is the official Anthropic tool for creating, testing, and optimizing skills with evaluation frameworks and description optimization.

### Verified Unity-Relevant External Skills

| Skill | Source | Purpose |
|-------|--------|---------|
| `unity-developer` | [fastmcp.me](https://fastmcp.me/Skills/Details/839/unity-developer) | Symbol-based code editing, Unity MCP integration |
| `game-developer-specialist` | [mcpmarket.com](https://mcpmarket.com/tools/skills/game-development-specialist) | Unity 6, URP/HDRP pipelines |

> In-repo skills (`.claude/skills/`) take priority. External skills fill gaps.
