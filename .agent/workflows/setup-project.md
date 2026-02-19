---
description: Full project initialization wizard for new Unity projects.
---

# /setup-project — Project Setup Workflow

Initialize a new Unity project with all infrastructure: folders, packages, git, and MCP connections.

## Agent
**Architect**

## Steps

### 0. Select Dev Mode
Ask the user before anything else:

> "Which dev mode do you want for this project?
>
> - **Assistant** — You build the game. I assist with documentation, recommendations, and explanations. I never proceed without your explicit direction.
> - **Mix** (recommended default) — We collaborate. I suggest next steps and execute after you confirm key decisions.
> - **Automatic** — Tell me your game idea and goals, I'll build the game with minimal interruptions.
>
> There's no wrong answer — you can change it later in `ProjectConfig.yaml → ai_mode`."

**If automatic mode is selected**, ask ALL of the following now so the AI can proceed independently:
1. One-sentence game pitch + core loop description
2. Target platform(s) and genre
3. 1-3 reference games
4. Rough feature list (can expand during production)
5. Preferred packages (tweening library, networking, etc.)
6. MCP availability (Unity, GitHub, Linear, Notion)

Set `ai_mode` in ProjectConfig.yaml based on the answer.

---

## Prerequisites
- Unity project created and open in Editor.
- `docs/ProjectConfig.yaml` filled out (done during Phase 1 with Architect agent).

### 1. Verify Environment
- Confirm Unity version matches ProjectConfig.
- **Ask the user** which tools they have available:
  - **Unity MCP** — "Do you have Unity MCP installed? This lets me inspect/modify scenes, packages, and run tests directly."
  - **GitHub MCP** — "Do you have GitHub MCP connected? This lets me create repos, branches, and commits."
  - **Linear/Notion** — "Are you using Linear, Notion, or Jira for task management? If so, do you have the MCP connected?"
- If the user needs install links, reference `docs/MCP_SETUP.md` — **never guess URLs**.
- Update `ProjectConfig.yaml → mcp` section based on answers.
- Report status to user.

### 2. Create Folder Structure
Use `unity-project-setup` skill:
- Create `Assets/_Project/` with all standard subfolders.
- Structure depends on `folder_strategy` from ProjectConfig.
- Create `Core.asmdef` in the root scripts folder.

### 3. Install Packages
From `ProjectConfig.yaml → packages`:
- Via Unity MCP `manage_packages` if available.
- Otherwise, provide manifest.json entries for manual install.
- **Recommend essential packages** based on project type (e.g., 2D Sprite, TextMeshPro).
- **Install third-party** from `ProjectConfig.yaml → third_party` (e.g., DOTween).

### 4. Configure Editor Settings
Via Unity MCP or manual instructions:
- Set render pipeline (URP/HDRP/Built-in)
- Set color space to Linear
- Set scripting backend to IL2CPP
- Configure target platform(s)

### 5. Generate Core Utilities
- **GameDebug.cs** — Using `unity-debugging` skill
- Add `ENABLE_LOGS` to Development build scripting defines

### 6. Git Setup
Via GitHub MCP:
- Initialize repository (if not done)
- Create `.gitignore` (Unity template)
- Create `develop` branch from `main`
- Set branch protection rules (user does this — admin)
- Initial commit: "chore: project setup"

### 7. Documentation Setup
- Copy template files from `templates/` → `docs/`
- Ensure GDD, TDD, GFD, SprintPlan are ready to be filled

### 8. Final Checklist
```
✅ Folder structure created
✅ Core.asmdef created
✅ Packages installed
✅ Editor settings configured
✅ GameDebug.cs generated
✅ Git repository initialized
✅ .gitignore created
✅ Documentation templates copied
✅ MCP connections verified
```

## Output
- Fully configured Unity project ready for Phase 4 (Production)
- All infrastructure in place
- Ready to start `/plan` for the first sprint
