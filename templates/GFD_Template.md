---
title: "[Project Name] — Game Feel Document"
version: "0.1.0"
date: ""
author: ""
status: "Draft"
---

# Game Feel Document

> **How to use this template**: Work through the Feedback Matrix with the **Game Designer agent** during `/brainstorm` or `/polish`. Every meaningful player action should produce at least 3 types of feedback.

---

## 1. Feedback Philosophy

**Rule of Three**: Every player action that matters should produce feedback in at least 3 channels:
1. **Visual** — Particles, flashes, screen effects, UI animations
2. **Audio** — SFX, musical stings, pitch variation
3. **Kinesthetic** — Camera shake, hitstop, controller haptics, time scale

---

## 2. Feel Middleware

> Populated from `ProjectConfig.yaml → feel_tools`

| Type | Tool | Notes |
|------|------|-------|
| Tweening | | DOTween / PrimeTween / Custom |
| Feedback System | | FEEL / MMFeedbacks / Custom |
| Audio | | FMOD / Wwise / Unity Audio |
| Camera | | Cinemachine / Custom |

---

## 3. Master Feedback Matrix

| Event | VFX | Audio | Camera | Tween | Haptic | Priority |
|-------|-----|-------|--------|-------|--------|----------|
| Player Jump | Dust particles | Jump SFX | Slight zoom out | Squash/stretch | Light tap | High |
| Player Land | Impact dust | Land thud | Light shake | Scale compress | Medium tap | High |
| Player Hit | Blood/spark | Impact hit | Medium shake | Color flash (red) | Strong pulse | Critical |
| Player Death | Explosion/fade | Death SFX + sting | Heavy shake → zoom | Ragdoll/dissolve | Long pulse | Critical |
| Enemy Death | Explosion | Death pop | Light shake | Scale → particles | Light tap | Medium |
| Coin Collect | Sparkle trail | Chime (pitched up) | None | Scale bounce | Light tap | Medium |
| UI Button Press | Highlight glow | Click SFX | None | Scale punch | None | Low |
| Level Complete | Confetti/fireworks | Fanfare | Zoom out | UI slide-in | Pattern | High |
| | | | | | | |

---

## 4. Camera Shake Profiles

| Profile | Duration | Magnitude | Frequency | Falloff | Use Case |
|---------|----------|-----------|-----------|---------|----------|
| Light | 0.1s | 0.1 | 15 | Linear | Coin, small hit |
| Medium | 0.2s | 0.3 | 15 | EaseOut | Damage, explosion |
| Heavy | 0.35s | 0.6 | 20 | EaseOut | Boss hit, death |
| Rumble | 1.0s | 0.15 | 10 | Linear | Earthquake, engine |

---

## 5. Hitstop Profiles

| Profile | Duration | Time Scale | Use Case |
|---------|----------|------------|----------|
| Micro | 0.02s | 0.0 | Light attacks |
| Short | 0.05s | 0.0 | Medium attacks |
| Impact | 0.1s | 0.0 | Heavy attacks, kills |
| Dramatic | 0.2s | 0.1 | Boss kills, critical moments |

---

## 6. UI Motion Guidelines

| Element | Enter | Exit | Duration | Easing |
|---------|-------|------|----------|--------|
| Panel | Slide up + fade | Slide down + fade | 0.3s | EaseOutCubic |
| Button hover | Scale to 1.05 | Scale to 1.0 | 0.1s | EaseOutQuad |
| Notification | Slide in from right | Fade out | 0.25s | EaseOutBack |
| Score counter | Count up animation | — | 0.5s | Linear |
| Health bar | Smooth lerp | — | 0.2s | EaseOutQuad |

---

## 7. Audio Design Notes

### SFX Rules
- All impact sounds should have **2-3 pitch variations** (±10%) to avoid repetition.
- Layer sounds: base impact + environmental reverb + character-specific flavor.
- Keep SFX under **2 seconds** unless ambient.

### Music
<!-- Musical style, adaptive music system, layer triggers -->
