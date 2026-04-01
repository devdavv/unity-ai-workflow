---
name: uw-scriptable-object-arch
description: Generate ScriptableObject-based systems including data containers, event channels, and runtime sets. Use when creating game data (weapons, enemies, items, abilities), cross-system events, or live object tracking. Triggers on requests like "create weapon data", "add an event system", "make a runtime set", "decouple these systems", "create item database", "add a damage event", "track active enemies", "create a config SO", "add game settings data", or any task involving ScriptableObject data containers, event-driven communication, or runtime object tracking. Reads ProjectConfig.yaml -> architecture_pattern to determine SO-first vs DI-first context.
---

# ScriptableObject Architecture

Generate SO patterns for data, events, and runtime collections.

## Before You Start

1. Read `docs/ProjectConfig.yaml` for:
   - `architecture_pattern` — `"so-first"` (default, this skill is primary) or `"di-first"` (SOs still hold data, but events become Commands — see `uw-dependency-injection`).
   - `mcp.unity_mcp` — if `true`, call `refresh_unity` after creating files.
2. Read `docs/TDD_Template.md` for data architecture decisions.
3. Read `docs/NAMING_CONVENTIONS.md` for SO file naming (PascalCase: `SwordData.asset`).
4. Ensure the feature has an `.asmdef` — create with `uw-unity-feature-scaffold` if needed.

## Three Patterns

### 1. Data Container

Store game-tunable values in a ScriptableObject. Designers edit them in the Inspector without touching code.

```csharp
using UnityEngine;

[CreateAssetMenu(fileName = "New WeaponData", menuName = "Game/Data/Weapon Data")]
public class WeaponData : ScriptableObject
{
    [Header("Combat")]
    [SerializeField] private float _damage = 10f;
    [SerializeField] private float _fireRate = 0.5f;
    [SerializeField] private float _range = 15f;

    [Header("Ammo")]
    [SerializeField] private int _maxAmmo = 30;
    [SerializeField] private float _reloadTime = 1.5f;

    [Header("Feel")]
    [SerializeField] private ShakeProfile _hitShakeProfile;
    [SerializeField] private AudioClip _fireSound;
    [SerializeField] private GameObject _muzzleFlashPrefab;

    // Read-only properties — game code reads, designers tune in Inspector
    public float Damage => _damage;
    public float FireRate => _fireRate;
    public float Range => _range;
    public int MaxAmmo => _maxAmmo;
    public float ReloadTime => _reloadTime;
    public ShakeProfile HitShakeProfile => _hitShakeProfile;
    public AudioClip FireSound => _fireSound;
    public GameObject MuzzleFlashPrefab => _muzzleFlashPrefab;
}
```

### 2. Event Channel

Decouple systems by broadcasting events through a ScriptableObject asset. Any system can subscribe without knowing who raises the event.

**Parameterless event:**
```csharp
using System;
using UnityEngine;

[CreateAssetMenu(fileName = "New GameEvent", menuName = "Game/Events/Game Event")]
public class GameEvent : ScriptableObject
{
    private event Action _onRaise;

    public void Raise() => _onRaise?.Invoke();
    public void Subscribe(Action listener) => _onRaise += listener;
    public void Unsubscribe(Action listener) => _onRaise -= listener;
}
```

**Typed event channels** for events that carry data:
```csharp
using System;
using UnityEngine;

[CreateAssetMenu(fileName = "New IntEvent", menuName = "Game/Events/Int Event")]
public class IntEvent : ScriptableObject
{
    private event Action<int> _onRaise;

    public void Raise(int value) => _onRaise?.Invoke(value);
    public void Subscribe(Action<int> listener) => _onRaise += listener;
    public void Unsubscribe(Action<int> listener) => _onRaise -= listener;
}

[CreateAssetMenu(fileName = "New FloatEvent", menuName = "Game/Events/Float Event")]
public class FloatEvent : ScriptableObject
{
    private event Action<float> _onRaise;

    public void Raise(float value) => _onRaise?.Invoke(value);
    public void Subscribe(Action<float> listener) => _onRaise += listener;
    public void Unsubscribe(Action<float> listener) => _onRaise -= listener;
}
```

**Usage:**
```csharp
// Publisher (doesn't know who listens)
[SerializeField] private IntEvent _onScoreChanged;
_onScoreChanged.Raise(newScore);

// Subscriber (doesn't know who publishes)
[SerializeField] private IntEvent _onScoreChanged;
private void OnEnable() => _onScoreChanged.Subscribe(UpdateScoreUI);
private void OnDisable() => _onScoreChanged.Unsubscribe(UpdateScoreUI);
```

### 3. Runtime Set

Track live objects (active enemies, collectibles, spawned projectiles) without `FindObjectsOfType`. Objects register themselves on spawn and unregister on destroy.

```csharp
using System.Collections.Generic;
using UnityEngine;

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

    private void OnDisable() => _items.Clear(); // Prevent stale refs across play sessions
}
```

**Concrete sets** (needed because Unity can't serialize open generics):
```csharp
[CreateAssetMenu(fileName = "New EnemySet", menuName = "Game/Sets/Enemy Set")]
public class EnemyRuntimeSet : RuntimeSet<EnemyController> { }
```

**Self-registering pattern:**
```csharp
public class EnemyController : MonoBehaviour
{
    [SerializeField] private EnemyRuntimeSet _activeEnemies;

    private void OnEnable() => _activeEnemies.Add(this);
    private void OnDisable() => _activeEnemies.Remove(this);
}
```

## Naming Convention

| Pattern | Naming | Example |
|---------|--------|---------|
| Data Container | `{Type}Data` | `WeaponData`, `EnemyData` |
| Event Channel | `{Action}Event` | `OnDeathEvent`, `ScoreChangedEvent` |
| Typed Event | `{Type}Event` | `IntEvent`, `FloatEvent`, `StringEvent` |
| Runtime Set | `{Type}Set` or `{Type}RuntimeSet` | `EnemySet`, `CollectibleRuntimeSet` |

## After Setup

- **Add game feel:** Use `uw-game-feel-integrator` — SO data containers pair well with feel parameters (ShakeProfile, tween durations stored in the SO).
- **Write tests:** Use `uw-unity-test-runner` — test event channels (subscribe, raise, verify callback) and runtime sets (add, remove, clear) in EditMode tests.
- **Custom inspectors:** Use `uw-unity-editor-tools` for designer-friendly SO inspectors (preview panels, validation).
- **Graduate to DI:** If the project outgrows SO events, use `uw-dependency-injection` — SO data stays, events become Commands.

## When to Consider DI Instead

If your project needs any of these, check `ProjectConfig.yaml -> architecture_pattern` and consider switching to `di-first` with the `uw-dependency-injection` skill:
- Cross-feature orchestration beyond simple events (Command pattern)
- Constructor injection for testability with mock services
- Scoped lifetimes (per-scene, per-level services)
- Factory patterns with automatic dependency resolution

**DI + SO coexist**: In DI-first projects, ScriptableObjects still hold data — they're injected into the container as instances. Events are replaced by Commands.

## Rules

- Always use `[CreateAssetMenu]` on every ScriptableObject.
- Always use `[SerializeField] private` + public read-only properties for data fields.
- Use `Action`/`Action<T>` delegates for events, not UnityEvents (better performance, no boxing).
- Runtime sets must clear on `OnDisable()` to prevent stale references across play sessions.
- Always unsubscribe from events in `OnDisable` — matching every `Subscribe` with an `Unsubscribe`.
- All SO code must live inside an `.asmdef`.
- SO file names use PascalCase (per `NAMING_CONVENTIONS.md`): `SwordData.asset`.
- If `ProjectConfig.yaml -> mcp.unity_mcp` is `true`, call `refresh_unity` after creating files.
