---
name: ScriptableObject Architecture
description: Generates ScriptableObject data containers, event channels, and runtime sets following best practices.
---

# ScriptableObject Architecture

Generates ScriptableObject-based systems for data management, event channels, and runtime collections.

## Prerequisites
- Read `docs/TDD_Template.md` for data architecture decisions.
- Read `docs/NAMING_CONVENTIONS.md` for SO naming conventions.

## Patterns

### 1. Data Container

```csharp
using UnityEngine;

[CreateAssetMenu(fileName = "New WeaponData", menuName = "Game/Data/Weapon Data")]
public class WeaponData : ScriptableObject
{
    [Header("Identity")]
    [SerializeField] private string _weaponName;
    [SerializeField] private Sprite _icon;

    [Header("Stats")]
    [SerializeField] private float _damage = 10f;
    [SerializeField] private float _fireRate = 0.5f;
    [SerializeField] private float _range = 50f;
    [SerializeField] private int _maxAmmo = 30;

    // Public read-only properties
    public string WeaponName => _weaponName;
    public Sprite Icon => _icon;
    public float Damage => _damage;
    public float FireRate => _fireRate;
    public float Range => _range;
    public int MaxAmmo => _maxAmmo;
}
```

### 2. Event Channel (Parameterless)

```csharp
using UnityEngine;
using System;

[CreateAssetMenu(fileName = "New GameEvent", menuName = "Game/Events/Game Event")]
public class GameEvent : ScriptableObject
{
    private event Action _onRaise;

    public void Raise() => _onRaise?.Invoke();
    public void Subscribe(Action listener) => _onRaise += listener;
    public void Unsubscribe(Action listener) => _onRaise -= listener;
}
```

### 3. Event Channel (Typed)

```csharp
using UnityEngine;
using System;

[CreateAssetMenu(fileName = "New IntEvent", menuName = "Game/Events/Int Event")]
public class IntEvent : ScriptableObject
{
    private event Action<int> _onRaise;

    public void Raise(int value) => _onRaise?.Invoke(value);
    public void Subscribe(Action<int> listener) => _onRaise += listener;
    public void Unsubscribe(Action<int> listener) => _onRaise -= listener;
}
```

### 4. Runtime Set

```csharp
using UnityEngine;
using System.Collections.Generic;

[CreateAssetMenu(fileName = "New RuntimeSet", menuName = "Game/Sets/Runtime Set")]
public class RuntimeSet<T> : ScriptableObject
{
    private readonly List<T> _items = new();

    public IReadOnlyList<T> Items => _items;
    public int Count => _items.Count;

    public void Add(T item)
    {
        if (!_items.Contains(item))
            _items.Add(item);
    }

    public void Remove(T item) => _items.Remove(item);

    private void OnDisable() => _items.Clear();
}
```

## Naming Conventions

| Type | Prefix | Example |
|------|--------|---------|
| Data Container | `{Type}Data` | `WeaponData`, `EnemyData` |
| Event Channel | `{Action}Event` | `PlayerDeathEvent`, `ScoreChangedEvent` |
| Runtime Set | `{Type}Set` | `ActiveEnemySet`, `CollectibleSet` |

## Important Notes
- Always use `[CreateAssetMenu]` — never create SOs only through code.
- Use `[SerializeField] private` with public read-only properties.
- Event channels should use `Action`/`Action<T>` delegates, not UnityEvents (better performance).
- Runtime sets must clear on `OnDisable()` to prevent stale references between play sessions.

## MCP Integration
- Use Unity MCP `manage_scriptable_object` to create SO instances.
- Use Unity MCP `manage_asset` to place SO assets in the correct `Data/` folder.
