# /polish — Polish Workflow

Apply "juice" to a working feature using the Game Feel Document's Feedback Matrix.

## Agent
Load **Game Designer** from `.agent/agents/game-designer.md`.
Load `game-feel-integrator` skill from `.agent/skills/game-feel-integrator/SKILL.md`.

## Prerequisites
- The feature **must work correctly** before polish. Don't polish broken code.
- `docs/GFD.md` must have entries for the events you're polishing.
- Check `docs/ProjectConfig.yaml → feel_tools` for available middleware.

## Steps

### 1. Audit Current Feel
Play the feature and note:
- Which actions feel **flat** or **unresponsive**?
- Which moments should be **impactful** but aren't?
- Is there **visual feedback** for every player input?

### 2. Consult the GFD
For each event in the feature, check the Feedback Matrix:
```
Event: Player Hit
├── VFX: Blood splash, screen flash
├── Audio: Impact SFX (pitch-varied)
├── Camera: Medium shake (0.2s, 0.3 magnitude)
├── Tween: Color flash (red, 0.05s yoyo)
└── Haptic: Strong pulse (0.1s)
```

### 3. Implement Juice
Use `game-feel-integrator` skill:
- Screen shake, tweens (scale punch, color flash, position bounce)
- Particle effects, audio cues with pitch variation
- Hitstop (time scale pause) for impactful moments
- Camera zoom for dramatic events

### 4. Tune Values
- Start **exaggerated**, then dial back.
- "Too much juice" is easier to fix than "too little."
- Test on target platform — mobile haptics differ from desktop.

### 5. Performance Check
- Profile the added effects.
- Ensure particle systems are pooled.
- Ensure tweens are properly killed on object destruction.

### 6. Output
- Polished feature with full feedback loop
- Updated GFD with final tuned values
- Performance validated

$ARGUMENTS
