---
name: uw-state-machine
description: Interface-based state machine patterns for Unity game states, character controllers, and UI flows. Use when implementing game flow (menu → gameplay → game over), character states (idle → run → jump → fall), AI behavior states, or UI screen transitions. Triggers on "state machine", "game states", "character states", "screen flow", or any state management task.
---

# State Machine

Interface-based state machines for game flow, characters, and UI.

## Core Interface

```csharp
public interface IState
{
    Awaitable Enter(CancellationToken ct);
    Awaitable Exit(CancellationToken ct);
}
```

For states that need per-frame updates, extend with `IUpdatableState`:
```csharp
public interface IUpdatableState : IState
{
    void Update();
}
```

## State Machine Service

Only one state is active at a time. Transitions are async to support loading screens and animations.

```csharp
public class StateMachine
{
    private IState _currentState;

    public async Awaitable TransitionTo(IState newState, CancellationToken ct) {
        if (_currentState != null)
            await _currentState.Exit(ct);
        _currentState = newState;
        await _currentState.Enter(ct);
    }
}
```

## Common Implementations

### Game Flow States
```
LoadingState → LobbyState → GamePlayState → GameOverState
                  ↑                              │
                  └──────────────────────────────┘
```

Each state manages its own scene loading, UI, and services:
```csharp
public class GamePlayState : IState
{
    public async Awaitable Enter(CancellationToken ct) {
        await SceneManager.LoadSceneAsync("GamePlay", LoadSceneMode.Additive)
            .WithCancellation(ct);
        // Initialize controllers, enable input
    }

    public async Awaitable Exit(CancellationToken ct) {
        // Cleanup, disable input
        await SceneManager.UnloadSceneAsync("GamePlay")
            .WithCancellation(ct);
    }
}
```

### Character States
```
IdleState ←→ RunState → JumpState → FallState
    ↑           ↑           │           │
    └───────────────────────┴───────────┘
```

Character states typically need per-frame updates and transition conditions:
```csharp
public class CharacterIdleState : IUpdatableState
{
    private readonly CharacterController _controller;
    private readonly StateMachine _fsm;

    public async Awaitable Enter(CancellationToken ct) {
        _controller.PlayAnimation("Idle");
    }

    public void Update() {
        if (_controller.MoveInput.magnitude > 0.1f)
            _fsm.TransitionTo(_runState, _ct).Forget();
    }

    public async Awaitable Exit(CancellationToken ct) { }
}
```

### UI Screen Flow
```
MainMenuScreen → SettingsScreen → ConfirmDialog
      ↑                               │
      └───────────────────────────────┘
```

Use for screen stacks where each screen manages its own visual state.

## Rules

- One active state at a time per state machine
- Always use `CancellationToken` in Enter/Exit
- States should not reference each other directly — transitions go through the StateMachine
- When using DI-first architecture, register states and StateMachine in the container
- When using SO-first architecture, states can be MonoBehaviours with serialized transition references
- Clean up subscriptions and listeners in `Exit()`
