# Phase 3: Project Setup

> **Agent**: Architect
> **Workflow**: `/setup-project`
> **Skill**: `unity-project-setup`

## Goal
Create the Unity project skeleton with all infrastructure in place.

## Activities

### 1. Folder Structure
Create `Assets/_Project/` with all subfolders based on `folder_strategy`.

### 2. Assembly Definitions
Create `.asmdef` files matching the TDD's assembly graph.

### 3. Package Installation
Install all packages from `ProjectConfig.yaml → packages`.

### 4. Core Utilities
- Generate `GameDebug.cs` wrapper (via `unity-debugging` skill)
- Add `ENABLE_LOGS` to Development scripting defines
- Create core ScriptableObject base classes

### 5. Git Configuration
Via GitHub MCP:
- Create `.gitignore` (Unity template)
- Create `develop` branch
- Set up branch strategy
- Initial commit

### 6. Editor Configuration
Via Unity MCP or manual:
- Render pipeline setup
- Quality settings per platform
- Color space (Linear)
- Physics settings

### 7. Documentation
- Copy templates → `docs/` folder
- Set up sprint tracking

## Entry Criteria
- Phase 2 complete (TDD approved)
- Unity project created and open

## Exit Criteria
- [ ] Folder structure created
- [ ] All asmdefs created and compiling
- [ ] All packages installed
- [ ] GameDebug.cs generated
- [ ] Git initialized with .gitignore
- [ ] Editor settings configured
- [ ] Documentation templates in place
