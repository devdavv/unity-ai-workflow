---
name: uw-unity-project-setup
description: Full project initialization wizard from ProjectConfig, including folders, packages, Git, and MCP setup. Use when setting up a new Unity project from scratch, creating the folder structure, or bootstrapping infrastructure. Triggers on requests like "set up a new project", "create folder structure", "initialize the project", or "/uw-cmd-setup-project". Reads ProjectConfig.yaml for all decisions.
---

# Unity Project Setup

Orchestrate complete project initialization from `ProjectConfig.yaml`.

## Before You Start
1. Ensure `docs/ProjectConfig.yaml` is filled out.
2. Unity project must be created and open in Editor.
3. Check which MCP servers are available (Unity MCP for in-editor operations, GitHub for version control).

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
- **GameDebug.cs** via `uw-unity-debugging` skill
- Add `ENABLE_LOGS` define for Development builds

### 5. Git Setup
- Create `.gitignore` (Unity template)
- Create `develop` branch
- Initial commit
- Use GitHub MCP if available, otherwise git CLI

### 6. Configure Editor
- Render pipeline, color space (Linear), scripting backend (IL2CPP)

### 7. Report MCP Status
```
✅ Unity MCP — Connected
✅ GitHub MCP — Connected
⬜ Linear MCP — Not configured
```

## Asset Loading Strategy

| Approach | When to Use |
|----------|-------------|
| **Direct references** | Prefabs/SOs referenced in Inspector — always loads with scene |
| **Resources/** | Prototype only. Loads synchronously. Everything in Resources/ ships in build. |
| **Addressables** | Production default. Async loading, memory management, optional remote hosting. Install `com.unity.addressables`. |
| **AssetBundles** | Advanced/custom CDN pipelines. Addressables wraps this — prefer Addressables unless you need low-level control. |

For persistent data: encrypt PlayerPrefs values at minimum. For production, prefer server-side storage with authentication.

## Rules
- **Never** create folders inside `Assets/` without `_Project/` prefix.
- Use `uw-unity-feature-scaffold` for feature modules.
- Use `Resources/` sparingly — prefer Addressables for production.
