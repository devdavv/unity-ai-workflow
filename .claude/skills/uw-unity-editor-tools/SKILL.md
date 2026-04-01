---
name: uw-unity-editor-tools
description: Generate custom editor tooling including Custom Inspectors, PropertyDrawers, EditorWindows, Gizmos, and Handles. Use when building designer-facing tools, spatial visualization, batch operations, or custom component UIs. Triggers on requests like "create a custom inspector", "add gizmos", "build an editor window", "make this component designer-friendly", "add a debug visualization", "create a level editor tool", "add a property drawer", "make a batch rename tool", or any editor tooling work. Always uses SerializedProperty and Undo for safety.
---

# Unity Editor Tools

Generate editor tooling that follows Unity best practices.

## Before You Start

1. Read `docs/ProjectConfig.yaml` for:
   - `unity_version` — UI Toolkit editor UI requires 2022.2+ (all Unity 6+ projects qualify).
   - `ui_system` — if `"UIToolkit"` or `"Mixed"`, prefer UI Toolkit for editor UIs too (consistent approach).
   - `mcp.unity_mcp` — if `true`, call `refresh_unity` after creating files.
2. Check if the feature has an `Editor/` asmdef — if not, create one. The Editor asmdef must reference the Runtime asmdef and have `Editor` as its only Include Platform.
3. Read `docs/NAMING_CONVENTIONS.md` for editor script naming.

## Core Rules (Non-Negotiable)

```csharp
// ALWAYS: Use SerializedProperty for inspector fields
SerializedProperty _healthProp;
void OnEnable() => _healthProp = serializedObject.FindProperty("_health");

// NEVER: Direct field access (breaks multi-edit, prefabs, undo)
((MyComponent)target)._health = EditorGUILayout.FloatField(((MyComponent)target)._health);
```

```csharp
// ALWAYS: Record undo before modifications
Undo.RecordObject(target, "Changed patrol point");
myComponent.patrolPoints[i] = newPos;

// NEVER: Modify without undo
myComponent.patrolPoints[i] = newPos;
```

```csharp
// ALWAYS: Mark dirty after changes in tools
EditorUtility.SetDirty(target);

// ALWAYS: Apply modified properties in inspectors
serializedObject.ApplyModifiedProperties();
```

## Tool Selection

| Need | Tool | Reference |
|------|------|-----------|
| Custom component UI in Inspector | Custom Inspector | `references/custom-inspector.md` |
| Reusable field decoration (`[MinMax]`, `[ReadOnly]`) | PropertyDrawer | `references/property-drawer.md` |
| Standalone tool panel | EditorWindow | `references/editor-window.md` |
| Always-on spatial visualization | `OnDrawGizmos` / `OnDrawGizmosSelected` | Inline below |
| Interactive scene handles (draggable) | `Handles` API | Inline below |

## Gizmos (Quick Reference)

```csharp
// In a MonoBehaviour — no Editor assembly needed
private void OnDrawGizmosSelected()
{
    Gizmos.color = new Color(0f, 1f, 0f, 0.3f);
    Gizmos.DrawWireSphere(transform.position, _radius);
}
```

## Handles (Quick Reference)

```csharp
// In an Editor script — requires Editor assembly
[CustomEditor(typeof(PatrolPath))]
public class PatrolPathEditor : Editor
{
    private void OnSceneGUI()
    {
        var path = (PatrolPath)target;
        for (int i = 0; i < path.Points.Count; i++)
        {
            EditorGUI.BeginChangeCheck();
            Vector3 newPos = Handles.PositionHandle(path.Points[i], Quaternion.identity);
            if (EditorGUI.EndChangeCheck())
            {
                Undo.RecordObject(path, "Move patrol point");
                path.Points[i] = newPos;
                EditorUtility.SetDirty(path);
            }
        }
    }
}
```

## Editor Script Placement

```
Assets/_Project/Features/{Feature}/
├── Runtime/
│   ├── {Feature}.asmdef          <- Runtime assembly
│   └── MyComponent.cs
└── Editor/
    ├── {Feature}.Editor.asmdef   <- References Runtime assembly
    ├── MyComponentInspector.cs
    └── MyComponentGizmos.cs
```

The Editor asmdef must:
- Reference the Runtime asmdef
- Have `Editor` in its Include Platforms (only)
- Never be referenced by Runtime assemblies

## Build Validation

Add pre-build validators to catch issues before shipping. Place in an Editor assembly.

```csharp
using UnityEditor;
using UnityEditor.Build;
using UnityEditor.Build.Reporting;

public class PreBuildValidator : IPreprocessBuildWithReport
{
    public int callbackOrder => 0;

    public void OnPreprocessBuild(BuildReport report)
    {
        // Run unit tests — fail build if any test fails
        // Validate all scenes in Build Settings exist
        // Check for missing script references on prefabs
    }
}
```

Common validations: unit test pass/fail, scene list completeness, missing component references on prefabs, unresolved addressable entries.

## After Setup

- **Debug visualization:** Use `uw-unity-debugging` Gizmos for debug-specific visualizations (raycast paths, trigger volumes, AI sight cones).
- **Write tests:** Use `uw-unity-test-runner` — test editor tools in EditMode (validate that SerializedProperty operations produce correct results).
- **SO inspectors:** Use `uw-scriptable-object-arch` data containers with custom inspectors for designer-friendly data editing.
- **UI Toolkit binding:** Use `uw-ui-toolkit-binder` patterns if building editor windows with data binding.

## Principles

- **Undo is non-negotiable**: Any editor tool that modifies state without undo support is a bug. Always `Undo.RecordObject` before changes.
- **Designer empathy**: Tools exist to make non-programmers productive. A confusing inspector is a failed tool. Ask: is this for designers (intuitive, visual) or developers (can be technical)?
- **Build alongside features**: Don't wait for a "tooling phase." Build the inspector while the component is being created.

## Rules

- Always use `SerializedProperty` for inspector fields — never direct field access.
- Always record Undo before any state modification.
- Always `serializedObject.ApplyModifiedProperties()` at the end of inspector `OnInspectorGUI`.
- Editor scripts live in `Editor/` folders with their own `.asmdef` that includes only `Editor` platform.
- Never reference Editor assemblies from Runtime assemblies.
- `[SerializeField] private` for all Inspector fields — never public fields.
- If `ProjectConfig.yaml -> mcp.unity_mcp` is `true`, call `refresh_unity` after creating files.
