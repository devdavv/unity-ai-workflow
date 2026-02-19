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

## Tuning
- Start **exaggerated**, then dial back.
- Always check the GFD Feedback Matrix before implementing.
- Ensure tweens are killed on object destruction.
- **Sync ADSR across channels**: Attack and Release timings must match across Visual, Audio, and Kinesthetic. If the SFX fades over 0.5s, particles and shake damping must also fade over 0.5s.
