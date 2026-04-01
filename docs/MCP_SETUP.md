# MCP Setup Guide

> Reference during `/uw-cmd-setup-project`. **Never guess MCP package URLs** — use only the links below.

---

## Unity MCP

**Package**: [CoplayDev/unity-mcp](https://github.com/CoplayDev/unity-mcp.git)

```
https://github.com/CoplayDev/unity-mcp.git
```

Install via Unity Package Manager -> Add package from git URL.

**Capabilities**: Inspect/modify scenes, manage prefabs, materials, textures, packages, run tests, read console output.

**Claude Code config** (`.claude/settings.json`):
```json
{
  "mcpServers": {
    "unity": {
      "command": "node",
      "args": ["path/to/unity-mcp/server.js"]
    }
  }
}
```

---

## GitHub MCP

Built into Claude Code. No separate install needed — connect via settings with a GitHub Personal Access Token.

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

## Optional: Tavily Search MCP

**Purpose**: Real-time web search for verifying Unity 6 API changes, package versions, and current best practices.

**When to use**: During `/uw-cmd-implement-feature` Step 4 (verification check). If connected, Claude Code queries Tavily directly instead of asking the user to verify externally.

**Setup**: Requires a Tavily API key (free tier available at [tavily.com](https://tavily.com)).

**Capabilities**: Web search, documentation lookup, package version verification.
