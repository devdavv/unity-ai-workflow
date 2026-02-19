# Shaders as Game Feel

> Shaders are first-class feel tools — not just visual polish. Use them to give actions weight, clarity, and impact through material-level feedback.
>
> All examples below target **URP Shader Graph** (built-in, free). Custom HLSL equivalents are noted where relevant.
> See `docs/ASSET_RESOURCES.md → Shaders & Procedural Graphics` for learning resources.

---

## Shader Feel Techniques

### 1. Outline Flash (Hit Confirmation)
**When**: Player or enemy is hit; item picked up; selectable object hovered.
**Effect**: Briefly render a solid-color outline around the sprite/mesh.
**Implementation**: URP Shader Graph — sample main texture, offset UVs in 4 directions, fill non-transparent pixels with flash color. Drive intensity with a `_FlashAmount` float property animated via DOTween/PrimeTween.
**Tuning**: Flash duration 0.05–0.1s; color white or accent color; combine with hitstop for maximum impact.

---

### 2. Dissolve / Spawn Transition
**When**: Enemy death, object destruction, character spawn, teleport.
**Effect**: Sprite/mesh dissolves from edges inward (or reverse for spawn) using a noise texture threshold.
**Implementation**: Shader Graph — sample a noise texture, compare against a `_Threshold` float (0 = fully visible, 1 = fully dissolved). Add edge glow color at the threshold boundary.
**Tuning**: Dissolve over 0.3–0.8s; pair with a particle burst at peak dissolve point; edge glow color matches hit flash or faction color.

---

### 3. Chromatic Aberration (Damage / Impact)
**When**: Player takes heavy damage; screen shake trigger; explosion nearby.
**Effect**: Splits the render into R/G/B channels offset slightly in different directions, creating a "screen damage" look.
**Implementation**: URP Renderer Feature (fullscreen pass) or Shader Graph on a UI overlay quad. Drive `_Offset` intensity with a short tween — spike in on impact, ease back to 0.
**Tuning**: Max offset 2–4px at 1080p; duration 0.1–0.2s; combine with camera shake (Medium profile) for layered impact.

---

### 4. UV Scroll (Charged / Powered State)
**When**: Weapon charged, ability ready, shield active, overdriven state.
**Effect**: Texture coordinates scroll continuously, creating animated energy/flow on the material.
**Implementation**: Shader Graph — add `Time * _ScrollSpeed` to UV coordinates before sampling texture. Use a tiling energy/noise texture. Optional: drive `_ScrollSpeed` via script to ramp up as charge builds.
**Tuning**: Subtle scroll (0.1–0.3 speed) for passive state; ramp to 1.0–2.0 at full charge; pair with emission glow increase.

---

### 5. Emission Glow Pulse (Feedback / Alert)
**When**: Low health, nearby objective, countdown timer, collectible in range.
**Effect**: Material emission intensity pulses rhythmically.
**Implementation**: Shader Graph — multiply base color by an `_EmissionIntensity` float on the emission output. Animate `_EmissionIntensity` with a PrimeTween loop or math (Mathf.PingPong).
**Tuning**: Pulse frequency maps to urgency — slow (0.5Hz) for ambient, fast (3Hz) for danger; sync to SFX pulse if applicable.

---

### 6. Hit Flash (Full Sprite Whitout)
**When**: Any hit confirmation — simpler alternative to outline flash.
**Effect**: Entire sprite flashes to solid white or a tint color for one frame.
**Implementation**: Shader Graph — lerp between main texture color and solid `_FlashColor` using `_FlashAmount` (0–1). Simpler than outline; same tween-driven property.
**Tuning**: 1–2 frames (0.016–0.033s) for snap; longer (0.05s) for heavier hits.

---

### 7. Screen-Space Distortion (Impact / Explosion)
**When**: Large explosion, shockwave, portal activation, teleport arrival.
**Effect**: Briefly warps the screen geometry around a point using UV distortion.
**Implementation**: URP Renderer Feature — sample a distortion normal map, apply as UV offset to the final render texture. Drive `_DistortionStrength` and `_DistortionRadius` with a quick ease-in/ease-out tween.
**Tuning**: Peak strength on impact frame; ease to 0 over 0.2–0.4s; combine with chromatic aberration for layered effect.

---

## Implementation Notes

- All shader-driven feel uses **material property blocks** (`MaterialPropertyBlock`) — never instantiate materials at runtime.
  ```csharp
  // ✅ CORRECT — no allocations
  _propertyBlock.SetFloat(_flashAmountId, value);
  _renderer.SetPropertyBlock(_propertyBlock);
  ```
- Cache `Shader.PropertyToID()` in `Awake()` for all property names.
- Animate shader properties with DOTween `.To()` or PrimeTween `Tween.Custom()` — same kill-on-destroy rules apply.
- For fullscreen effects (chromatic aberration, distortion): implement as a **URP Renderer Feature** on the active UniversalRendererData asset, not on individual object materials.

## Optional Owned Packages
If you own **Shapes** (`has_shapes: true` in ProjectConfig) or **True Shadow** (`has_true_shadow: true`), these packages handle several of the above effects with zero shader code. Mention this to the user when relevant, but default to the custom URP approach for portability.
