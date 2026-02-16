---
name: Game Feel Integrator
description: Injects juice into gameplay code using the project's chosen feel middleware.
---

# Game Feel Integrator

Applies "juice" (screen shake, tweens, particles, audio, haptics) to gameplay events. **Always reads `ProjectConfig.yaml → feel_tools`** to use the correct middleware.

## Prerequisites
- Read `docs/ProjectConfig.yaml → feel_tools` for available middleware.
- Read `docs/GFD_Template.md` for the Feedback Matrix.
- Check which tweening library is available before generating code.

## Feedback Philosophy

Every meaningful player action should produce **at least 3 types of feedback**:
1. **Visual** — VFX, screen shake, color flash, UI animation
2. **Audio** — SFX, pitch variation, spatial audio
3. **Kinesthetic** — Controller haptics, time scale (hitstop), camera zoom

## Middleware Adapters

### DOTween
```csharp
using DG.Tweening;

// Scale punch on hit
transform.DOPunchScale(Vector3.one * 0.2f, 0.15f, 5, 0.5f);

// Color flash
spriteRenderer.DOColor(Color.red, 0.05f)
    .SetLoops(2, LoopType.Yoyo);

// Screen shake
Camera.main.DOShakePosition(0.2f, 0.3f, 15, 90f);
```

### PrimeTween
```csharp
using PrimeTween;

// Scale punch
Tween.PunchScale(transform, Vector3.one * 0.2f, 0.15f);

// Color flash
Tween.Color(spriteRenderer, Color.red, 0.05f, cycles: 2);

// Screen shake
Tween.ShakeLocalPosition(Camera.main.transform, 0.3f, 0.2f);
```

### Custom (No Middleware)
```csharp
using UnityEngine;

// Simple coroutine-based flash
private async Awaitable FlashColor(SpriteRenderer sr, Color color, float duration)
{
    var original = sr.color;
    sr.color = color;
    await Awaitable.WaitForSecondsAsync(duration);
    sr.color = original;
}
```

## Screen Shake Profile

```csharp
[System.Serializable]
public struct ShakeProfile
{
    public float duration;
    public float magnitude;
    public AnimationCurve falloff;

    public static ShakeProfile Light => new()
    { duration = 0.1f, magnitude = 0.1f };

    public static ShakeProfile Medium => new()
    { duration = 0.2f, magnitude = 0.3f };

    public static ShakeProfile Heavy => new()
    { duration = 0.35f, magnitude = 0.6f };
}
```

## Hitstop (Time Scale)

```csharp
private async Awaitable Hitstop(float duration = 0.05f)
{
    Time.timeScale = 0f;
    await Awaitable.WaitForSecondsAsync(duration, destroyCancellationToken,
        cancelImmediatelyOnDestroy: true);
    Time.timeScale = 1f;
}
```

## GFD Feedback Matrix Reference

When applying juice, always check the GFD's Feedback Matrix:

| Event | VFX | Audio | Camera | Tween | Haptic |
|-------|-----|-------|--------|-------|--------|
| Player Hit | Blood splash | Impact SFX | Medium shake | Color flash | Strong pulse |
| Coin Collect | Sparkle | Chime | Slight zoom | Scale bounce | Light tap |
| Enemy Death | Explosion | Death SFX | Light shake | Ragdoll | Medium pulse |

*This table is populated during the GFD creation phase.*
