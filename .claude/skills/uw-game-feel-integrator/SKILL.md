---
name: uw-game-feel-integrator
description: Inject juice and game feel into gameplay code using the project's chosen middleware. Use when adding screen shake, tweens, particles, audio feedback, haptics, hitstop, or any form of "juice" to gameplay events. Triggers on requests like "add juice", "make this feel better", "add screen shake", "polish this feature", "add feedback to this action", "make the hit feel impactful", "add particles when", "screen shake on damage", "tween this", "add camera shake", "make this snappy", or any game feel, polish, or feedback work. Always reads ProjectConfig.yaml -> feel_tools and docs/GFD.md Feedback Matrix before generating code.
---

# Game Feel Integrator

Apply juice to gameplay events using the GFD Feedback Matrix.

## Before You Start

1. Read `docs/ProjectConfig.yaml` for:
   - `feel_tools.tweening` — which tween library (`"dotween"`, `"primetween"`, or `"none"`).
   - `feel_tools.feedback_system` — feedback framework if any.
   - `feel_tools.audio` — audio middleware.
   - `feel_tools.camera` — camera system (Cinemachine, custom, etc.).
   - `mcp.unity_mcp` — if `true`, call `refresh_unity` after creating files.
2. Read `docs/GFD.md` for the Feedback Matrix — it defines which events need which feedback channels.
3. Read `docs/CODING_STANDARDS.md` for async patterns (`Awaitable` + `CancellationToken`) used in middleware-agnostic patterns.

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

## Tween Cleanup

Tweens must be killed when their target is destroyed or disabled, otherwise they cause null reference exceptions or operate on stale objects.

**DOTween:**
```csharp
private void OnDestroy()
{
    transform.DOKill();  // Kill all tweens on this transform
}
```

**PrimeTween:**
```csharp
private void OnDestroy()
{
    Tween.StopAll(this);  // Kill all tweens targeting this object
}
```

**No middleware (Awaitable):** Use `CancellationToken` linked to `destroyCancellationToken`:
```csharp
private async Awaitable FlashAsync()
{
    var ct = destroyCancellationToken;
    // Awaitable work — auto-cancels when MonoBehaviour is destroyed
    await Awaitable.WaitForSecondsAsync(0.1f, ct);
}
```

## Tuning
- Start **exaggerated**, then dial back.
- Always check the GFD Feedback Matrix before implementing.
- Ensure tweens are killed on object destruction (see Tween Cleanup above).
- **Sync ADSR across channels**: Attack and Release timings must match across Visual, Audio, and Kinesthetic. If the SFX fades over 0.5s, particles and shake damping must also fade over 0.5s.
- **Profile feel code**: Use Unity Profiler (Timeline view) to check that feel effects don't cause frame drops. Particle bursts, tween cascades, and audio one-shots in the same frame can spike.

## Sourcing & Communication
- **Cite your sources**: When suggesting a feel pattern, name the reference game. Example: *"A swap feel inspired by Royal Match's snappy 0.15s tween"* or *"Celeste's coyote time approach."* This helps the user visualize and verify.
- **Recommend assets**: When VFX, SFX, or art assets are needed, reference `docs/ASSET_RESOURCES.md` for curated free/paid sources.

## After Setup

- **Write tests:** Use `uw-unity-test-runner` — test feel parameters (shake profile values, hitstop duration) in EditMode tests.
- **Code review:** Use `uw-code-review` to verify Rule of Three and tween cleanup before committing.
- **UI animation:** Use `uw-ui-toolkit-binder` for UI Toolkit USS transitions. Use game feel patterns here for effects that go beyond USS (complex sequences, screen flash).
- **Debug feel issues:** Use `uw-unity-debugging` if effects aren't triggering or timing feels off.

## Rules

- Rule of Three: every meaningful action needs feedback in at least 3 channels (Visual, Audio, Kinesthetic).
- Pool all particles, floating text, and projectiles — never `Instantiate` in hot paths.
- Kill/stop tweens in `OnDestroy` or `OnDisable` to prevent null reference exceptions.
- Sync ADSR across channels — mismatched timing breaks immersion.
- `[SerializeField] private` for shake profiles, tween durations, and other tuning values.
- All game feel code must live inside an `.asmdef`.
- If `ProjectConfig.yaml -> mcp.unity_mcp` is `true`, call `refresh_unity` after creating files.
