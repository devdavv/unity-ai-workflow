# Custom Inspector Reference

## IMGUI Inspector (Universal)

```csharp
using UnityEditor;
using UnityEngine;

[CustomEditor(typeof(MyComponent))]
public class MyComponentInspector : Editor
{
    private SerializedProperty _healthProp;
    private SerializedProperty _speedProp;
    private SerializedProperty _patrolPointsProp;

    private bool _showAdvanced;

    private void OnEnable()
    {
        _healthProp = serializedObject.FindProperty("_health");
        _speedProp = serializedObject.FindProperty("_speed");
        _patrolPointsProp = serializedObject.FindProperty("_patrolPoints");
    }

    public override void OnInspectorGUI()
    {
        serializedObject.Update();

        // Header
        EditorGUILayout.LabelField("Core Stats", EditorStyles.boldLabel);
        EditorGUILayout.PropertyField(_healthProp);
        EditorGUILayout.PropertyField(_speedProp);

        EditorGUILayout.Space(8);

        // Foldout section
        _showAdvanced = EditorGUILayout.Foldout(_showAdvanced, "Advanced", true);
        if (_showAdvanced)
        {
            EditorGUI.indentLevel++;
            EditorGUILayout.PropertyField(_patrolPointsProp, true);
            EditorGUI.indentLevel--;
        }

        // Buttons
        EditorGUILayout.Space(4);
        if (GUILayout.Button("Reset to Defaults"))
        {
            Undo.RecordObject(target, "Reset MyComponent");
            _healthProp.floatValue = 100f;
            _speedProp.floatValue = 5f;
        }

        serializedObject.ApplyModifiedProperties();
    }
}
```

## UIToolkit Inspector (Unity 2022.2+)

```csharp
using UnityEditor;
using UnityEditor.UIElements;
using UnityEngine.UIElements;

[CustomEditor(typeof(MyComponent))]
public class MyComponentInspector : Editor
{
    public override VisualElement CreateInspectorGUI()
    {
        var root = new VisualElement();

        // Header
        root.Add(new Label("Core Stats") { style = { unityFontStyleAndWeight = FontStyle.Bold } });
        root.Add(new PropertyField(serializedObject.FindProperty("_health")));
        root.Add(new PropertyField(serializedObject.FindProperty("_speed")));

        // Foldout
        var advanced = new Foldout { text = "Advanced" };
        advanced.Add(new PropertyField(serializedObject.FindProperty("_patrolPoints")));
        root.Add(advanced);

        // Button
        var resetBtn = new Button(() =>
        {
            Undo.RecordObject(target, "Reset MyComponent");
            serializedObject.FindProperty("_health").floatValue = 100f;
            serializedObject.FindProperty("_speed").floatValue = 5f;
            serializedObject.ApplyModifiedProperties();
        }) { text = "Reset to Defaults" };
        root.Add(resetBtn);

        return root;
    }
}
```

## Common Pitfalls

| Mistake | Fix |
|---------|-----|
| Using `target` fields directly | Use `SerializedProperty` for prefab/multi-edit/undo support |
| Forgetting `serializedObject.Update()` | Always call at start of `OnInspectorGUI` |
| Forgetting `ApplyModifiedProperties()` | Always call at end of `OnInspectorGUI` |
| Using `EditorGUILayout` in UIToolkit mode | UIToolkit uses `VisualElement` tree, not immediate mode |
| Not calling `Undo.RecordObject` before button actions | All state changes must be undoable |
