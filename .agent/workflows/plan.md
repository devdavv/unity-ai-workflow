---
description: Break a feature or sprint into actionable tasks with estimates and dependencies.
---

# /plan — Planning Workflow

Break a feature or sprint into actionable tasks with clear acceptance criteria.

## Agent
**Architect** (primary), **Game Designer** (for design validation).

## Steps

### 1. Define the Feature
Ask the user:
- What **feature** or **sprint goal** are we planning?
- Which **GDD scenario(s)** does this map to?
- Are there **dependencies** on existing systems?

### 2. Architecture Check
Read `docs/TDD.md` to understand:
- Which assemblies are involved
- Data flow between systems
- Any API constraints

### 3. Task Breakdown
Create a numbered task list with:
- **Task name** (action-oriented: "Implement X", "Create Y")
- **Estimated complexity** (S/M/L)
- **Dependencies** (which tasks must complete first)
- **Acceptance criteria** (what "done" looks like)
- **Agent** (which agent persona handles this task)

### 4. Sprint Structure
```markdown
## Sprint [N]: [Goal]
**Duration**: [X days/weeks]

| # | Task | Size | Depends On | Agent | Status |
|---|------|------|-----------|-------|--------|
| 1 | Create PlayerHealth SO | S | — | Gameplay Dev | ⬜ |
| 2 | /implement-feature: damage system (incl. game feel) | M | 1 | Gameplay Dev + Game Designer | ⬜ |
| 3 | /implement-feature: health UI (incl. animations) | M | 1 | UI Specialist + Game Designer | ⬜ |
| 4 | Write health edge-case tests | S | 2 | QA Tester | ⬜ |
```

> **Note**: Game feel (VFX, SFX, camera, tweens) is embedded inside each `/implement-feature` task — not a separate row. If a feature's game feel is explicitly deferred, mark it with `TODO(gamefeel)` and address it in Phase 5.

### 5. Output
- Updated `docs/SprintPlan.md`
- If Linear MCP is connected, create tasks in Linear
- Task dependency graph (optional)
