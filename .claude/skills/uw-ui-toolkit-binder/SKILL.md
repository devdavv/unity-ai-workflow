---
name: uw-ui-toolkit-binder
description: Generate Unity 6+ UI Toolkit Runtime Data Binding code using MVVM pattern with [CreateProperty], DataBinding, and PropertyPath. Use when creating UI screens, HUDs, menus, inventories, settings panels, or any data-bound UI elements with UI Toolkit. Triggers on requests like "create a health bar", "build the HUD", "make a settings screen", "bind data to UI", "create an inventory UI", "add a pause menu", "show player stats on screen", "build a shop UI", "create a dialogue box", or any task involving runtime UI layout, styling, or data display. Also use when the user mentions UXML, USS, ViewModel, or UI bindings. Only activates when ProjectConfig.yaml -> ui_system is "UIToolkit" or "Mixed". Requires Unity 6.0+.
---

# UI Toolkit Binder

Generate MVVM UI code using Unity 6's Runtime Data Binding.

## Before You Start

1. Read `docs/ProjectConfig.yaml` for:
   - `ui_system` — must be `"UIToolkit"` or `"Mixed"`. If `"UGUI"`, this skill does not apply.
   - `architecture_pattern` — `"so-first"` or `"di-first"` (affects how ViewModels get their data).
   - `mcp.unity_mcp` — if `true`, call `refresh_unity` after creating files.
2. Read `docs/NAMING_CONVENTIONS.md` — UXML files use PascalCase (e.g., `PlayerHUD.uxml`), USS files match (e.g., `PlayerHUD.uss`).
3. Read `docs/CODING_STANDARDS.md` — class structure order, serialization rules, async patterns apply to ViewModels.
4. If UI motion/animation is needed, check `docs/GFD_Template.md` for guidelines and use `uw-game-feel-integrator` for tween integration.

## When to Use UI Toolkit vs uGUI

| Use Case | Recommended | Why |
|----------|-------------|-----|
| Menus, settings, inventories, data-heavy UI | **UI Toolkit** | Batched rendering, no GameObject overhead, CSS-like styling |
| Animated in-game HUD, world-space UI | **uGUI** | Easier world-space Canvas, mature animation workflow |
| Complex project with both | **Hybrid** | UI Toolkit for menus, uGUI for in-game HUD |

If `ProjectConfig.yaml -> ui_system` is `"UGUI"`, stop here — use uGUI patterns instead.

## Architecture

```
Model (Data/SO) --> ViewModel ([CreateProperty] MonoBehaviour) --> View (UXML + USS)
```

- **Model**: Game data — ScriptableObjects, runtime state, network state. The ViewModel reads from these.
- **ViewModel**: A MonoBehaviour with `[CreateProperty]` on properties. This is the binding source. It bridges game data to UI elements without the View knowing about game logic.
- **View**: UXML defines layout, USS defines styling. Bound to ViewModel properties via `DataBinding` in a controller script.

## File Structure

For a feature called `PlayerHUD` inside a feature module:

```
Features/PlayerHUD/
├── Scripts/
│   ├── {RootNamespace}.PlayerHUD.asmdef
│   ├── PlayerHUDViewModel.cs
│   └── PlayerHUDController.cs
├── UI/
│   ├── PlayerHUD.uxml
│   └── PlayerHUD.uss
└── Tests/
    └── {RootNamespace}.PlayerHUD.Tests.asmdef
```

Use `uw-unity-feature-scaffold` to generate the module structure first, then add the UI-specific files.

## ViewModel Pattern

The ViewModel exposes bindable properties using `[CreateProperty]` from `Unity.Properties`. Properties must have **public getters** for the binding system to read them.

```csharp
using Unity.Properties;
using UnityEngine;

namespace {RootNamespace}.PlayerHUD
{
    public class PlayerHUDViewModel : MonoBehaviour
    {
        [SerializeField] private int _maxHealth = 100;

        private int _currentHealth;

        [CreateProperty]
        public int Health
        {
            get => _currentHealth;
            set
            {
                if (_currentHealth == value) return;
                _currentHealth = value;
            }
        }

        [CreateProperty]
        public float HealthPercent => _maxHealth > 0 ? (float)Health / _maxHealth : 0f;

        [CreateProperty]
        public string HealthText => $"{Health} / {_maxHealth}";

        [CreateProperty]
        public int CoinCount { get; private set; }

        /// <summary>
        /// Called by game systems to update health display.
        /// </summary>
        public void SetHealth(int current, int max)
        {
            _maxHealth = max;
            Health = current;
        }

        public void AddCoins(int amount)
        {
            CoinCount += amount;
        }
    }
}
```

When a `[CreateProperty]` value changes, the binding system detects it automatically on the next frame. No manual "notify" call is needed — Unity polls `[CreateProperty]` values each frame.

## UXML Template

UXML defines the layout structure. Think of it like HTML for Unity UI.

```xml
<ui:UXML xmlns:ui="UnityEngine.UIElements" xmlns:uie="UnityEditor.UIElements">
    <ui:VisualElement name="hud-root" class="hud-container">
        <!-- Health bar -->
        <ui:VisualElement name="health-section" class="stat-section">
            <ui:Label name="health-label" text="100 / 100" class="stat-text" />
            <ui:VisualElement name="health-bar-bg" class="bar-background">
                <ui:VisualElement name="health-bar-fill" class="bar-fill health-fill" />
            </ui:VisualElement>
        </ui:VisualElement>

        <!-- Coin counter -->
        <ui:VisualElement name="coin-section" class="stat-section">
            <ui:Label name="coin-label" text="0" class="stat-text" />
        </ui:VisualElement>
    </ui:VisualElement>
</ui:UXML>
```

**Naming**: Use kebab-case for UXML element `name` attributes (e.g., `health-label`, `coin-section`). This follows USS selector conventions and avoids conflicts with C# naming.

## USS Stylesheet

USS is Unity's CSS-like styling language. Define visual appearance here, not in C#.

```css
.hud-container {
    position: absolute;
    top: 16px;
    left: 16px;
    padding: 8px;
    flex-direction: column;
}

.stat-section {
    flex-direction: row;
    align-items: center;
    margin-bottom: 4px;
}

.stat-text {
    font-size: 18px;
    color: white;
    -unity-font-style: bold;
    min-width: 80px;
}

.bar-background {
    width: 200px;
    height: 16px;
    background-color: rgba(0, 0, 0, 0.5);
    border-radius: 4px;
    overflow: hidden;
}

.bar-fill {
    height: 100%;
    border-radius: 4px;
    transition-property: width;
    transition-duration: 0.3s;
}

.health-fill {
    background-color: rgb(76, 175, 80);
    width: 100%;
}
```

**Transitions**: USS supports `transition-property`, `transition-duration`, `transition-timing-function`, and `transition-delay` for smooth animated changes. Use these for bar fills, opacity fades, and position shifts instead of C# tweens where possible.

## Controller: Binding + Events

The controller loads the UXML, applies the USS, wires up data bindings, and handles UI events like button clicks.

```csharp
using UnityEngine;
using UnityEngine.UIElements;

namespace {RootNamespace}.PlayerHUD
{
    [RequireComponent(typeof(UIDocument))]
    public class PlayerHUDController : MonoBehaviour
    {
        [SerializeField] private PlayerHUDViewModel _viewModel;

        private UIDocument _document;
        private Label _healthLabel;
        private VisualElement _healthBarFill;
        private Label _coinLabel;

        private void Awake()
        {
            _document = GetComponent<UIDocument>();
        }

        private void OnEnable()
        {
            var root = _document.rootVisualElement;

            // Cache element references (equivalent of GetComponent caching)
            _healthLabel = root.Q<Label>("health-label");
            _healthBarFill = root.Q<VisualElement>("health-bar-fill");
            _coinLabel = root.Q<Label>("coin-label");

            // Data bindings
            _healthLabel.SetBinding("text", new DataBinding
            {
                dataSource = _viewModel,
                dataSourcePath = new PropertyPath(nameof(PlayerHUDViewModel.HealthText))
            });

            _coinLabel.SetBinding("text", new DataBinding
            {
                dataSource = _viewModel,
                dataSourcePath = new PropertyPath(nameof(PlayerHUDViewModel.CoinCount))
            });
        }

        private void Update()
        {
            // Update bar width from ViewModel — binding handles labels,
            // but bar width needs manual style update since it's a style property
            _healthBarFill.style.width = Length.Percent(_viewModel.HealthPercent * 100f);
        }
    }
}
```

**Why cache in OnEnable, not Awake?** The `UIDocument.rootVisualElement` may not be available in `Awake`. `OnEnable` is the earliest reliable point to query the visual tree.

## Event Binding (Buttons, Clicks)

For interactive elements like buttons, register callbacks in `OnEnable` and unregister in `OnDisable` to prevent memory leaks.

```csharp
private Button _pauseButton;
private Button _settingsButton;

private void OnEnable()
{
    var root = _document.rootVisualElement;

    _pauseButton = root.Q<Button>("pause-button");
    _settingsButton = root.Q<Button>("settings-button");

    _pauseButton.clicked += OnPauseClicked;
    _settingsButton.clicked += OnSettingsClicked;
}

private void OnDisable()
{
    _pauseButton.clicked -= OnPauseClicked;
    _settingsButton.clicked -= OnSettingsClicked;
}

private void OnPauseClicked()
{
    // Raise event or call game system
}

private void OnSettingsClicked()
{
    // Navigate to settings screen
}
```

For more complex interactions (hover, drag, custom input), use `RegisterCallback<T>`:

```csharp
// Pointer events
element.RegisterCallback<PointerEnterEvent>(OnHover);
element.RegisterCallback<PointerLeaveEvent>(OnHoverExit);

// Always unregister in OnDisable
element.UnregisterCallback<PointerEnterEvent>(OnHover);
```

## Dynamic Lists (Runtime-Generated Items)

For inventories, leaderboards, or any list populated at runtime, create items from a template UXML and add them to a parent container.

```csharp
[SerializeField] private VisualTreeAsset _itemTemplate;

private void PopulateInventory(List<ItemData> items)
{
    var container = _document.rootVisualElement.Q<VisualElement>("inventory-list");
    container.Clear();

    foreach (var item in items)
    {
        var element = _itemTemplate.Instantiate();
        element.Q<Label>("item-name").text = item.DisplayName;
        element.Q<Label>("item-count").text = item.Count.ToString();
        container.Add(element);
    }
}
```

For large lists (100+ items), use `ListView` with virtualization — it only renders visible items:

```csharp
var listView = root.Q<ListView>("inventory-list");
listView.itemsSource = _inventoryItems;
listView.makeItem = () => _itemTemplate.Instantiate();
listView.bindItem = (element, index) =>
{
    var item = _inventoryItems[index];
    element.Q<Label>("item-name").text = item.DisplayName;
};
```

## Value Formatting

When binding numeric data that needs formatting (percentages, currency, time), handle it in the ViewModel as a computed `[CreateProperty]` string:

```csharp
[CreateProperty]
public string DamageText => $"{Damage:F0}";

[CreateProperty]
public string TimerText
{
    get
    {
        int minutes = Mathf.FloorToInt(_remainingTime / 60f);
        int seconds = Mathf.FloorToInt(_remainingTime % 60f);
        return $"{minutes:D2}:{seconds:D2}";
    }
}

[CreateProperty]
public string PriceText => $"{Price:N0} G";
```

Keep formatting logic in the ViewModel, not the View layer. The View should only display strings.

## Performance Notes

- **Query once, cache always.** Call `root.Q<T>()` in `OnEnable` and store references. Never query inside `Update`.
- **USS transitions over C# tweens** for simple property changes (opacity, width, position). USS transitions are GPU-accelerated.
- **ListView for large lists.** Virtualization avoids creating hundreds of VisualElements. Use `ListView.RefreshItems()` to update, not `Clear()` + re-add.
- **Minimize style changes in Update.** Each `style.*` assignment can trigger layout recalculation. Batch changes or use USS class toggling (`AddToClassList`/`RemoveFromClassList`) instead.
- **Class toggling over direct style.** Prefer `element.EnableInClassList("low-health", healthPercent < 0.25f)` over setting individual style properties in C#.

## After Setup

- **Add game feel:** Use `uw-game-feel-integrator` for screen shake, flash effects, or tween-based UI animations that go beyond USS transitions.
- **Custom editor tools:** Use `uw-unity-editor-tools` if you need custom Inspector UI or Editor Windows built with UI Toolkit.
- **Write tests:** Use `uw-unity-test-runner` — test ViewModel logic (property calculations, state changes) in EditMode tests without needing UI rendering.

## Rules

- `[CreateProperty]` requires `Unity.Properties` namespace — always include the using.
- `DataBinding` and related types are in `UnityEngine.UIElements`.
- Always generate matching `.uxml` and `.uss` files alongside ViewModel C# scripts.
- UXML/USS file names use PascalCase (per `NAMING_CONVENTIONS.md`): `PlayerHUD.uxml`, `PlayerHUD.uss`.
- Cache all `root.Q<T>()` calls in `OnEnable` — never query the visual tree in `Update`.
- Register event callbacks in `OnEnable`, unregister in `OnDisable`.
- `[SerializeField] private` for Inspector-exposed fields on ViewModels and Controllers — never public fields.
- Keep formatting/conversion logic in the ViewModel. The View only displays strings and reacts to events.
- If `ProjectConfig.yaml -> mcp.unity_mcp` is `true`, call `refresh_unity` after creating files.
