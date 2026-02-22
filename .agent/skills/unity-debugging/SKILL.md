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

Generate during project setup. Uses `[Conditional("ENABLE_LOGS")]` so all calls are stripped in release builds. Includes `[CallerFilePath]`, `[CallerMemberName]`, and `[CallerLineNumber]` for automatic context — no need to manually describe where a log came from.

```csharp
using System.Diagnostics;
using System.Runtime.CompilerServices;

public enum LogTopic { General, Gameplay, Audio, UI, Network, Physics, AI }

public static class GameDebug
{
    [Conditional("ENABLE_LOGS")]
    public static void Log(
        string message,
        LogTopic topic = LogTopic.General,
        [CallerFilePath] string file = "",
        [CallerMemberName] string member = "",
        [CallerLineNumber] int line = 0) =>
        UnityEngine.Debug.Log(Format(topic, message, file, member, line));

    [Conditional("ENABLE_LOGS")]
    public static void LogWarning(
        string message,
        LogTopic topic = LogTopic.General,
        [CallerFilePath] string file = "",
        [CallerMemberName] string member = "",
        [CallerLineNumber] int line = 0) =>
        UnityEngine.Debug.LogWarning(Format(topic, message, file, member, line));

    [Conditional("ENABLE_LOGS")]
    public static void LogError(
        string message,
        LogTopic topic = LogTopic.General,
        [CallerFilePath] string file = "",
        [CallerMemberName] string member = "",
        [CallerLineNumber] int line = 0) =>
        UnityEngine.Debug.LogError(Format(topic, message, file, member, line));

    private static string Format(
        LogTopic topic, string msg, string file, string member, int line) {
        var fileName = System.IO.Path.GetFileNameWithoutExtension(file);
        return $"[{topic}] {fileName}.{member}:{line} — {msg}";
    }
}
```

**Usage:** `GameDebug.Log("Player spawned", LogTopic.Gameplay);`
**Output:** `[Gameplay] PlayerController.Start:42 — Player spawned`

Add `ENABLE_LOGS` to Scripting Define Symbols for Development builds only. Customize `LogTopic` enum per project.
