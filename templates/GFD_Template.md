---
title: "[Project Name] — Game Feel Document"
version: "0.1.0"
date: ""
author: ""
status: "Draft"
---

# Game Feel Document

> **How to use this template**: Work through the Feedback Matrix during `/uw-cmd-brainstorm` (initial pass), `/uw-cmd-implement-feature` (one row per feature during production), or `/uw-cmd-polish` (final tuning). Every meaningful player action should produce at least 3 types of feedback.

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
| Feedback System | | DOTween / PrimeTween / Custom Code |
| Audio | | FMOD / Wwise / Unity Audio |
| Camera | | Custom / Cinemachine (if needed) |

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

---

## 8. Design System

> **How to use**: Fill this out during **Phase 1 (Pre-Production)**. This becomes the single source of truth for all visual decisions.

### Color Palette

| Role | Color | Hex | Usage |
|------|-------|-----|-------|
| Primary | | | Main actions, CTAs, key UI elements |
| Secondary | | | Supporting elements, secondary actions |
| Accent | | | Highlights, notifications, rewards |
| Background | | | Panels, menus, overlays |
| Surface | | | Cards, input fields, elevated containers |
| Text Primary | | | Body text, headings |
| Text Secondary | | | Captions, hints, disabled labels |
| Success | | | Health full, objectives complete |
| Warning | | | Low resource, caution states |
| Danger | | | Damage, death, critical alerts |

> **Accessibility**: Ensure text-on-background combos meet **WCAG AA** (≥ 4.5:1 contrast ratio). Test with a colorblind simulator.

### Typography Scale

| Level | Size | Weight | Use Case |
|-------|------|--------|----------|
| Display | 48px | Bold | Title screens, splash |
| H1 | 32px | Bold | Screen headers |
| H2 | 24px | SemiBold | Section headers |
| H3 | 18px | SemiBold | Subsection headers |
| Body | 16px | Regular | General text |
| Caption | 12px | Regular | Labels, timestamps, hints |
| Overline | 10px | Bold (uppercase) | Categories, tags |

> **Font family**: `<!-- e.g., Inter, Roboto, or a custom game font -->`

### Spacing Grid

Base unit: `4px`. All spacing should be multiples of the base.

| Token | Value | Usage |
|-------|-------|-------|
| xs | 4px | Inline element gaps |
| sm | 8px | Tight padding, icon gaps |
| md | 16px | Standard padding, element spacing |
| lg | 24px | Section spacing |
| xl | 32px | Screen-level margins |
| 2xl | 48px | Major section separations |

### Iconography

| Property | Value |
|----------|-------|
| Style | <!-- Outlined / Filled / Rounded --> |
| Size standard | 24×24px (touch target: 44×44px minimum) |
| Stroke width | <!-- 1.5px / 2px --> |

### HUD Safe Zones

<!-- Define where HUD elements can be placed relative to gameplay -->

| Zone | Position | What goes here |
|------|----------|----------------|
| Top-left | Health, shields | Critical player state |
| Top-right | Score, timer | Session info |
| Bottom-center | Action bar, abilities | Active controls |
| Corners | Minimap, compass | Navigation aids |

> **Rule**: No critical gameplay info in the center 60% of the screen — that's the player's focus area.
