# Phase 4: Production

> **Agents**: All (on demand)
> **Workflows**: `/plan` → `/create` → `/test` → `/debug`

## Goal
Build the game through iterative sprints.

## Sprint Cycle

```
/plan → Define tasks → /create → Implement → /test → Verify → /debug → Fix → Commit
  ↑                                                                              |
  └──────────────────────── Next Sprint ◄────────────────────────────────────────┘
```

### 1. Sprint Planning (`/plan`)
- Define sprint goal from GDD
- Break into tasks with estimates
- Assign to agent personas
- Create Linear tasks (if connected)

### 2. Implementation (`/create`)
- TDD-first: write test → implement → pass
- Follow RULES.md and coding standards
- Use feature branches: `feature/{name}`
- Commit messages: `feat({scope}): {description}`

### 3. Testing (`/test`)
- Map GDD Gherkin scenarios to NUnit tests
- EditMode for pure logic, PlayMode for Unity lifecycle
- Run via Unity MCP or manually

### 4. Debugging (`/debug`)
- 4-phase framework: Gather → Isolate → Hypothesize → Fix
- Use GameDebug for targeted logging
- Fix commits: `fix({scope}): {root cause}`

### 5. Integration
- Merge feature branches via PR (GitHub MCP)
- Run full test suite on develop
- Resolve merge conflicts

## Commit Convention
```
feat(player): implement jump mechanic
fix(health): prevent negative health values
refactor(ui): extract health bar to ViewModel
test(combat): add damage edge case tests
chore(project): update package versions
```

## Entry Criteria
- Phase 3 complete (project skeleton ready)

## Exit Criteria
- [ ] All planned features implemented
- [ ] All GDD scenarios have passing tests
- [ ] No critical bugs
- [ ] Ready for polish
