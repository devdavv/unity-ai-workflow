# Phase 5: Polish

> **Agent**: Game Designer + Art Director
> **Workflow**: `/polish`
> **Skill**: `game-feel-integrator`

## Goal
Transform a functional game into a *feeling* game. Add juice, optimize performance, and finalize art.

## Activities

### 1. Juice Pass (`/polish`)
For each feature, apply the GFD Feedback Matrix:
- Screen shake on impacts
- Tweens on UI interactions
- Particle effects on key events
- Audio with pitch variation
- Hitstop for impactful moments
- Camera reactions

### 2. Art Integration
Art Director populates scenes:
- Replace placeholder geometry with final art
- Set up materials and lighting
- Configure LODs for target platforms
- Apply naming conventions to all assets

### 3. Performance Optimization
- Profile with Unity Profiler
- Identify hot paths in Update loops
- Object pooling for particles and projectiles
- Texture compression for target platforms
- Draw call batching

### 4. Regression Testing
- Run full test suite after every change
- Verify no gameplay regressions from polish
- Performance benchmarks meet TDD targets

### 5. Final Tuning
- Adjust feel values (start exaggerated, dial back)
- Balance data in ScriptableObjects
- Playtest and iterate

## Entry Criteria
- Phase 4 complete (features working)

## Exit Criteria
- [ ] All GFD Feedback Matrix entries implemented
- [ ] Art assets integrated
- [ ] Performance meets TDD targets
- [ ] Full test suite passing
- [ ] Game feels polished and responsive
