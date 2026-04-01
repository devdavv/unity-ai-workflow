# PropertyDrawer Reference

## Anatomy

A PropertyDrawer has two parts:
1. **PropertyAttribute** — The decorator users put on fields (`[MinMax]`, `[ReadOnly]`)
2. **PropertyDrawer** — The editor class that renders the custom UI

## IMGUI PropertyDrawer

```csharp
// --- Attribute (Runtime assembly) ---
using UnityEngine;

public class MinMaxAttribute : PropertyAttribute
{
    public float Min { get; }
    public float Max { get; }

    public MinMaxAttribute(float min, float max)
    {
        Min = min;
        Max = max;
    }
}

// --- Usage (Runtime assembly) ---
[MinMax(0f, 100f)]
[SerializeField] private Vector2 _healthRange = new(20f, 80f);
```

```csharp
// --- Drawer (Editor assembly) ---
using UnityEditor;
using UnityEngine;

[CustomPropertyDrawer(typeof(MinMaxAttribute))]
public class MinMaxDrawer : PropertyDrawer
{
    public override void OnGUI(Rect position, SerializedProperty property, GUIContent label)
    {
        var attr = (MinMaxAttribute)attribute;

        if (property.propertyType != SerializedPropertyType.Vector2)
        {
            EditorGUI.LabelField(position, label.text, "Use MinMax with Vector2");
            return;
        }

        Vector2 range = property.vector2Value;
        float min = range.x;
        float max = range.y;

        EditorGUI.BeginProperty(position, label, property);

        position = EditorGUI.PrefixLabel(position, label);

        // Min field
        var minRect = new Rect(position.x, position.y, 50f, position.height);
        // Slider
        var sliderRect = new Rect(position.x + 55f, position.y, position.width - 110f, position.height);
        // Max field
        var maxRect = new Rect(position.xMax - 50f, position.y, 50f, position.height);

        min = EditorGUI.FloatField(minRect, min);
        EditorGUI.MinMaxSlider(sliderRect, ref min, ref max, attr.Min, attr.Max);
        max = EditorGUI.FloatField(maxRect, max);

        property.vector2Value = new Vector2(min, max);

        EditorGUI.EndProperty();
    }
}
```

## UIToolkit PropertyDrawer (Unity 2022.2+)

```csharp
[CustomPropertyDrawer(typeof(MinMaxAttribute))]
public class MinMaxDrawer : PropertyDrawer
{
    public override VisualElement CreatePropertyGUI(SerializedProperty property)
    {
        var attr = (MinMaxAttribute)attribute;
        var container = new VisualElement { style = { flexDirection = FlexDirection.Row } };

        var minField = new FloatField("Min") { bindingPath = $"{property.propertyPath}.x" };
        var maxField = new FloatField("Max") { bindingPath = $"{property.propertyPath}.y" };
        var slider = new MinMaxSlider(attr.Min, attr.Max, attr.Min, attr.Max);

        container.Add(new Label(property.displayName));
        container.Add(minField);
        container.Add(slider);
        container.Add(maxField);

        return container;
    }
}
```

## Useful Drawer Ideas

| Attribute | Purpose |
|-----------|---------|
| `[ReadOnly]` | Display field but disable editing |
| `[MinMax(min, max)]` | Dual-handle range slider |
| `[SceneRef]` | Dropdown of scenes in build settings |
| `[TagSelector]` | Dropdown of project tags |
| `[LayerSelector]` | Dropdown of project layers |
| `[Conditional("_boolField")]` | Show/hide field based on another field |
| `[InlineEditor]` | Show SO fields inline instead of object picker |

## Key Rules
- Attribute class goes in **Runtime** assembly (it decorates serialized fields).
- Drawer class goes in **Editor** assembly.
- Always wrap in `EditorGUI.BeginProperty` / `EndProperty` for proper prefab override styling.
- Always handle unexpected property types gracefully (log warning, don't crash).
