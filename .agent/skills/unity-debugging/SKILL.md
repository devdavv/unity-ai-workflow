---
name: Unity Debugging
description: Systematic 4-phase debugging framework for Unity projects with GameDebug wrapper generation. Use when encountering any bug, test failure, or unexpected behavior, before proposing fixes. Triggers on requests like "this doesn't work", "there's a bug", "it crashes when", "fix this error", or any debugging task. Four-phase framework (root cause investigation, pattern analysis, hypothesis testing, implementation) that ensures understanding before attempting solutions.
---

# Unity Debugging

4-phase debugging framework. Also generates the `GameDebug` wrapper class.

## 4 Phases

### Phase 1: Gather Evidence
1. Read Unity console (`read_console` via MCP or ask user for errors).
2. Reproduce — exact steps to trigger.
3. Scope — single script, system interaction, or timing?
4. Check `git log -5` for recent changes.

### Phase 2: Isolate
1. Comment out systems to narrow the culprit.
2. Add targeted `GameDebug.Log` at decision points.
3. Check common causes:
   - Execution order (`Awake` vs `Start` vs `OnEnable`)
   - Null reference (destroyed object, unassigned field)
   - Unity API called from background thread
   - State not reset in `OnDisable`/`OnDestroy`
   - Physics in `Update` instead of `FixedUpdate`

### Phase 3: Hypothesize & Test
1. State: *"I believe X happens because Y."*
2. Write a test proving/disproving the hypothesis.
3. If test fails → back to Phase 2.

### Phase 4: Fix & Verify
1. Implement minimal fix.
2. Re-run the hypothesis test.
3. Run full test suite for regressions.
4. Clean up temp debug logs.
5. Commit: `fix({scope}): {root cause description}`

## GameDebug Wrapper

Generate during project setup. Uses `[Conditional("ENABLE_LOGS")]` so all calls are stripped in release builds.

```csharp
using System.Diagnostics;

public static class GameDebug
{
    [Conditional("ENABLE_LOGS")]
    public static void Log(string message) =>
        UnityEngine.Debug.Log($"[Game] {message}");

    [Conditional("ENABLE_LOGS")]
    public static void LogWarning(string message) =>
        UnityEngine.Debug.LogWarning($"[Game] {message}");

    [Conditional("ENABLE_LOGS")]
    public static void LogError(string message) =>
        UnityEngine.Debug.LogError($"[Game] {message}");
}
```

Add `ENABLE_LOGS` to Scripting Define Symbols for Development builds only.
