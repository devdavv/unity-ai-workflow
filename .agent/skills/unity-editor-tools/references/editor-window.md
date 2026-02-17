# EditorWindow Reference

## IMGUI EditorWindow (Universal)

```csharp
using UnityEditor;
using UnityEngine;

public class LevelValidatorWindow : EditorWindow
{
    private Vector2 _scrollPos;
    private string _searchFilter = "";

    [MenuItem("Tools/Level Validator")]
    public static void ShowWindow()
    {
        var window = GetWindow<LevelValidatorWindow>("Level Validator");
        window.minSize = new Vector2(300, 200);
    }

    private void OnEnable()
    {
        // Initialize data, subscribe to events
    }

    private void OnDisable()
    {
        // Cleanup, unsubscribe from events
    }

    private void OnGUI()
    {
        // Toolbar
        EditorGUILayout.BeginHorizontal(EditorStyles.toolbar);
        _searchFilter = EditorGUILayout.TextField(_searchFilter, EditorStyles.toolbarSearchField);
        if (GUILayout.Button("Validate All", EditorStyles.toolbarButton, GUILayout.Width(80)))
        {
            ValidateAll();
        }
        EditorGUILayout.EndHorizontal();

        // Scrollable content
        _scrollPos = EditorGUILayout.BeginScrollView(_scrollPos);

        // ... render your content here ...

        EditorGUILayout.EndScrollView();

        // Status bar
        EditorGUILayout.LabelField("Status: Ready", EditorStyles.miniLabel);
    }

    private void ValidateAll()
    {
        // Validation logic
    }
}
```

## UIToolkit EditorWindow (Unity 2022.2+)

```csharp
using UnityEditor;
using UnityEngine;
using UnityEngine.UIElements;

public class LevelValidatorWindow : EditorWindow
{
    [MenuItem("Tools/Level Validator")]
    public static void ShowWindow()
    {
        var window = GetWindow<LevelValidatorWindow>("Level Validator");
        window.minSize = new Vector2(300, 200);
    }

    // Use CreateGUI instead of OnEnable for UIToolkit
    public void CreateGUI()
    {
        var root = rootVisualElement;

        // Optional: load a UXML template
        // var tree = AssetDatabase.LoadAssetAtPath<VisualTreeAsset>("path/to/window.uxml");
        // tree.CloneTree(root);

        // Toolbar
        var toolbar = new Toolbar();
        var searchField = new ToolbarSearchField();
        searchField.RegisterValueChangedCallback(evt => FilterResults(evt.newValue));
        toolbar.Add(searchField);

        var validateBtn = new ToolbarButton(() => ValidateAll()) { text = "Validate All" };
        toolbar.Add(validateBtn);
        root.Add(toolbar);

        // Scrollable content
        var scrollView = new ScrollView();
        scrollView.Add(new Label("Results will appear here"));
        root.Add(scrollView);
    }

    private void FilterResults(string query) { /* ... */ }
    private void ValidateAll() { /* ... */ }
}
```

## Lifecycle

| Method | When Called | Use For |
|--------|-----------|---------|
| `OnEnable()` | Window opens or domain reload | IMGUI data init, subscriptions |
| `CreateGUI()` | After `OnEnable`, once | UIToolkit tree building |
| `OnGUI()` | Every frame (IMGUI only) | Immediate mode rendering |
| `OnDisable()` | Window closes or domain reload | Cleanup, unsubscribe |
| `OnFocus()` | Window gets focus | Refresh data |
| `OnLostFocus()` | Window loses focus | Save state |
| `OnSelectionChange()` | Editor selection changes | Update to show selected object |

## Common Patterns

### Dockable Tab
```csharp
// Dock next to Inspector by default
var window = GetWindow<MyWindow>(typeof(InspectorWindow));
```

### Persistent Data (survives domain reload)
```csharp
// Use ScriptableSingleton for persistent window state
[FilePath("ProjectSettings/MyTool.asset", FilePathAttribute.Location.ProjectFolder)]
public class MyToolSettings : ScriptableSingleton<MyToolSettings>
{
    public string lastSearchQuery;
    public void Save() => Save(true);
}
```

### Progress Bar for Long Operations
```csharp
EditorUtility.DisplayProgressBar("Validating", $"Checking {name}...", progress);
// ... work ...
EditorUtility.ClearProgressBar();
```

## Key Rules
- Use `CreateGUI` for UIToolkit, `OnGUI` for IMGUI — never mix.
- Always set `minSize` to prevent layout breaking.
- Never do heavy work in `OnGUI` — it runs every frame.
- Use `EditorApplication.delayCall` to defer work after domain reload.
