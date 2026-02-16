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
| 2 | Implement damage system | M | 1 | Gameplay Dev | ⬜ |
| 3 | Write health tests | S | 2 | QA Tester | ⬜ |
| 4 | Create health UI | M | 1 | UI Specialist | ⬜ |
| 5 | Add hit feedback | S | 2,4 | Game Designer | ⬜ |
```

### 5. Output
- Updated `docs/SprintPlan.md`
- If Linear MCP is connected, create tasks in Linear
- Task dependency graph (optional)
