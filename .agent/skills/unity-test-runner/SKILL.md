---
name: Unity Test Runner
description: Generates EditMode and PlayMode tests using NUnit, mapped from GDD Gherkin scenarios.
---

# Unity Test Runner

Generates NUnit tests for Unity's Test Framework. Maps GDD Gherkin scenarios (`Given/When/Then`) directly to test methods.

## Prerequisites
- Read `docs/GDD_Template.md` for Gherkin scenarios to convert to tests.
- Ensure a `Tests.asmdef` exists for the feature (create with `unity-feature-scaffold` if needed).
- Check if Unity MCP is available for `run_tests` / `get_test_job`.

## Gherkin → Test Mapping

### GDD Scenario
```gherkin
Scenario: Player takes damage
  Given a player with 100 health
  When the player takes 30 damage
  Then the player health should be 70
  And a damage feedback event should fire
```

### Generated EditMode Test
```csharp
using NUnit.Framework;

[TestFixture]
public class PlayerHealthTests
{
    [Test]
    public void TakeDamage_ReducesHealthByDamageAmount()
    {
        // Given
        var health = new HealthSystem(maxHealth: 100);

        // When
        health.TakeDamage(30);

        // Then
        Assert.AreEqual(70, health.CurrentHealth);
    }

    [Test]
    public void TakeDamage_FiresDamageEvent()
    {
        // Given
        var health = new HealthSystem(maxHealth: 100);
        bool eventFired = false;
        health.OnDamaged += _ => eventFired = true;

        // When
        health.TakeDamage(30);

        // Then
        Assert.IsTrue(eventFired);
    }
}
```

### Generated PlayMode Test
```csharp
using System.Collections;
using NUnit.Framework;
using UnityEngine;
using UnityEngine.TestTools;

public class PlayerHealthPlayModeTests
{
    [UnityTest]
    public IEnumerator TakeDamage_TriggersVisualFeedback()
    {
        // Given
        var go = new GameObject("Player");
        var controller = go.AddComponent<PlayerController>();
        yield return null; // Wait for Awake/Start

        // When
        controller.TakeDamage(30);
        yield return new WaitForSeconds(0.1f);

        // Then
        Assert.AreEqual(70, controller.Health);

        // Cleanup
        Object.Destroy(go);
    }
}
```

## Test Organization

```
Tests/
├── EditMode/
│   ├── {Feature}Tests.cs       # Pure logic tests
│   └── {Feature}DataTests.cs   # ScriptableObject data tests
└── PlayMode/
    ├── {Feature}PlayTests.cs   # MonoBehaviour lifecycle tests
    └── {Feature}IntegrationTests.cs  # Multi-system tests
```

## Naming Convention

`MethodName_StateUnderTest_ExpectedBehavior`

Examples:
- `TakeDamage_WhenHealthIsZero_TriggersDeathEvent`
- `Shoot_WhenOutOfAmmo_ReturnsFalse`
- `Jump_WhenGrounded_AppliesUpwardForce`

## MCP Integration
- **`run_tests`**: Execute specific test suites or individual tests.
- **`get_test_job`**: Check test results asynchronously.
- **`read_console`**: Capture test output and error logs.

If Unity MCP is not available, instruct the user: "Please run tests via Unity Editor → Window → General → Test Runner."
