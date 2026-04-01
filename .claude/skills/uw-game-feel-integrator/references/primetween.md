# PrimeTween Patterns

## Scale Punch
```csharp
Tween.PunchScale(transform, Vector3.one * 0.2f, 0.15f);
```

## Color Flash
```csharp
Tween.Color(spriteRenderer, Color.red, 0.05f, cycles: 2);
```

## Screen Shake
```csharp
Tween.ShakeLocalPosition(Camera.main.transform, 0.3f, 0.2f);
```

## UI Slide In
```csharp
Tween.UIAnchoredPosition(rectTransform, Vector2.zero, 0.3f, ease: Ease.OutBack);
```

## Cleanup
PrimeTween auto-cleans on destroy — no manual kill needed.
