---
name: uw-dependency-injection
description: Opt-in DI architecture using Reflex for Unity 6+. Covers container setup, scoping, injection, command pattern, update management, and class taxonomy. Use when ProjectConfig.yaml -> architecture_pattern is "di-first", or when the user asks about dependency injection, inversion of control, service architecture, or testable game code. Triggers on "set up DI", "add dependency injection", "create a service", "wire up dependencies", "make this testable", "decouple these systems", "add a command pattern", "cross-feature communication", or architecture discussions involving testability, decoupling, and service layers. Only activates when ProjectConfig.yaml -> architecture_pattern is "di-first". For simpler projects, use uw-scriptable-object-arch instead.
---

# Dependency Injection (Reflex)

DI-first architecture for complex Unity projects. **Only applies when `ProjectConfig.yaml -> architecture_pattern: di-first`**. For simpler projects, use `uw-scriptable-object-arch` (SO-first) instead.

## Before You Start

1. Read `docs/ProjectConfig.yaml` for:
   - `architecture_pattern` — must be `"di-first"` for this skill to apply.
   - `di_framework` — should be `"reflex"` (the default and recommended framework).
   - `mcp.unity_mcp` — if `true`, call `refresh_unity` after creating files.
2. Read `docs/CODING_STANDARDS.md` for async patterns (`Awaitable` + `CancellationToken`), access modifiers (`internal` for cross-class helpers within an asmdef), and class structure.
3. Read `docs/NAMING_CONVENTIONS.md` for file/class naming.
4. Ensure each feature module has its own `.asmdef` — create with `uw-unity-feature-scaffold`. Installers, Controllers, Services all live inside feature asmdefs.

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
  +-- SceneScope (scene lifetime — per-scene services, controllers)
        +-- Manual child scopes (gameplay round, level, etc.)
```

### Installer Pattern

Each feature module has one Installer that registers its bindings. The Installer lives inside the feature's `.asmdef`.

```csharp
using Reflex.Core;
using UnityEngine;

namespace {RootNamespace}.Core
{
    public class CoreInstaller : MonoBehaviour, IInstaller
    {
        [SerializeField] private AudioSettingsData _audioSettings;

        public void InstallBindings(ContainerBuilder builder)
        {
            // Services (singleton, app lifetime)
            builder.AddSingleton(typeof(IAudioService), typeof(AudioService));
            builder.AddSingleton(typeof(IStateMachineService), typeof(StateMachineService));

            // SO data (inject existing asset as value)
            builder.AddInstance(_audioSettings);
        }
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

namespace {RootNamespace}.Combat
{
    public class ArrowController
    {
        [Inject] private readonly IAudioService _audio;
        [Inject] private readonly IArrowMovementController _movement;

        // OR constructor injection (preferred for non-MonoBehaviours):
        public ArrowController(IAudioService audio, IArrowMovementController movement)
        {
            _audio = audio;
            _movement = movement;
        }
    }
}
```

**Constructor injection** is preferred for plain C# classes — it makes dependencies explicit and prevents forgetting to inject. Use `[Inject]` attribute injection for MonoBehaviours (which Unity constructs).

## Interface-First Rule

Always bind to interfaces, not concrete types. This enables mock injection for testing, makes dependencies explicit, and follows the Dependency Inversion Principle.

```csharp
// Correct — bind to interface
builder.AddSingleton(typeof(IAudioService), typeof(AudioService));

// Wrong — binding concrete directly prevents mock injection
builder.AddSingleton(typeof(AudioService));
```

## Circular Dependency Prevention

Circular dependencies (A depends on B, B depends on A) cause infinite resolution loops. Prevent them with:

1. **Commands** — if two features need to communicate, use a Command that resolves its deps at execution time instead of at construction time.
2. **Events / ScriptableObject channels** — decouple with `uw-scriptable-object-arch` event channels.
3. **Interfaces in a shared assembly** — extract the interface into `{RootNamespace}.Core.asmdef` so both features depend on the interface, not on each other.

If the container throws a circular dependency error, trace the dependency chain and break it at the point where a Command or event channel makes more sense than a direct reference.

## App Lifecycle / Single Entry Point (SEP)

Only one scene has a `Start()` method. All other initialization flows from there.

```
CoreScene loads -> RootScope binds -> CoreInitiator.Start()
  -> Load GameScene (additively) -> SceneScope binds -> GameInitiator.Init()
    -> Load GamePlayScene -> SceneScope binds -> GamePlayInitiator.Init()
```

**Rules:**
- CoreScene is always the first scene (set in Build Settings index 0, or via editor DefaultSceneSelector).
- Each scene has one Installer (bindings) and one Initiator (entry/exit points).
- Only CoreInitiator has a `Start()` method — everything else is initialized via async `InitEntryPoint()`.

## Class Taxonomy, Commands & Update Management

See [references/class-taxonomy-and-commands.md](references/class-taxonomy-and-commands.md) for the full 9-class taxonomy, Command pattern, and centralized Update management.

**Key rule:** Commands are the ONLY classes allowed to cross feature boundaries. This prevents circular dependencies — each Command resolves its own references from the DI container at execution time.

### Cross-Feature Command Example

When Feature A (Combat) needs to notify Feature B (Score) about a hit:

```csharp
// Lives in a shared Commands assembly or in the feature that initiates it
public class OnHitScoredCommand : ICommand
{
    [Inject] private readonly IScoreController _score;
    [Inject] private readonly IAudioService _audio;

    public void Execute()
    {
        _score.AddPoints(10);
        _audio.PlayOneShot(AudioClipType.Hit);
    }
}
```

The Combat feature doesn't reference Score directly — it resolves and executes the Command, which the container wires up. Register in the installer: `builder.AddTransient(typeof(OnHitScoredCommand));`

## Integration with ScriptableObject Data

DI and SO work together — SO holds data, DI manages services and wiring:

```csharp
// In installer: inject SO data asset into container
[SerializeField] private WeaponDatabase _weaponDatabase;

public void InstallBindings(ContainerBuilder builder)
{
    builder.AddInstance(_weaponDatabase); // SO data available via [Inject]
}
```

See `uw-scriptable-object-arch` for data container and event channel patterns that complement DI.

## After Setup

- **Scaffold features:** Use `uw-unity-feature-scaffold` to create feature modules with their own `.asmdef` — each feature gets its own Installer.
- **State management:** Use `uw-state-machine` for game flow states, with states receiving dependencies via constructor injection.
- **Write tests:** Use `uw-unity-test-runner` — interface-based DI makes testing easy: construct classes with mock dependencies, no container needed in tests.
- **SO data:** Use `uw-scriptable-object-arch` for game data assets that get injected into services via `AddInstance`.

## Rules

- Only applies when `ProjectConfig.yaml -> architecture_pattern: di-first`.
- Always bind to interfaces, not concrete types (interface-first rule).
- Constructor injection preferred for plain C# classes. `[Inject]` attribute for MonoBehaviours.
- Commands are the ONLY classes allowed to cross feature boundaries.
- Each feature module needs its own `.asmdef` (per `NAMING_CONVENTIONS.md`). Installers live inside the feature's asmdef.
- `[SerializeField] private` for Inspector-exposed fields on Installers and MonoBehaviours — never public fields.
- Use `Awaitable` with `CancellationToken` for async operations (per `CODING_STANDARDS.md`).
- CoreScene is Build Settings index 0. Only CoreInitiator has `Start()`.
- Break circular dependencies with Commands, events, or shared interface assemblies — never with direct cross-feature references.
- If `ProjectConfig.yaml -> mcp.unity_mcp` is `true`, call `refresh_unity` after creating files.
