# Phase 5: Polish

> **Agents**: Game Designer + Art Director
> **Workflow**: `/polish`
> **Skill**: `game-feel-integrator`

## Goal

Final tuning, art integration, and performance profiling. Game feel was integrated per-feature during Phase 4 — Phase 5 is about raising the ceiling, not building the floor.

If any features have deferred `TODO(gamefeel):` comments, this phase resolves them first.

---

## Activities

### 0. Resolve Deferred Game Feel
Before anything else — scan for `TODO(gamefeel):` comments across the codebase:
- For each found, run a mini `/implement-feature` game feel pass (Steps 3–7 only)
- Update the GFD Feedback Matrix row from "pending" to complete
- Remove the TODO comment once addressed

### 1. Final Tuning Pass (`/polish`)
For features where game feel is already integrated, tune and amplify:
- Revisit each GFD Feedback Matrix row
- Adjust values (shake magnitude, tween duration, SFX volume) after extended playtesting
- "Start exaggerated, dial back" still applies to any values that haven't been playtested enough

### 2. Art Integration
Art Director replaces placeholder art with final assets:
- Swap placeholder geometry with final models/sprites
- Set up materials, shaders, and lighting
- Configure LODs for target platforms
- Apply naming conventions to all assets (`M_`, `T_`, `SPR_`, etc.)

### 3. Performance Optimization
- Profile with Unity Profiler — identify hot paths in Update loops
- Pool particles and projectiles if not already done
- Texture compression for target platforms
- Draw call batching and GPU instancing where applicable
- Verify all performance targets from `docs/TDD.md` are met

### 4. Regression Testing
- Run full test suite after every change
- Verify no gameplay regressions from polish changes
- Performance benchmarks must meet TDD targets

### 5. Final Playtest
- Full playthrough — does it feel complete?
- Check all GFD Feedback Matrix entries are implemented
- Any moment that still feels "flat" gets a last `/polish` pass

---

## Entry Criteria
- Phase 4 complete (all features implemented and committed)
- GFD Feedback Matrix has a row per feature (integrated or `TODO(gamefeel)` filed)

## Exit Criteria
- [ ] All `TODO(gamefeel):` comments resolved (or consciously deferred with a note)
- [ ] All GFD Feedback Matrix entries implemented and tuned
- [ ] Final art assets integrated (no placeholder geometry in release scenes)
- [ ] Performance meets TDD targets
- [ ] Full test suite passing
- [ ] Game feels polished and responsive
