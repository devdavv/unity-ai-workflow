---
description: Write and run tests for an implemented feature, mapping from GDD scenarios.
---

# /test — Testing Workflow

Write comprehensive tests for a feature and verify they all pass.

## Agent
**QA Tester**

## Steps

### 1. Identify Test Targets
- Which **feature** or **system** needs testing?
- Find the relevant **GDD Gherkin scenarios** for this feature.
- Check if tests already exist — avoid duplicates.

### 2. Generate Tests
Use `unity-test-runner` skill:
- **EditMode tests** for pure logic (no MonoBehaviour lifecycle).
- **PlayMode tests** for systems that need Update/Awake/Start.
- Map each Gherkin scenario to one or more test methods.

### 3. Edge Cases
Go beyond the GDD scenarios — test boundaries:
- What happens with **zero** or **negative** values?
- What happens at **max capacity**?
- What happens with **null** inputs?
- What happens with **rapid repeated** calls?

### 4. Run Tests
- Via Unity MCP `run_tests` if available.
- Capture results and report:
  ```
  ✅ 12/14 tests passed
  ❌ TakeDamage_WhenHealthIsZero_DoesNotGoNegative — FAILED
  ❌ Shoot_WhenDisconnected_QueuesAction — FAILED
  ```

### 5. Fix Failures
- If tests fail due to **implementation bugs**, switch to `/debug` workflow.
- If tests fail due to **test errors**, fix the test.
- Re-run until all pass.

### 6. Output
- Complete test suite with passing results
- Test coverage summary
- Any discovered bugs filed (via Linear MCP if available)
