# Unity AI Workflow — Auto-Routing Protocol

> **This file tells the AI HOW to determine which context to load for any given task.**
> The AI reads this file first, then announces its context selection to the user before proceeding.

---

## Routing Table

| If the task involves... | Templates to Read | Agent Persona | Primary Skills |
|-------------------------|-------------------|---------------|----------------|
| **Ideation / Brainstorming** | `GDD`, `GFD` | Game Designer | — |
| **Architecture / Structure** | `TDD`, `ProjectConfig` | Architect | `unity-feature-scaffold` |
| **Gameplay Logic / Systems** | `GDD`, `TDD`, `GFD` | Gameplay Dev + Game Designer | `scriptable-object-arch`, `game-feel-integrator` |
| **Editor Tools / Inspectors** | `TDD` | Tool Developer | `unity-editor-tools` |
| **User Interface** | `TDD` (UI section), `GFD` | UI Specialist | `ui-toolkit-binder` |
| **Networking / Multiplayer** | `TDD` (network section), `GDD` | Network Engineer | `network-setup` |
| **Testing / QA** | `GDD` (Gherkin specs), `TDD` | QA Tester | `unity-test-runner` |
| **Debugging / Errors** | `TDD`, `ProjectConfig` | QA Tester | `unity-debugging` |
| **Polish / Game Feel** | `GFD` | Game Designer | `game-feel-integrator` |
| **Art / Scene Setup** | `GFD`, `TDD` | Art Director | — (Unity MCP tools) |
| **Project Setup** | `ProjectConfig`, `TDD` | Architect | `unity-project-setup` |

---

## Execution Protocol

### Step 1: Analyze
Classify the user's request into one or more rows from the table above.

### Step 2: Announce
Before loading any context, tell the user what you're about to do:

```
🧠 Context Loading:
  📋 Task Type: [Gameplay Logic]
  👤 Agent: Gameplay Dev
  📄 Reading: GDD, TDD
  🔧 Skills: scriptable-object-arch
  
  Proceed? (or type a different approach)
```

### Step 3: Load
Read the indicated template files from the project's `docs/` folder (the filled-in versions, not the pristine templates). Read the agent persona file from `.agent/agents/`. Read the skill instructions from `.agent/skills/`.

### Step 4: Confirm (mode-dependent)
Read `ai_mode` from `ProjectConfig.yaml`:
- **assistant**: Always wait for explicit user confirmation, even for simple tasks. Present options with tradeoffs.
- **mix**: Proceed for simple tasks (adding a method, fixing a bug). Wait for confirmation on complex tasks (new feature, architecture change, new agent/skill loaded).
- **automatic**: Proceed immediately after announcing. Only pause for destructive actions (file deletion, force push, irreversible changes).

### Step 5: Suggest Workflows (mode-aware)
In **automatic mode**, don't suggest — just invoke the appropriate workflow directly.
In **mix/assistant mode**, suggest and wait:
If the task maps to a workflow command, suggest it:
- New idea → suggest `/brainstorm`
- Breaking down a sprint → suggest `/plan`
- Implementing a new feature → suggest `/implement-feature` (preferred — includes game feel)
- Writing isolated code / utility → suggest `/create`
- After implementation → suggest `/test`
- Bug or unexpected behavior → suggest `/debug`
- Feature works but feels "dry" → suggest `/polish`

---

## MCP Tool Guidance

Agents should leverage MCP tools when they would improve the task:

| MCP Server | When to Use | Example Actions |
|------------|-------------|-----------------|
| **Unity MCP** | Scene work, testing, debugging | `manage_scene`, `manage_prefabs`, `run_tests`, `read_console` |
| **GitHub MCP** | Version control, collaboration | `push_files`, `create_pull_request`, `create_branch` |
| **Linear MCP** | Sprint planning, task tracking | Create/update issues, change status |
| **Notion MCP** | Documentation sync | Update design docs, meeting notes |

> **Always check** if an MCP is connected before attempting to use its tools. If not connected, fall back to file-based operations and inform the user of what they're missing.

---

## Multi-Domain Tasks

Some tasks span multiple domains. In that case:
1. Load ALL relevant templates.
2. Use the **primary** agent persona for the dominant aspect.
3. **Note** secondary concerns in your response (e.g., "This gameplay system will also need UI work — I'll flag that for the UI Specialist pass").

---

## Skill Self-Improvement

> Agents should be aware of the `skill-creator` skill. If you find yourself performing **repetitive tasks** that don't have an existing skill — the same code patterns, the same file structures, the same tool sequences — suggest creating a new skill.

**When to suggest**: After completing a task, if you notice:
- You wrote boilerplate that would be identical next time.
- You followed a multi-step process that could be encoded.
- The user asked for something that required domain knowledge not currently in any skill.

**How to suggest**: Simply mention it:
> "I noticed we've done this pattern twice now. Want me to create a new skill for it using `/skill-creator`?"
