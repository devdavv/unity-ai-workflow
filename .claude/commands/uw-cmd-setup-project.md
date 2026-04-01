# /uw-cmd-setup-project — Project Setup Workflow

Initialize a new Unity project with all infrastructure: folders, packages, git, and MCP connections.

## Skills
Load `uw-unity-project-setup` skill from `.claude/skills/uw-unity-project-setup/SKILL.md`.

## Steps

### 0. Select Dev Mode
Ask the user before anything else:

> "Which dev mode do you want for this project?
>
> - **Guided** (recommended default) — We collaborate. I suggest next steps and execute after you confirm key decisions.
> - **Autonomous** — Tell me your game idea and goals, I'll build the game with minimal interruptions. For best results, also enable Claude Code auto mode.
>
> You can change it later in `docs/ProjectConfig.yaml -> ai_mode`."

**If autonomous mode:** Ask ALL onboarding questions now (pitch, platforms, genre, references, features, packages, MCP availability). Set `ai_mode` in ProjectConfig.

### 1. Verify Environment
- Confirm Unity version matches ProjectConfig.
- Ask about MCP availability (Unity MCP, GitHub MCP).
- If install links needed, reference `docs/MCP_SETUP.md` — never guess URLs.
- Update `docs/ProjectConfig.yaml -> mcp` section.

### 2. Create Folder Structure
Use `uw-unity-project-setup` skill:
- Create `Assets/_Project/` with standard subfolders.
- Structure depends on `folder_strategy` from ProjectConfig.
- Create `Core.asmdef` in root scripts folder.

### 3. Install Packages
From `docs/ProjectConfig.yaml -> packages`:
- Via Unity MCP `manage_packages` if available.
- Otherwise, provide manifest.json entries for manual install.
- Install third-party from `docs/ProjectConfig.yaml -> third_party`.

### 4. Configure Editor Settings
Via Unity MCP or manual instructions:
- Render pipeline (URP/HDRP/Built-in)
- Color space: Linear
- Scripting backend: IL2CPP
- Target platform(s)

### 5. Generate Core Utilities
- **GameDebug.cs** — Using `uw-unity-debugging` skill (`.claude/skills/uw-unity-debugging/SKILL.md`)
- Add `ENABLE_LOGS` to Development build scripting defines

### 6. Git Setup
- Initialize repository (if not done)
- Create `.gitignore` (Unity template)
- Create `develop` branch from `main`
- Initial commit: `chore: project setup`

### 7. Documentation Setup
- Copy template files from `templates/` -> `docs/`
- Ensure GDD, TDD, GFD, PRD, SprintPlan are ready to be filled

### 8. Final Checklist
```
- Folder structure created
- Core.asmdef created
- Packages installed
- Editor settings configured
- GameDebug.cs generated
- Git repository initialized
- .gitignore created
- Documentation templates copied
- MCP connections verified
```

## Output
- Fully configured Unity project ready for Phase 4 (Production)
- Ready to start `/uw-cmd-plan` for the first sprint

$ARGUMENTS
