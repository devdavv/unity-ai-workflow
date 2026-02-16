---
name: Unity Test Runner
description: Generates EditMode and PlayMode tests using NUnit, mapped from GDD Gherkin scenarios. Use when writing tests, validating features, or ensuring code quality. Triggers on requests like "write tests for this", "test the health system", "add unit tests", "verify this works", or any test-related task. Maps Given/When/Then scenarios directly to test methods.
---

# Unity Test Runner

Generate NUnit tests mapped from GDD Gherkin scenarios.

## Before You Start
1. Read `docs/GDD.md` for Gherkin scenarios to convert.
2. Ensure a `Tests.asmdef` exists (create with `unity-feature-scaffold` if needed).

## Gherkin → Test Mapping

```gherkin
Scenario: Player takes damage
  Given a player with 100 health
  When the player takes 30 damage
  Then the player health should be 70
```

Becomes:
```csharp
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
```

## Test Types
- **EditMode** (`Tests/EditMode/`): Pure logic, no MonoBehaviour lifecycle.
- **PlayMode** (`Tests/PlayMode/`): Needs `Awake`/`Start`/`Update`, uses `[UnityTest]` + `IEnumerator`.

## Naming Convention
`MethodName_StateUnderTest_ExpectedBehavior`

Examples: `TakeDamage_WhenHealthIsZero_TriggersDeathEvent`, `Jump_WhenGrounded_AppliesUpwardForce`

## MCP
- Use `run_tests` / `get_test_job` if Unity MCP is available.
- Otherwise: "Run tests via Unity Editor → Window → General → Test Runner."
