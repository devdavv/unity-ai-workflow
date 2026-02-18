# Phase 4: Production

> **Agents**: All (on demand)
> **Primary Workflow**: `/plan` → `/implement-feature` (per feature) → commit → repeat
> **Supporting Workflows**: `/test`, `/debug`, `/create` (for non-gameplay code)

## Goal
Build the game through iterative feature sprints. Every feature ships complete — code, game feel, and documentation — before moving to the next.

## The Feature Loop

```
/plan → pick feature → /implement-feature → tests pass → game feel integrated → commit
  ↑                                                                                  |
  └────────────────────────── Next Feature ◄─────────────────────────────────────────┘
```

Polish is **not Phase 5**. Each pass through `/implement-feature` includes:
1. Deep interrogation (reference games, edge cases, scope)
2. Game feel questions upfront (VFX, SFX, camera, haptics)
3. GDD + GFD documentation before coding
4. TDD-first implementation
5. Game feel integration in the same pass (or a filed TODO if deferred)

---

## Sprint Cycle

### 1. Sprint Planning (`/plan`)
- Define sprint goal from GDD feature list
- Break into individual features (`/implement-feature` candidates) and tasks (`/create` candidates)
- Estimate, assign agents, create Linear tasks (if connected)

### 2. Feature Implementation (`/implement-feature`)
For every player-facing feature:
- Interrogate → document → implement → feel → commit
- See [implement-feature.md](../.agent/workflows/implement-feature.md) for the full loop

For non-gameplay utilities (helpers, data classes, editor tools):
- Use `/create` directly

### 3. Testing (`/test`)
- Map GDD Gherkin scenarios to NUnit tests
- EditMode for pure logic, PlayMode for Unity lifecycle
- Run via Unity MCP or manually in Test Runner

### 4. Debugging (`/debug`)
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
