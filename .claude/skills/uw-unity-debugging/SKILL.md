---
name: uw-unity-debugging
description: Systematic 4-phase debugging framework for Unity projects with GameDebug wrapper generation. Use when encountering any bug, test failure, or unexpected behavior, before proposing fixes. Triggers on requests like "this doesn't work", "there's a bug", "it crashes when", "fix this error", "why is this happening", "null reference exception", "test is failing", "unexpected behavior", "something broke", or any debugging task. Four-phase framework (gather evidence, isolate, hypothesize, fix) that ensures understanding before attempting solutions.
---

# Unity Debugging

4-phase debugging framework. Also generates the `GameDebug` wrapper class.

## Before You Start

1. Check if `GameDebug.cs` already exists in the project (search for `class GameDebug`). If not, generate it using the template below during project setup.
2. Read `docs/ProjectConfig.yaml -> mcp.unity_mcp` — if `true`, use `read_console` to read Unity console output.
3. Read `docs/CODING_STANDARDS.md` for async patterns — bugs in async code often involve missing `CancellationToken` or unhandled `OperationCanceledException`.

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
3. If test fails -> back to Phase 2.

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

## After Debugging

- **Write regression test:** Use `uw-unity-test-runner` to add a test that catches the bug if it reappears.
- **Code review:** Use `uw-code-review` to verify the fix doesn't introduce new issues.
- **Visualize debug data:** Use `uw-unity-editor-tools` to add Gizmos or Handles for spatial debugging (visualize raycast paths, trigger volumes, etc.).

## Rules

- Always follow the 4-phase framework — never jump to a fix before gathering evidence and forming a hypothesis.
- Use `GameDebug` wrapper, never raw `Debug.Log` / `Debug.LogWarning` / `Debug.LogError`.
- Clean up all temporary debug logs after the fix is verified.
- Write a regression test for every bug fix.
- If `ProjectConfig.yaml -> mcp.unity_mcp` is `true`, use `read_console` for console output and `run_tests` to verify fixes.
