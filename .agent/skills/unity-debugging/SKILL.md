---
name: Unity Debugging
description: Systematic 4-phase debugging framework for Unity projects with GameDebug wrapper generation.
---

# Unity Debugging

A structured debugging methodology for Unity projects. Also generates the project's `GameDebug` wrapper class with conditional compilation.

## Prerequisites
- Check if Unity MCP is available for `read_console` and `debug_request_context`.
- Read `docs/ProjectConfig.yaml` for current project state.

## 4-Phase Debugging Framework

### Phase 1: Gather Evidence
**Goal**: Understand the symptom without guessing at the cause.

1. **Read the console** — via Unity MCP `read_console` or ask the user to share errors.
2. **Reproduce the bug** — determine exact steps to trigger it.
3. **Identify scope** — is this a single script, a system interaction, or a timing issue?
4. **Check recent changes** — use `git log -5` to see what changed recently.

### Phase 2: Isolate
**Goal**: Narrow the problem to the smallest possible surface area.

1. **Comment out systems** — disable unrelated systems to isolate the issue.
2. **Add targeted GameDebug logs** — log state at key decision points, not everywhere.
3. **Check timing** — is this an `Awake()` vs `Start()` ordering issue?
4. **Check threading** — is Unity API being called from a background thread?

### Phase 3: Hypothesize & Test
**Goal**: Form a specific theory and test it.

1. State the hypothesis: "I believe X happens because Y."
2. Write a test that would **prove** the hypothesis.
3. Run the test. If it fails, the hypothesis is wrong — go back to Phase 2.
4. If it passes, move to Phase 4.

### Phase 4: Fix & Verify
**Goal**: Implement the fix and prevent regression.

1. Implement the minimal fix.
2. Re-run the test from Phase 3.
3. Run the full test suite to check for regressions.
4. Clean up any temporary debug logs.
5. Commit with a descriptive message explaining the root cause.

## GameDebug Wrapper

Generate this class during Phase 3 (Project Setup) to replace `Debug.Log`:

```csharp
using UnityEngine;
using System.Diagnostics;

public static class GameDebug
{
    public enum Category
    {
        Gameplay,
        UI,
        Network,
        Audio,
        Physics,
        AI,
        System
    }

    [Conditional("ENABLE_LOGS")]
    public static void Log(string message, Category category = Category.System)
    {
        UnityEngine.Debug.Log($"[{category}] {message}");
    }

    [Conditional("ENABLE_LOGS")]
    public static void LogWarning(string message, Category category = Category.System)
    {
        UnityEngine.Debug.LogWarning($"[{category}] {message}");
    }

    [Conditional("ENABLE_LOGS")]
    public static void LogError(string message, Category category = Category.System)
    {
        UnityEngine.Debug.LogError($"[{category}] {message}");
    }

    [Conditional("ENABLE_LOGS")]
    public static void Assert(bool condition, string message = "")
    {
        if (!condition)
            UnityEngine.Debug.LogError($"[ASSERT FAILED] {message}");
    }
}
```

### Setup
1. Add `ENABLE_LOGS` to Project Settings → Player → Scripting Define Symbols for **Development** builds.
2. Do NOT add it for Release/Production builds — all `GameDebug` calls are stripped automatically.

## Common Unity Debugging Pitfalls

| Symptom | Likely Cause | Solution |
|---------|-------------|----------|
| NullReferenceException in Awake | Execution order | Use `[DefaultExecutionOrder]` or cache in Start |
| MissingReferenceException | Destroyed object accessed | Check `.gameObject != null` or use lifecycle events |
| Works in Editor, fails in build | `#if UNITY_EDITOR` code | Check for editor-only APIs |
| Runs fine once, breaks on replay | State not reset in OnDisable/OnDestroy | Clean up subscriptions and state |
| Physics behaves differently | FixedUpdate vs Update | Move physics code to FixedUpdate |

## MCP Integration
- **`read_console`**: Capture error logs from Unity Editor.
- **`debug_request_context`**: Get stack traces and object state.
- **`run_tests`**: Verify the fix doesn't break other tests.
