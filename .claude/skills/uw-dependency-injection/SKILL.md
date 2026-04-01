---
name: uw-dependency-injection
description: Opt-in DI architecture using Reflex for Unity 6+. Covers container setup, scoping, injection, command pattern, update management, and class taxonomy. Use when ProjectConfig.yaml → architecture_pattern is "di-first", or when the user asks about dependency injection, inversion of control, or service architecture. Triggers on "set up DI", "add dependency injection", "create a service", "wire up dependencies", or architecture discussions involving testability and decoupling.
---

# Dependency Injection (Reflex)

DI-first architecture for complex Unity projects. **Only applies when `ProjectConfig.yaml → architecture_pattern: di-first`**. For simpler projects, use `uw-scriptable-object-arch` (SO-first) instead.

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

## Class Taxonomy, Commands & Update Management

See [references/class-taxonomy-and-commands.md](references/class-taxonomy-and-commands.md) for the full 9-class taxonomy, Command pattern, and centralized Update management.

**Key rule:** Commands are the ONLY classes allowed to cross feature boundaries.

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
