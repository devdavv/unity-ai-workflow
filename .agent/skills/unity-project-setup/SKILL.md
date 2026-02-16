---
name: Unity Project Setup
description: Full project initialization wizard from ProjectConfig, including folders, packages, Git, and MCP setup.
---

# Unity Project Setup

Orchestrates the complete setup of a new Unity project based on `ProjectConfig.yaml`. This is triggered by the `/setup-project` workflow.

## Prerequisites
- A filled-out `docs/ProjectConfig.yaml` (created during Phase 1 with the Architect agent).
- Unity project created and opened in the Editor.
- GitHub MCP connected (for repo/gitignore setup).
- Unity MCP connected (for in-editor operations).

## Execution Steps

### 1. Verify ProjectConfig
Read `docs/ProjectConfig.yaml` and confirm:
- Unity version matches the open editor
- Required packages are listed
- Folder strategy is set
- Networking choice is set

### 2. Create Folder Structure

Based on `folder_strategy`:

**Feature-Based:**
```
Assets/_Project/
├── Core/
│   ├── Scripts/
│   │   └── Core.asmdef
│   └── Data/
├── Features/
│   └── (created per-feature later)
├── Art/
│   ├── Materials/
│   ├── Models/
│   ├── Textures/
│   └── Animations/
├── Audio/
│   ├── Music/
│   └── SFX/
├── UI/
│   ├── UXML/
│   ├── USS/
│   └── Scripts/
├── Prefabs/
├── Scenes/
├── Resources/  (use sparingly)
└── Tests/
```

**Type-Based:**
```
Assets/_Project/
├── Scripts/
│   ├── Core/
│   │   └── Core.asmdef
│   ├── UI/
│   └── (feature folders)
├── Art/
├── Audio/
├── Prefabs/
├── Scenes/
└── Tests/
```

### 3. Install Packages
From `ProjectConfig.yaml → packages`, install each:
```
com.unity.inputsystem
com.unity.cinemachine
com.unity.textmeshpro
```
Via Unity MCP `manage_packages` or manual instructions.

### 4. Generate Core Files
- **GameDebug.cs** — Using `unity-debugging` skill
- **Core.asmdef** — Root assembly definition
- **.gitignore** — Unity-specific via GitHub MCP or manual

### 5. Git Setup (via GitHub MCP)
- Create repository (if not exists)
- Create `.gitignore` with Unity template
- Create `develop` branch
- Initial commit

### 6. Configure Unity Editor (via Unity MCP)
- Set render pipeline
- Set color space (Linear)
- Set scripting backend (IL2CPP)
- Configure quality settings for target platforms

### 7. MCP Verification
Check which MCPs are connected and report status:
```
✅ Unity MCP — Connected
✅ GitHub MCP — Connected
⬜ Linear MCP — Not configured
⬜ Notion MCP — Not configured
```

## Important Notes
- **Never** create folders inside `Assets/` without the `_Project/` prefix.
- Use the `unity-feature-scaffold` skill for creating feature modules.
- The `Resources/` folder should be used sparingly — prefer Addressables for production.
- Always confirm the folder structure with the user before creating.
