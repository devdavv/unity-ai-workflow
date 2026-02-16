---
name: Unity Project Setup
description: Full project initialization wizard from ProjectConfig, including folders, packages, Git, and MCP setup. Use when setting up a new Unity project from scratch, creating the folder structure, or bootstrapping infrastructure. Triggers on requests like "set up a new project", "create folder structure", "initialize the project", or "/setup-project". Reads ProjectConfig.yaml for all decisions.
---

# Unity Project Setup

Orchestrate complete project initialization from `ProjectConfig.yaml`.

## Before You Start
1. Ensure `docs/ProjectConfig.yaml` is filled out.
2. Unity project must be created and open in Editor.
3. Check which MCPs are connected.

## Steps (in order)

### 1. Verify ProjectConfig
Confirm Unity version, packages, folder strategy, networking choice.

### 2. Create Folder Structure
Based on `folder_strategy`, create `Assets/_Project/` with standard subfolders:
- `Core/Scripts/` + `Core.asmdef`
- `Features/` or `Scripts/` (depending on strategy)
- `Art/`, `Audio/`, `UI/`, `Prefabs/`, `Scenes/`, `Tests/`

### 3. Install Packages
From `ProjectConfig.yaml → packages`, install via Unity MCP `manage_packages` or provide `manifest.json` entries.

### 4. Generate Core Utilities
- **GameDebug.cs** via `unity-debugging` skill
- Add `ENABLE_LOGS` define for Development builds

### 5. Git Setup (via GitHub MCP)
- Create `.gitignore` (Unity template)
- Create `develop` branch
- Initial commit

### 6. Configure Editor (via Unity MCP or manual)
- Render pipeline, color space (Linear), scripting backend (IL2CPP)

### 7. Report MCP Status
```
✅ Unity MCP — Connected
✅ GitHub MCP — Connected
⬜ Linear MCP — Not configured
```

## Rules
- **Never** create folders inside `Assets/` without `_Project/` prefix.
- Use `unity-feature-scaffold` for feature modules.
- Use `Resources/` sparingly — prefer Addressables.
