---
name: Unity Editor Tools
description: Generate custom editor tooling including Custom Inspectors, PropertyDrawers, EditorWindows, Gizmos, and Handles. Use when building designer-facing tools, spatial visualization, batch operations, or custom component UIs. Triggers on requests like "create a custom inspector", "add gizmos", "build an editor window", "make this component designer-friendly", or any editor tooling work. Works with the Tool Developer agent. Always uses SerializedProperty and Undo.
---

# Unity Editor Tools

Generate editor tooling that follows Unity best practices.

## Before Generating

1. Read `ProjectConfig.yaml → unity_version` — UIToolkit editor UI requires 2022.2+.
2. Check if the feature has an `Editor/` asmdef — if not, create one using `unity-feature-scaffold`.

## Core Rules (Non-Negotiable)

```csharp
// ✅ ALWAYS: Use SerializedProperty for inspector fields
SerializedProperty _healthProp;
void OnEnable() => _healthProp = serializedObject.FindProperty("_health");

// ❌ NEVER: Direct field access (breaks multi-edit, prefabs, undo)
((MyComponent)target)._health = EditorGUILayout.FloatField(((MyComponent)target)._health);
```

```csharp
// ✅ ALWAYS: Record undo before modifications
Undo.RecordObject(target, "Changed patrol point");
myComponent.patrolPoints[i] = newPos;

// ❌ NEVER: Modify without undo
myComponent.patrolPoints[i] = newPos;
```

```csharp
// ✅ ALWAYS: Mark dirty after changes in tools
EditorUtility.SetDirty(target);

// ✅ ALWAYS: Apply modified properties in inspectors
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
│   ├── {Feature}.asmdef          ← Runtime assembly
│   └── MyComponent.cs
└── Editor/
    ├── {Feature}.Editor.asmdef   ← References Runtime assembly
    ├── MyComponentInspector.cs
    └── MyComponentGizmos.cs
```

The Editor asmdef must:
- Reference the Runtime asmdef
- Have `Editor` in its Include Platforms (only)
- Never be referenced by Runtime assemblies
