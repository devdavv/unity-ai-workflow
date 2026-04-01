# /uw-cmd-plan — Planning Workflow

Break a feature or sprint into actionable tasks with clear acceptance criteria.

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
- **Skill** (which skill handles this task)

### 4. Sprint Structure
```markdown
## Sprint [N]: [Goal]
**Duration**: [X days/weeks]

| # | Task | Size | Depends On | Skill | Status |
|---|------|------|-----------|-------|--------|
| 1 | Create PlayerHealth SO | S | — | uw-scriptable-object-arch | pending |
| 2 | /uw-cmd-implement-feature: damage system (incl. game feel) | M | 1 | uw-game-feel-integrator | pending |
| 3 | /uw-cmd-implement-feature: health UI (incl. animations) | M | 1 | uw-ui-toolkit-binder | pending |
| 4 | Write health edge-case tests | S | 2 | uw-unity-test-runner | pending |
```

> Game feel is embedded inside each `/uw-cmd-implement-feature` task — not a separate row. If deferred, mark with `TODO(gamefeel)`.

### 5. Output
- Updated `docs/SprintPlan.md`
- If Linear MCP is connected, create tasks in Linear
- Task dependency graph (optional)

$ARGUMENTS
