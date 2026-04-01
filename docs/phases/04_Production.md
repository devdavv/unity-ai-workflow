# Phase 4: Production

> **Commands**: `/uw-cmd-plan` -> `/uw-cmd-implement-feature` (per feature) -> commit -> repeat
> **Supporting**: `/uw-cmd-test`, `/uw-cmd-debug`, `/uw-cmd-create` (for non-gameplay code)

## Goal
Build the game through iterative feature sprints. Every feature ships complete — code, game feel, and documentation — before moving to the next.

## The Feature Loop

```
/uw-cmd-plan → pick feature → /uw-cmd-implement-feature → tests pass → game feel integrated → commit
  ↑                                                                                  |
  └────────────────────────── Next Feature ◄─────────────────────────────────────────┘
```

Polish is **not Phase 5**. Each pass through `/uw-cmd-implement-feature` includes:
1. Deep interrogation (reference games, edge cases, scope)
2. Game feel questions upfront (VFX, SFX, camera, haptics)
3. GDD + GFD documentation before coding
4. TDD-first implementation
5. Game feel integration in the same pass (or a filed TODO if deferred)

---

## Sprint Cycle

### 1. Sprint Planning (`/uw-cmd-plan`)
- Define sprint goal from GDD feature list
- Break into individual features (`/uw-cmd-implement-feature` candidates) and tasks (`/uw-cmd-create` candidates)
- Estimate, assign owners, create Linear tasks (if connected)

### 2. Feature Implementation (`/uw-cmd-implement-feature`)
For every player-facing feature:
- Interrogate → document → implement → feel → commit
- Run `/uw-cmd-implement-feature` for the full loop

For non-gameplay utilities (helpers, data classes, editor tools):
- Use `/uw-cmd-create` directly

### 3. Testing (`/uw-cmd-test`)
- Map GDD Gherkin scenarios to NUnit tests
- EditMode for pure logic, PlayMode for Unity lifecycle
- Run via Unity MCP or manually in Test Runner

### 4. Debugging (`/uw-cmd-debug`)
- 4-phase framework: Gather → Isolate → Hypothesize → Fix
- Use GameDebug for targeted logging
- Fix commits: `fix({scope}): {root cause}`

### 5. Integration
- Merge feature branches via PR (GitHub MCP)
- Run full test suite on `develop`
- Resolve merge conflicts

---

## Commit Convention
```
feat(player): implement jump mechanic with game feel
feat(health): add damage system
fix(health): prevent negative health values
style(player): tune jump feel values
refactor(ui): extract health bar to ViewModel
test(combat): add damage edge case tests
chore(project): update package versions
```

---

## Entry Criteria
- Phase 3 complete (project skeleton, GameDebug, packages, git all configured)
- GDD has at least a rough feature list
- `ai_mode` set in ProjectConfig.yaml

## Exit Criteria
- [ ] All planned features implemented and committed
- [ ] All GDD Gherkin scenarios have passing tests
- [ ] No critical bugs
- [ ] GFD Feedback Matrix has a row per feature (integrated or `TODO(gamefeel)` filed)
- [ ] No deferred game feel TODOs that were explicitly approved to defer

> Note: There is no longer a separate "Polish Phase" as a gate. Game feel is woven into production.
> Phase 5 is reserved for final tuning passes, visual polish (lighting, VFX finesse), and performance profiling.
