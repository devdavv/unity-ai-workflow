---
name: Game Feel Integrator
description: Inject juice and game feel into gameplay code using the project's chosen middleware. Use when adding screen shake, tweens, particles, audio feedback, haptics, or hitstop to gameplay events. Triggers on requests like "add juice", "make this feel better", "add screen shake", "polish this feature", or any game feel work. Always reads ProjectConfig.yaml → feel_tools and docs/GFD.md Feedback Matrix before generating code.
---

# Game Feel Integrator

Apply juice to gameplay events using the GFD Feedback Matrix.

## Before You Start
1. Read `docs/ProjectConfig.yaml → feel_tools` for available middleware.
2. Read `docs/GFD.md` for the Feedback Matrix.

## Rule of Three
Every meaningful action needs feedback in **at least 3 channels**: Visual, Audio, Kinesthetic.

## Middleware Reference
- **DOTween**: See [references/dotween.md](references/dotween.md)
- **PrimeTween**: See [references/primetween.md](references/primetween.md)
- **No middleware**: Use `async Awaitable` coroutines
- **Shader effects as feel**: See [references/shaders-as-feel.md](references/shaders-as-feel.md) — outline flash, dissolve, chromatic aberration, UV scroll, distortion

## Theory & Inspiration
- **Game feel foundations + Loic Jacob methodology**: See [references/gamefeel-theory.md](references/gamefeel-theory.md)

## Universal Patterns (middleware-agnostic)

### Hitstop
```csharp
private async Awaitable Hitstop(float duration = 0.05f)
{
    Time.timeScale = 0f;
    await Awaitable.WaitForSecondsAsync(duration);
    Time.timeScale = 1f;
}
```

### Shake Profile
```csharp
[System.Serializable]
public struct ShakeProfile
{
    public float duration;
    public float magnitude;
    public static ShakeProfile Light => new() { duration = 0.1f, magnitude = 0.1f };
    public static ShakeProfile Medium => new() { duration = 0.2f, magnitude = 0.3f };
    public static ShakeProfile Heavy => new() { duration = 0.35f, magnitude = 0.6f };
}
```

## Object Pooling

Frequently spawned FX (particles, floating text, projectiles) must be pooled to avoid GC spikes:

```csharp
public class FXPool<T> where T : MonoBehaviour
{
    private readonly Queue<T> _pool = new();
    private readonly T _prefab;
    private readonly Transform _parent;

    public T Get() {
        var item = _pool.Count > 0 ? _pool.Dequeue() : Object.Instantiate(_prefab, _parent);
        item.gameObject.SetActive(true);
        return item;
    }

    public void Return(T item) {
        item.gameObject.SetActive(false);
        _pool.Enqueue(item);
    }
}
```

**Rules:** Pool all particles, floating text, projectiles. Return to pool on `OnParticleSystemStopped` or after tween completes. Pre-warm pools during scene load.

## Performance Tips
- **Camera separation**: Use separate world + UI cameras so post-processing doesn't affect UI
- **Shaders over CPU animations**: For simple repetitive motion (scrolling, pulsing), prefer shader-based animation — runs parallel on GPU, cheaper than DOTween
- **Shared materials enable batching**: Use material property blocks to vary parameters without breaking draw call batching
- **Legacy Animation for simple UI**: For simple UI animations (fade, slide), Legacy Animation clips are more performant than Animator controllers

## Tuning
- Start **exaggerated**, then dial back.
- Always check the GFD Feedback Matrix before implementing.
- Ensure tweens are killed on object destruction.
- **Sync ADSR across channels**: Attack and Release timings must match across Visual, Audio, and Kinesthetic. If the SFX fades over 0.5s, particles and shake damping must also fade over 0.5s.
