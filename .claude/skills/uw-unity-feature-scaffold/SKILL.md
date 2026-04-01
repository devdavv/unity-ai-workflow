---
name: uw-unity-feature-scaffold
description: Create a complete feature module with folder structure, Assembly Definition (.asmdef), test assembly, and namespace. Use when adding a new feature, system, or module to a Unity project. Triggers on requests like "create a new feature", "scaffold a module", "add a combat system", "I need a health system", "set up inventory", "create a new module for X", "add a new system", "new feature folder", or any task requiring new folders, assemblies, or namespaces for a gameplay feature — even when the user just names a system they want to build from scratch. Reads ProjectConfig.yaml for folder_strategy and MCP availability.
---

# Unity Feature Scaffold

Generate a feature module with folders, assembly definitions, and a starter class based on `docs/ProjectConfig.yaml`.

## Before You Start

1. Read `docs/ProjectConfig.yaml` for:
   - `project_name` — derive root namespace by converting to PascalCase (e.g., `"my cool game"` → `MyCoolGame`)
   - `folder_strategy` — `"feature-based"` or `"type-based"`
   - `architecture_pattern` — `"so-first"` or `"di-first"` (informs post-scaffold next steps)
   - `mcp.unity_mcp` — if `true`, call `refresh_unity` after creating files
2. Ask the user for the **feature name** (PascalCase, e.g., `Combat`, `PlayerMovement`, `Inventory`).
3. Derive the namespace: `{RootNamespace}.{FeatureName}` (e.g., `MyCoolGame.Combat`).
4. Confirm the namespace with the user before generating files.

## Folder Structures

**Feature-based** (`folder_strategy: "feature-based"`):
```
Assets/_Project/Features/{FeatureName}/
├── Scripts/
│   ├── {RootNamespace}.{FeatureName}.asmdef
│   └── {FeatureName}Manager.cs
├── Data/
└── Tests/
    └── {RootNamespace}.{FeatureName}.Tests.asmdef
```

**Type-based** (`folder_strategy: "type-based"`):
```
Assets/_Project/Scripts/{FeatureName}/
├── {RootNamespace}.{FeatureName}.asmdef
└── {FeatureName}Manager.cs
Assets/_Project/Tests/{FeatureName}/
└── {RootNamespace}.{FeatureName}.Tests.asmdef
```

## Assembly Definition Templates

### Feature asmdef

File: `{RootNamespace}.{FeatureName}.asmdef`

```json
{
    "name": "{RootNamespace}.{FeatureName}",
    "rootNamespace": "{RootNamespace}.{FeatureName}",
    "references": [
        "GUID:{CoreAsmdefGUID}"
    ],
    "includePlatforms": [],
    "excludePlatforms": [],
    "allowUnsafeCode": false,
    "overrideReferences": false,
    "precompiledReferences": [],
    "autoReferenced": true,
    "defineConstraints": [],
    "versionDefines": [],
    "noEngineReferences": false
}
```

### Test asmdef

File: `{RootNamespace}.{FeatureName}.Tests.asmdef`

```json
{
    "name": "{RootNamespace}.{FeatureName}.Tests",
    "rootNamespace": "{RootNamespace}.{FeatureName}.Tests",
    "references": [
        "GUID:{FeatureAsmdefGUID}",
        "UnityEngine.TestRunner",
        "UnityEditor.TestRunner"
    ],
    "includePlatforms": [
        "Editor"
    ],
    "excludePlatforms": [],
    "allowUnsafeCode": false,
    "overrideReferences": true,
    "precompiledReferences": [
        "nunit.framework.dll"
    ],
    "autoReferenced": false,
    "defineConstraints": [
        "UNITY_INCLUDE_TESTS"
    ],
    "versionDefines": [],
    "noEngineReferences": false
}
```

**GUID references:** Read the GUID from the target `.asmdef.meta` file (the `guid` field on line 2). Use `GUID:` prefix for robustness against renames. Fall back to the string name (e.g., `"{RootNamespace}.Core"`) if the `.meta` file isn't accessible.

## Starter File

File: `{FeatureName}Manager.cs`

```csharp
using UnityEngine;

namespace {RootNamespace}.{FeatureName}
{
    public class {FeatureName}Manager : MonoBehaviour
    {
    }
}
```

This is a minimal entry point. The actual class name and base type should be adjusted to fit the feature — not every feature needs a MonoBehaviour manager.

## Cross-Feature References

When Feature B depends on Feature A, add Feature A's asmdef GUID to Feature B's `references` array. Read the GUID from Feature A's `.asmdef.meta` file — never modify `.meta` files directly.

Keep dependencies **one-directional**. If two features need to communicate bidirectionally, extract shared interfaces or event channels into `{RootNamespace}.Core` (see `uw-scriptable-object-arch` for event channel patterns).

## After Scaffolding

- **Write tests:** Use `uw-unity-test-runner` — the test asmdef is already configured.
- **Add architecture patterns:** Use `uw-scriptable-object-arch` (SO-first) or `uw-dependency-injection` (DI-first) based on `ProjectConfig.yaml → architecture_pattern`.
- **Create editor tools:** Use `uw-unity-editor-tools` for custom inspectors or editor windows alongside the feature.

## Rules

- All scripts must live inside an `.asmdef`.
- All paths start with `Assets/_Project/` to avoid Asset Store conflicts.
- Never create, modify, or delete `.meta` files — Unity generates them automatically.
- Asmdef file names use dot notation: `{RootNamespace}.{FeatureName}.asmdef` (per `NAMING_CONVENTIONS.md`).
- If `ProjectConfig.yaml → mcp.unity_mcp` is `true`, call `refresh_unity` after creating files. Otherwise, instruct the user to return to Unity Editor for auto-refresh.
