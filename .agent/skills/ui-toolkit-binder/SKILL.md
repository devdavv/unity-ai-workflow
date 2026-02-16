---
name: UI Toolkit Binder
description: Generates Unity 6.2 UI Toolkit Runtime Data Binding code with MVVM pattern.
---

# UI Toolkit Binder

Generates C# code for Unity 6.2's **Runtime Data Binding** system using `[CreateProperty]`, `DataBinding`, and `PropertyPath`. Only activates when `ProjectConfig.yaml → ui_system` is `"UIToolkit"` or `"Mixed"`.

## Prerequisites
- Verify `docs/ProjectConfig.yaml → ui_system` includes UI Toolkit.
- Verify Unity version is 6.0+ (Runtime Binding requires Unity 6).
- Read `docs/GFD_Template.md` for UI motion guidelines if applicable.

## Architecture Pattern: MVVM

```
┌─────────┐     ┌───────────┐     ┌─────────┐
│  Model   │────▶│ ViewModel │◀────│  View   │
│ (Data)   │     │ (Binding) │     │ (UXML)  │
└─────────┘     └───────────┘     └─────────┘
```

## Code Generation Templates

### ViewModel with `[CreateProperty]`

```csharp
using Unity.Properties;
using UnityEngine;

public class PlayerHUDViewModel : MonoBehaviour
{
    [CreateProperty]
    public int Health { get; private set; }

    [CreateProperty]
    public int MaxHealth { get; private set; }

    [CreateProperty]
    public float HealthPercent => MaxHealth > 0 ? (float)Health / MaxHealth : 0f;

    [CreateProperty]
    public string HealthText => $"{Health} / {MaxHealth}";

    public void UpdateHealth(int current, int max)
    {
        Health = current;
        MaxHealth = max;
    }
}
```

### Binding Setup in C#

```csharp
using UnityEngine;
using UnityEngine.UIElements;

public class PlayerHUDController : MonoBehaviour
{
    [SerializeField] private UIDocument _uiDocument;
    [SerializeField] private PlayerHUDViewModel _viewModel;

    private void OnEnable()
    {
        var root = _uiDocument.rootVisualElement;

        // Bind the label to the ViewModel
        var healthLabel = root.Q<Label>("health-label");
        healthLabel.SetBinding("text", new DataBinding
        {
            dataSource = _viewModel,
            dataSourcePath = new PropertyPath(nameof(PlayerHUDViewModel.HealthText))
        });

        // Bind a progress bar
        var healthBar = root.Q<ProgressBar>("health-bar");
        healthBar.SetBinding("value", new DataBinding
        {
            dataSource = _viewModel,
            dataSourcePath = new PropertyPath(nameof(PlayerHUDViewModel.HealthPercent))
        });
    }
}
```

### UXML Template

```xml
<ui:UXML xmlns:ui="UnityEngine.UIElements">
    <ui:VisualElement class="hud-container">
        <ui:Label name="health-label" class="health-text" />
        <ui:ProgressBar name="health-bar" class="health-bar" />
    </ui:VisualElement>
</ui:UXML>
```

### USS Stylesheet

```css
.hud-container {
    flex-direction: row;
    padding: 10px;
}

.health-text {
    font-size: 24px;
    color: white;
    -unity-font-style: bold;
}

.health-bar {
    width: 200px;
    height: 20px;
}
```

## MCP Integration
- Use Unity MCP `manage_script` to create ViewModel and Controller scripts.
- Use Unity MCP `manage_asset` to create UXML and USS files.
- Use Unity MCP `manage_components` to attach UI Document and ViewModel to GameObjects.

## Important Notes
- `[CreateProperty]` requires the `Unity.Properties` namespace.
- Properties must have **public getters** for binding to work.
- The `DataBinding` class is in `UnityEngine.UIElements`.
- Always verify the Unity version supports Runtime Binding before generating this code.
