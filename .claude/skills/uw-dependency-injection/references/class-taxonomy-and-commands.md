# DI Class Taxonomy & Command Pattern

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
