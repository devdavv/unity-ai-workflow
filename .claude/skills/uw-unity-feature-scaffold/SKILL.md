---
name: uw-unity-feature-scaffold
description: Create a complete feature module with folder structure, Assembly Definition (.asmdef), test assembly, and namespace. Use when adding a new feature, system, or module to a Unity project. Triggers on requests like "create a new feature", "scaffold a module", "add a combat system", or any task requiring a new assembly/namespace. Reads ProjectConfig.yaml for folder_strategy (feature-based or type-based).
---

# Unity Feature Scaffold

Generate a feature module based on `docs/ProjectConfig.yaml → folder_strategy`.

## Before You Start
1. Read `docs/ProjectConfig.yaml` for `folder_strategy` and project namespace.
2. Ask the user for the **feature name** and confirm the namespace.

## Folder Structures

**Feature-based** (`folder_strategy: "feature-based"`):
```
Assets/_Project/Features/{FeatureName}/
├── Scripts/
│   ├── {FeatureName}.asmdef
│   └── Runtime/
├── Data/
└── Tests/
    └── {FeatureName}.Tests.asmdef
```

**Type-based** (`folder_strategy: "type-based"`):
```
Assets/_Project/Scripts/{FeatureName}/
└── {FeatureName}.asmdef
Assets/_Project/Tests/{FeatureName}/
└── {FeatureName}.Tests.asmdef
```

## Assembly Definition

For the feature asmdef, set:
- `name` and `rootNamespace` to `{ProjectNamespace}.{FeatureName}`
- Reference `{ProjectNamespace}.Core`
- `autoReferenced: true`, `allowUnsafeCode: false`

For the test asmdef, set:
- `name` to `{ProjectNamespace}.{FeatureName}.Tests`
- Reference the feature asmdef + `UnityEngine.TestRunner` + `UnityEditor.TestRunner`
- `includePlatforms: ["Editor"]`, `defineConstraints: ["UNITY_INCLUDE_TESTS"]`
- `overrideReferences: true`, `precompiledReferences: ["nunit.framework.dll"]`

## Object Creation Patterns

| Pattern | When to Use |
|---------|-------------|
| **Simple Factory** | One class creates instances of another. No DI needed. |
| **SO Factory** | ScriptableObject holds prefab ref + config. Inspector-friendly. |
| **DI Factory** | Container resolves dependencies during creation. DI-first only. |

```csharp
// Simple Factory (SO-first or DI-first)
public class ProjectileFactory
{
    [SerializeField] private GameObject _prefab;
    public GameObject Create(Vector3 position) =>
        Object.Instantiate(_prefab, position, Quaternion.identity);
}
```

For DI-first projects, factories register with the container to auto-inject dependencies into spawned objects. See `uw-dependency-injection` skill.

## Prefab Workflow
Prefer prefab-based workflows over scene-embedded objects to avoid scene merge conflicts in team settings. Assemble scenes from prefab instances.

## Rules
- **Never** create scripts outside of an assembly definition.
- Always prefix with `Assets/_Project/` to avoid Asset Store conflicts.
- Use Unity MCP `refresh_unity` after creating files.
