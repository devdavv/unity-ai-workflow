# DOTween Patterns

## Scale Punch
```csharp
transform.DOPunchScale(Vector3.one * 0.2f, 0.15f, 5, 0.5f);
```

## Color Flash
```csharp
spriteRenderer.DOColor(Color.red, 0.05f).SetLoops(2, LoopType.Yoyo);
```

## Screen Shake
```csharp
Camera.main.DOShakePosition(0.2f, 0.3f, 15, 90f);
```

## UI Slide In
```csharp
rectTransform.DOAnchorPosY(0f, 0.3f).From(new Vector2(0, -500)).SetEase(Ease.OutBack);
```

## Cleanup
Always kill tweens on destroy:
```csharp
private void OnDestroy() => transform.DOKill();
```
