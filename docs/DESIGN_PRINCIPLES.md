# Game UI/UX Design Principles

Reference for the **Art Director** agent when designing game interfaces.

## Layout & Readability

- **Center 60% is sacred** — reserved for gameplay. HUD elements stay in edges/corners.
- **Group by function**: health/shields together, score/timer together, actions bottom-center.
- **Consistent anchor points** — players memorize where info lives. Don't move HUD elements between screens.
- **Z-order**: Gameplay → HUD → Menus → Modals → Tooltips (back → front).

## Color in Games

- **Semantic colors are universal**: Green = good, Red = bad, Yellow = caution. Don't fight player instinct.
- **Desaturate backgrounds** so gameplay elements and UI pop by contrast.
- **Enemy vs Ally** must be distinguishable even in colorblind modes (use shape + color, never color alone).
- **Damage flash**: brief red overlay or vignette — never tint the entire screen for more than 0.1s.

## Typography in Games

- **In-gameplay text**: minimum 24px at 1080p. Players are far from the screen.
- **Menu text**: minimum 16px. Players are closer but may still be on TV.
- **Damage numbers**: use scaling + motion (float up, fade out) — never static text.
- **Dynamic content** (scores, timers): use monospace or tabular figures to prevent layout jitter.

## Touch & Input Targets

- **Minimum touch target**: 44×44px (Apple HIG) / 48×48dp (Material).
- **Spacing between targets**: at least 8px to prevent mis-taps.
- **Controller navigation**: every interactive element must be reachable via D-pad. Test tab order.

## Accessibility Checklist

- [ ] Text contrast ≥ 4.5:1 (WCAG AA)
- [ ] No information conveyed by color alone (add icons or patterns)
- [ ] Colorblind simulation tested (Protanopia, Deuteranopia, Tritanopia)
- [ ] Font size options or scaling support
- [ ] Screen reader tags on UI Toolkit elements (`aria-label` / `name` property)
- [ ] Subtitles with speaker identification and background contrast

## Common Game UI Anti-Patterns

| Anti-Pattern | Why It's Bad | Fix |
|-------------|-------------|-----|
| Full-screen blur behind menus | Hides gameplay context, expensive GPU | Semi-transparent overlay with vignette |
| Tiny close buttons on modals | Frustrating on touch/controller | Large X button + tap-outside-to-close |
| Text-only tutorials | Players skip them | Contextual prompts during gameplay |
| Percentage-only health bars | Hard to read at a glance | Bar + number + color shift |
| Animated backgrounds behind text | Reduces readability | Solid or frosted panel behind text |
