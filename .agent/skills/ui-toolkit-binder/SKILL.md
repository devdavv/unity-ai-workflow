---
name: UI Toolkit Binder
description: Generate Unity 6+ UI Toolkit Runtime Data Binding code using MVVM pattern with [CreateProperty], DataBinding, and PropertyPath. Use when creating UI screens, HUDs, menus, or any data-bound UI elements with UI Toolkit. Triggers on requests like "create a health bar", "build the HUD", "bind data to UI", or any UI Toolkit work. Only activates when ProjectConfig.yaml → ui_system is "UIToolkit" or "Mixed". Requires Unity 6.0+.
---

# UI Toolkit Binder

Generate MVVM UI code using Unity 6's Runtime Data Binding.

## Before You Start
1. Verify `docs/ProjectConfig.yaml → ui_system` includes UIToolkit.
2. Check `docs/GFD_Template.md` for UI motion guidelines if applicable.

## Architecture

```
Model (Data) → ViewModel ([CreateProperty] MonoBehaviour) → View (UXML + USS)
```

## Key Pattern: ViewModel with Binding

```csharp
using Unity.Properties;
using UnityEngine;

public class PlayerHUDViewModel : MonoBehaviour
{
    [CreateProperty]
    public int Health { get; private set; }

    [CreateProperty]
    public string HealthText => $"{Health} / {MaxHealth}";
}
```

Bind in the controller:
```csharp
var label = root.Q<Label>("health-label");
label.SetBinding("text", new DataBinding
{
    dataSource = _viewModel,
    dataSourcePath = new PropertyPath(nameof(PlayerHUDViewModel.HealthText))
});
```

## Critical Notes
- `[CreateProperty]` requires `Unity.Properties` namespace.
- Properties must have **public getters** for binding.
- `DataBinding` is in `UnityEngine.UIElements`.
- Always generate matching `.uxml` and `.uss` files alongside C# scripts.
