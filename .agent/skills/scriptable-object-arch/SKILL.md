---
name: ScriptableObject Architecture
description: Generate ScriptableObject-based systems including data containers, event channels, and runtime sets. Use when creating game data (weapons, enemies, items), cross-system events, or live object tracking. Triggers on requests like "create weapon data", "add an event system", "make a runtime set", "decouple these systems", or any SO-based architecture work. Read docs/TDD_Template.md for data architecture decisions.
---

# ScriptableObject Architecture

Generate SO patterns for data, events, and runtime collections.

## Before You Start
1. Read `docs/TDD_Template.md` for data architecture decisions.
2. Read `docs/NAMING_CONVENTIONS.md` for SO naming rules.

## Three Patterns

### 1. Data Container
```csharp
[CreateAssetMenu(fileName = "New WeaponData", menuName = "Game/Data/Weapon Data")]
public class WeaponData : ScriptableObject
{
    [SerializeField] private float _damage = 10f;
    public float Damage => _damage;  // Read-only property
}
```

### 2. Event Channel
```csharp
[CreateAssetMenu(fileName = "New GameEvent", menuName = "Game/Events/Game Event")]
public class GameEvent : ScriptableObject
{
    private event Action _onRaise;
    public void Raise() => _onRaise?.Invoke();
    public void Subscribe(Action listener) => _onRaise += listener;
    public void Unsubscribe(Action listener) => _onRaise -= listener;
}
```
For typed events, use `Action<T>` and create `IntEvent`, `FloatEvent`, etc.

### 3. Runtime Set
```csharp
[CreateAssetMenu(fileName = "New RuntimeSet", menuName = "Game/Sets/Runtime Set")]
public class RuntimeSet<T> : ScriptableObject
{
    private readonly List<T> _items = new();
    public IReadOnlyList<T> Items => _items;
    public void Add(T item) { if (!_items.Contains(item)) _items.Add(item); }
    public void Remove(T item) => _items.Remove(item);
    private void OnDisable() => _items.Clear(); // Prevent stale refs
}
```

## Naming: `{Type}Data`, `{Action}Event`, `{Type}Set`

## Rules
- Always use `[CreateAssetMenu]`.
- Always use `[SerializeField] private` + public read-only properties.
- Use `Action`/`Action<T>` delegates, not UnityEvents (better performance).
- Runtime sets **must** clear on `OnDisable()`.
