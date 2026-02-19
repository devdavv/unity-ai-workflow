# /debug — Debugging Workflow

Apply the 4-phase debugging framework to identify and fix bugs.

## Agent
Load **QA Tester** from `.agent/agents/qa-tester.md`.
Load `unity-debugging` skill from `.agent/skills/unity-debugging/SKILL.md`.

## Phase 1: Gather Evidence
1. **Read the error** — Via Unity MCP `read_console` or user-provided logs.
2. **Reproduce** — What are the exact steps to trigger the bug?
3. **Scope** — Single script? System interaction? Timing?
4. **Recent changes** — Check `git log -5` for what changed.

## Phase 2: Isolate
1. **Narrow the search** — Comment out systems to find the culprit.
2. **Add targeted logs** — Use `GameDebug.Log` at decision points.
3. **Common causes checklist**:
   - Execution order (`Awake` vs `Start` vs `OnEnable`)
   - Null reference (destroyed object, unassigned field)
   - Threading (Unity API on background thread)
   - State not reset (`OnDisable`/`OnDestroy` cleanup)
   - Physics timing (`Update` vs `FixedUpdate`)

## Phase 3: Hypothesize & Test
1. State the hypothesis: *"I believe [X] happens because [Y]."*
2. Write a test that proves or disproves the hypothesis.
3. Run the test. If it fails → back to Phase 2 with new info.

## Phase 4: Fix & Verify
1. Implement the **minimal** fix.
2. Run the specific test from Phase 3.
3. Run the **full test suite** for regressions.
4. Clean up temporary debug logs.
5. Commit with: `fix({scope}): {root cause description}`

## Common Unity Pitfalls

| Symptom | Common Cause |
|---------|-------------|
| NullRef in Awake | Execution order — use `[DefaultExecutionOrder]` |
| MissingComponent | Destroyed object still referenced |
| Works in Editor, fails in build | Editor-only API usage |
| Intermittent failure | Race condition or timing |
| Physics jitter | Wrong update loop |

## Output
- Root cause identified and documented
- Fix implemented with test coverage
- Regression suite passing

$ARGUMENTS
