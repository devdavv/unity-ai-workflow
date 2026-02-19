# MCP Setup Guide

> Reference for agents during `/setup-project`. **Never guess MCP package URLs**  — use only the links below.

---

## Unity MCP

**Package**: [CoplayDev/unity-mcp](https://github.com/CoplayDev/unity-mcp.git)

```
https://github.com/CoplayDev/unity-mcp.git
```

Install via Unity Package Manager → Add package from git URL.

**Capabilities**: Inspect/modify scenes, manage prefabs, materials, textures, packages, run tests, read console output.

---

## GitHub MCP

Built into most AI coding assistants (Antigravity, Claude Code, Cursor). No separate install needed — just connect via the assistant's MCP settings with a GitHub Personal Access Token.

**Capabilities**: Create repos, branches, commits, PRs, manage issues.

---

## Linear MCP

**Official docs**: [linear.app/docs/mcp](https://linear.app/docs/mcp)

Follow Linear's official setup instructions. Do NOT guess package URLs.

**Capabilities**: Create/manage issues, projects, sprints, labels.

---

## Notion MCP

**Official docs**: [developers.notion.com/guides/mcp](https://developers.notion.com/guides/mcp/mcp)

Follow Notion's official setup instructions. Do NOT guess package URLs.

**Capabilities**: Read/write pages, databases, create design docs.

---

## Optional MCPs

### Tavily Search MCP

**Purpose**: Real-time web search for verifying Unity 6 API changes, package versions, and current best practices — without leaving Antigravity.

**When to use**: During `/implement-feature` Step 4 (Knowledge Freshness check). If connected, the agent queries Tavily directly instead of asking the user to use Perplexity.

**Setup**: Find the Tavily MCP via Antigravity's MCP settings or `skills.sh` [UNVERIFIED: verify current package name]. Requires a Tavily API key (free tier available at [tavily.com](https://tavily.com)).

**Capabilities**: Web search, documentation lookup, package version verification.

---

## Claude Code Only

> The following tools are for the **Claude Code** workflow adaptation (future roadmap). They do not apply to Antigravity.

### claude-mem

**Repo**: [github.com/thedotmack/claude-mem](https://github.com/thedotmack/claude-mem)

**Purpose**: Persistent cross-session memory for Claude Code. Captures tool usage and project knowledge via lifecycle hooks → stores in SQLite + vector search (Chroma) → injects relevant context into new sessions.

**Antigravity equivalent**: Use `templates/SessionState_Template.md` to manually summarize and carry context across sessions. The agent will offer to write this when a session gets long (see RULES.md → Context Window Hygiene).
