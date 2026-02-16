---
name: Unity Feature Scaffold
description: Creates a complete feature folder structure with Assembly Definition files based on the project's folder strategy.
---

# Unity Feature Scaffold

Creates a new feature module with proper folder structure, Assembly Definition (`.asmdef`), and namespace based on the project's `ProjectConfig.yaml → folder_strategy`.

## Prerequisites
- Read `docs/ProjectConfig.yaml` for `folder_strategy` (feature-based or type-based).
- Read `docs/NAMING_CONVENTIONS.md` for folder and namespace naming rules.

## Execution Steps

### 1. Determine Folder Strategy

**Feature-Based** (`folder_strategy: "feature-based"`):
```
Assets/_Project/Features/{FeatureName}/
├── Scripts/
│   ├── {FeatureName}.asmdef
│   ├── Runtime/
│   └── Editor/  (if needed)
├── Prefabs/
├── UI/          (if needed)
├── Data/        (ScriptableObjects)
└── Tests/
    ├── {FeatureName}.Tests.asmdef
    └── EditMode/
```

**Type-Based** (`folder_strategy: "type-based"`):
```
Assets/_Project/
├── Scripts/{FeatureName}/
│   └── {FeatureName}.asmdef
├── Prefabs/{FeatureName}/
├── UI/{FeatureName}/
├── Data/{FeatureName}/
└── Tests/{FeatureName}/
    └── {FeatureName}.Tests.asmdef
```

### 2. Generate Assembly Definition

```json
{
    "name": "{ProjectNamespace}.{FeatureName}",
    "rootNamespace": "{ProjectNamespace}.{FeatureName}",
    "references": [
        "{ProjectNamespace}.Core"
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

### 3. Generate Test Assembly Definition

```json
{
    "name": "{ProjectNamespace}.{FeatureName}.Tests",
    "rootNamespace": "{ProjectNamespace}.{FeatureName}.Tests",
    "references": [
        "{ProjectNamespace}.{FeatureName}",
        "UnityEngine.TestRunner",
        "UnityEditor.TestRunner"
    ],
    "includePlatforms": ["Editor"],
    "excludePlatforms": [],
    "allowUnsafeCode": false,
    "overrideReferences": true,
    "precompiledReferences": ["nunit.framework.dll"],
    "autoReferenced": false,
    "defineConstraints": ["UNITY_INCLUDE_TESTS"],
    "versionDefines": [],
    "noEngineReferences": false
}
```

### 4. Create Initial Files
- A placeholder script with the correct namespace.
- A placeholder test file with a passing `[Test]` method.

## MCP Integration
- If Unity MCP is available, use `manage_asset` to create folders and verify the asmdef is recognized.
- Use `refresh_unity` after creating files so Unity picks up the new assembly.

## Important Notes
- **Never** create scripts outside of an assembly definition.
- The `_Project` prefix prevents conflicts with Asset Store packages.
- Always ask the user for the feature name and confirm the namespace before creating.
