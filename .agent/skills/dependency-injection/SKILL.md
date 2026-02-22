---
name: Dependency Injection
description: Opt-in DI architecture using Reflex for Unity 6+. Covers container setup, scoping, injection, command pattern, update management, and class taxonomy. Use when ProjectConfig.yaml → architecture_pattern is "di-first", or when the user asks about dependency injection, inversion of control, or service architecture. Triggers on "set up DI", "add dependency injection", "create a service", "wire up dependencies", or architecture discussions involving testability and decoupling.
---

# Dependency Injection (Reflex)

DI-first architecture for complex Unity projects. **Only applies when `ProjectConfig.yaml → architecture_pattern: di-first`**. For simpler projects, use `scriptable-object-arch` (SO-first) instead.

## When to Use DI vs SO-First

| SO-First (default) | DI-First |
|---------------------|----------|
| Small/medium scope | Large/complex scope |
| Solo or small team | Team with testing culture |
| Rapid prototyping | Production architecture |
| Simple event-driven communication | Cross-feature orchestration via Commands |
| ScriptableObject event channels | Interface-based injection + factories |

Both can coexist: DI containers can bind ScriptableObject data assets as singletons.

## Reflex Setup

**Install** (OpenUPM): `openupm install com.gustavopsantos.reflex`
**Or** UPM Git URL: `https://github.com/gustavopsantos/reflex.git?path=/Assets/Reflex/`

### Container Hierarchy

```
RootScope (app lifetime — singletons, core services)
  └── SceneScope (scene lifetime — per-scene services, controllers)
        └── Manual child scopes (gameplay round, level, etc.)
```

### Installer Pattern

```csharp
using Reflex.Core;

public class CoreInstaller : MonoBehaviour, IInstaller
{
    [SerializeField] private AudioSettingsData _audioSettings;

    public void InstallBindings(ContainerBuilder builder) {
        // Services (singleton, app lifetime)
        builder.AddSingleton(typeof(IAudioService), typeof(AudioService));
        builder.AddSingleton(typeof(IStateMachineService), typeof(StateMachineService));

        // SO data (inject existing asset as value)
        builder.AddInstance(_audioSettings);
    }
}
```

### Binding Types

| Method | What It Does | Lifetime |
|--------|-------------|----------|
| `AddSingleton(type, impl)` | Container creates one instance | App/Scene |
| `AddTransient(type, impl)` | New instance each resolve | Per-resolve |
| `AddInstance(obj)` | Register existing object (SO, config) | Singleton |

### Injection

```csharp
using Reflex.Attributes;

public class ArrowController
{
    [Inject] private readonly IAudioService _audio;
    [Inject] private readonly IArrowMovementController _movement;

    // OR constructor injection (preferred for non-MonoBehaviours):
    public ArrowController(IAudioService audio, IArrowMovementController movement) {
        _audio = audio;
        _movement = movement;
    }
}
```

## App Lifecycle / Single Entry Point (SEP)

Only one scene has a `Start()` method. All other initialization flows from there.

```
CoreScene loads → RootScope binds → CoreInitiator.Start()
  → Load GameScene (additively) → SceneScope binds → GameInitiator.Init()
    → Load GamePlayScene → SceneScope binds → GamePlayInitiator.Init()
```

**Rules:**
- CoreScene is always the first scene (set in Build Settings index 0, or via editor DefaultSceneSelector)
- Each scene has one Installer (bindings) and one Initiator (entry/exit points)
- Only CoreInitiator has a `Start()` method — everything else is initialized via async `InitEntryPoint()`

## Class Responsibility Taxonomy

When using DI-first, code falls into 9 categories with strict reference rules:

| Class Type | Responsibility | Can Reference |
|------------|---------------|---------------|
| **Installer** | Binds interfaces to implementations in DI container | DI container only |
| **Initiator** | Scene entry/exit points. Starts async init chain | Services, Commands |
| **Command** | Cross-feature orchestration. Resolves own deps from container | Any Controller/Service (ONLY class allowed to cross feature boundaries) |
| **Controller** | Feature brain. Queries Data, updates View, invokes Commands | Own Data + View + Commands |
| **View** | Visual representation (MonoBehaviour). Never invokes logic | Inner Views only. Receives callbacks. |
| **Data** | Pure data (fields, properties, small conditionals). Called "Data" not "Model" to avoid 3D model confusion | Nothing |
| **Service** | Shared functionality with no View. Stateless or manages shared state | Other Services |
| **Factory** | Encapsulates object creation logic | DI container for deps |
| **Utils/Extensions** | Static helpers and extension methods | Nothing (static) |

**Key rule:** Commands are the ONLY classes allowed to reference other features' Controllers. This prevents circular dependencies — each Command resolves its own references from the DI container at execution time.

## Command Pattern

Commands replace events for cross-feature communication. Each resolves its own dependencies from the container to avoid circular references.

```csharp
public interface ICommand { void Execute(); }
public interface ICommandAsync { Awaitable Execute(CancellationToken ct); }
public interface ICommandWithResult<T> { T Execute(); }

public class ArrowCollisionCommand : ICommandAsync
{
    [Inject] private readonly IScoreController _score;
    [Inject] private readonly IAudioService _audio;
    [Inject] private readonly IFxController _fx;

    public async Awaitable Execute(CancellationToken ct) {
        _score.AddPoints(10);
        _audio.PlayOneShot(AudioClipType.Hit);
        await _fx.PlayBurstAsync(ct);
    }
}
```

Register in installer: `builder.AddTransient(typeof(ArrowCollisionCommand));`

## Update Management

Centralize Update/FixedUpdate/LateUpdate to avoid proliferating MonoBehaviour update methods:

```csharp
public interface IUpdatable { void OnUpdate(); }
public interface IFixedUpdatable { void OnFixedUpdate(); }
public interface ILateUpdatable { void OnLateUpdate(); }
```

One MonoBehaviour manages subscriptions:
```csharp
public class UpdateService : MonoBehaviour
{
    private readonly List<IUpdatable> _updatables = new();

    public void Subscribe(IUpdatable updatable) => _updatables.Add(updatable);
    public void Unsubscribe(IUpdatable updatable) => _updatables.Remove(updatable);

    private void Update() {
        for (int i = _updatables.Count - 1; i >= 0; i--)
            _updatables[i].OnUpdate();
    }
}
```

Controllers implement `IUpdatable` and subscribe/unsubscribe in their init/dispose.

## Interface-First Rule

Always bind to interfaces, not concrete types:
- Enables mock injection for unit tests
- Makes dependencies explicit and swappable
- Follows Dependency Inversion Principle

```csharp
// ✅ Bind to interface
builder.AddSingleton(typeof(IAudioService), typeof(AudioService));

// ❌ Don't bind concrete directly
builder.AddSingleton(typeof(AudioService));
```

## Integration with ScriptableObject Data

DI and SO work together — SO holds data, DI manages services and wiring:

```csharp
// In installer: inject SO data asset into container
[SerializeField] private BalloonsConfiguration _balloonsConfig;

public void InstallBindings(ContainerBuilder builder) {
    builder.AddInstance(_balloonsConfig); // SO data available via [Inject]
}
```
