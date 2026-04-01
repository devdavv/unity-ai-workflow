---
name: uw-state-machine
description: Interface-based state machine patterns for Unity game states, character controllers, and UI flows. Use when implementing game flow (menu -> gameplay -> game over), character states (idle -> run -> jump -> fall), AI behavior states, or UI screen transitions. Triggers on "state machine", "game states", "character states", "screen flow", "game flow", "add states to", "idle/run/jump", "menu flow", "AI states", "boss phases", or any task involving managing discrete states with transitions between them. Reads ProjectConfig.yaml -> architecture_pattern to adapt patterns for SO-first or DI-first.
---

# State Machine

Interface-based state machines for game flow, characters, and UI.

## Before You Start

1. Read `docs/ProjectConfig.yaml` for:
   - `architecture_pattern` — `"so-first"` or `"di-first"` (affects how states and the StateMachine are created and wired).
   - `mcp.unity_mcp` — if `true`, call `refresh_unity` after creating files.
2. Read `docs/CODING_STANDARDS.md` for async patterns (`Awaitable` + `CancellationToken`) — state transitions are async.
3. Ensure the feature has an `.asmdef` — create with `uw-unity-feature-scaffold` if needed.

## Core Interface

```csharp
public interface IState
{
    Awaitable Enter(CancellationToken ct);
    Awaitable Exit(CancellationToken ct);
}
```

For states that need per-frame updates (character movement, AI ticking), extend with:

```csharp
public interface IUpdatableState : IState
{
    void Update();
}
```

## State Machine Service

Only one state is active at a time. Transitions are async to support loading screens, animations, and cleanup between states.

```csharp
public class StateMachine
{
    private IState _currentState;

    public IState CurrentState => _currentState;

    public async Awaitable TransitionTo(IState newState, CancellationToken ct)
    {
        if (_currentState != null)
            await _currentState.Exit(ct);

        _currentState = newState;
        await _currentState.Enter(ct);
    }
}
```

If the current state implements `IUpdatableState`, the owner (a MonoBehaviour or update service) should call `Update()` each frame:

```csharp
private void Update()
{
    if (_stateMachine.CurrentState is IUpdatableState updatable)
        updatable.Update();
}
```

## CancellationToken Lifecycle

States receive a `CancellationToken` in `Enter` and `Exit`. This token controls the lifetime of async work within that state — when the token cancels, any in-flight awaits should stop cleanly.

**Where tokens come from:**

```csharp
// Per-scene token — cancels when the scene unloads
private CancellationTokenSource _sceneCts;

private void OnEnable()
{
    _sceneCts = CancellationTokenSource.CreateLinkedTokenSource(
        Application.exitCancellationToken);
}

private void OnDisable()
{
    _sceneCts?.Cancel();
    _sceneCts?.Dispose();
}
```

**Pass the token through transitions:**
```csharp
await _stateMachine.TransitionTo(new GamePlayState(_services), _sceneCts.Token);
```

**Inside a state, use the token for all async work:**
```csharp
public class LoadingState : IState
{
    public async Awaitable Enter(CancellationToken ct)
    {
        await SceneManager.LoadSceneAsync("Level1", LoadSceneMode.Additive)
            .WithCancellation(ct);
        await LoadAssetsAsync(ct);
    }

    public async Awaitable Exit(CancellationToken ct)
    {
        // Cleanup — this also receives ct in case exit itself is async
    }
}
```

## Common Implementations

### Game Flow States

```
LoadingState -> LobbyState -> GamePlayState -> GameOverState
                   ^                              |
                   +------------------------------+
```

Each state manages its own scene loading, UI, and services:

```csharp
public class GamePlayState : IState
{
    private readonly IScoreService _score;
    private readonly IAudioService _audio;

    public GamePlayState(IScoreService score, IAudioService audio)
    {
        _score = score;
        _audio = audio;
    }

    public async Awaitable Enter(CancellationToken ct)
    {
        await SceneManager.LoadSceneAsync("GamePlay", LoadSceneMode.Additive)
            .WithCancellation(ct);
        _score.Reset();
        _audio.PlayMusic(MusicTrack.Gameplay);
    }

    public async Awaitable Exit(CancellationToken ct)
    {
        _audio.StopMusic();
        await SceneManager.UnloadSceneAsync("GamePlay")
            .WithCancellation(ct);
    }
}
```

### Character States

```
IdleState <-> RunState -> JumpState -> FallState
    ^           ^           |           |
    +-----------+-----------+-----------+
```

Character states typically need per-frame updates and transition conditions:

```csharp
public class CharacterIdleState : IUpdatableState
{
    private readonly CharacterController _controller;
    private readonly StateMachine _fsm;
    private readonly CancellationToken _ct;

    public CharacterIdleState(CharacterController controller, StateMachine fsm, CancellationToken ct)
    {
        _controller = controller;
        _fsm = fsm;
        _ct = ct;
    }

    public async Awaitable Enter(CancellationToken ct)
    {
        _controller.PlayAnimation("Idle");
    }

    public void Update()
    {
        if (_controller.MoveInput.magnitude > 0.1f)
        {
            _fsm.TransitionTo(
                new CharacterRunState(_controller, _fsm, _ct), _ct).Forget();
        }
    }

    public async Awaitable Exit(CancellationToken ct) { }
}
```

### UI Screen Flow

```
MainMenuScreen -> SettingsScreen -> ConfirmDialog
      ^                               |
      +-------------------------------+
```

Use for screen stacks where each screen manages its own visual state. Each screen state shows/hides its UI root and handles input.

## Conditional Transitions

For states with multiple exit paths, check conditions in `Update()` and transition to the appropriate next state:

```csharp
public void Update()
{
    if (_controller.IsGrounded && _controller.JumpPressed)
    {
        _fsm.TransitionTo(new JumpState(_controller, _fsm, _ct), _ct).Forget();
        return; // Only one transition per frame
    }

    if (!_controller.IsGrounded)
    {
        _fsm.TransitionTo(new FallState(_controller, _fsm, _ct), _ct).Forget();
        return;
    }
}
```

**Only transition once per frame** — check the most important condition first and `return` after starting a transition. Multiple transitions in the same frame cause race conditions.

## DI Integration (di-first)

When `architecture_pattern: "di-first"`, register the StateMachine and use a factory to create states with injected dependencies:

```csharp
// In installer
public void InstallBindings(ContainerBuilder builder)
{
    builder.AddSingleton(typeof(StateMachine));
    builder.AddTransient(typeof(GamePlayState));
    builder.AddTransient(typeof(GameOverState));
}
```

States receive their dependencies via constructor injection — the container resolves them automatically:

```csharp
public class GamePlayState : IState
{
    [Inject] private readonly IScoreService _score;
    [Inject] private readonly IAudioService _audio;

    // Container creates this with all deps resolved
}
```

See `uw-dependency-injection` for container setup, installer patterns, and the class taxonomy.

## SO-First Integration (so-first)

When `architecture_pattern: "so-first"`, states can be MonoBehaviours with serialized transition references, or plain classes created in a MonoBehaviour controller:

```csharp
public class GameFlowController : MonoBehaviour
{
    [SerializeField] private GameEvent _onGameStart;
    [SerializeField] private GameEvent _onGameOver;

    private StateMachine _fsm;

    private void Awake()
    {
        _fsm = new StateMachine();
    }

    private async void Start()
    {
        var ct = destroyCancellationToken;
        await _fsm.TransitionTo(new LobbyState(_onGameStart), ct);
    }
}
```

Use `uw-scriptable-object-arch` event channels for communication between states and other systems.

## After Setup

- **Write tests:** Use `uw-unity-test-runner` — test state transitions in EditMode by asserting `CurrentState` type after `TransitionTo`.
- **Add game feel:** Use `uw-game-feel-integrator` for transition animations, screen effects between states.
- **Debug state issues:** Use `uw-unity-debugging` if states aren't entering/exiting as expected.

## Rules

- One active state at a time per state machine.
- Always pass `CancellationToken` to `Enter` and `Exit` — all async work inside states must respect cancellation.
- Link CancellationTokens to `Application.exitCancellationToken` or scene lifetime.
- States should not reference each other directly — transitions go through the StateMachine.
- Clean up subscriptions, event listeners, and spawned objects in `Exit()`.
- Only one transition per frame — check conditions with early `return` after starting a transition.
- All state machine code must live inside an `.asmdef`.
- Use `[SerializeField] private` for any Inspector-exposed fields on MonoBehaviour owners.
- If `ProjectConfig.yaml -> mcp.unity_mcp` is `true`, call `refresh_unity` after creating files.
