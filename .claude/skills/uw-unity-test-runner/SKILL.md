---
name: uw-unity-test-runner
description: Generates EditMode and PlayMode tests using NUnit, mapped from GDD Gherkin scenarios. Use when writing tests, validating features, ensuring code quality, or verifying behavior after changes. Triggers on requests like "write tests for this", "test the health system", "add unit tests", "verify this works", "add PlayMode tests", "test this feature", "write integration tests", "make sure this doesn't break", "add test coverage", "TDD this feature", or any task involving test creation, test-driven development, or quality assurance. Maps Given/When/Then scenarios directly to test methods using NUnit assertions.
---

# Unity Test Runner

Generate NUnit tests mapped from GDD Gherkin scenarios.

## Before You Start

1. Read `docs/GDD.md` for Gherkin scenarios (Given/When/Then) to convert into tests.
2. Ensure a `Tests.asmdef` exists for the feature — create with `uw-unity-feature-scaffold` if needed.
3. Read `docs/CODING_STANDARDS.md` for async patterns (`Awaitable` + `CancellationToken`) used in PlayMode tests.
4. Read `docs/NAMING_CONVENTIONS.md` for test class and file naming.
5. Check `docs/ProjectConfig.yaml -> mcp.unity_mcp` — if `true`, use `run_tests` / `get_test_job` to execute tests. Otherwise, instruct the user to run via Unity Editor (Window -> General -> Test Runner).

## Test Types

| Type | Location | When to Use | Runner |
|------|----------|-------------|--------|
| **EditMode** | `Tests/EditMode/` | Pure logic — no MonoBehaviour lifecycle, no scene required | `[Test]` with NUnit |
| **PlayMode** | `Tests/PlayMode/` | Needs `Awake`/`Start`/`Update`, scene loading, physics, or coroutines | `[UnityTest]` + `IEnumerator` |

**Default to EditMode** whenever possible — they're faster, more reliable, and easier to debug. Only use PlayMode when the code under test depends on Unity's runtime lifecycle.

## Gherkin to Test Mapping

Each Gherkin scenario from the GDD becomes one test method. The Given/When/Then structure maps directly to Arrange/Act/Assert:

```gherkin
Scenario: Player takes damage
  Given a player with 100 health
  When the player takes 30 damage
  Then the player health should be 70
```

```csharp
using NUnit.Framework;

namespace {RootNamespace}.{FeatureName}.Tests
{
    [TestFixture]
    public class HealthSystemTests
    {
        [Test]
        public void TakeDamage_WithFullHealth_ReducesHealthByDamageAmount()
        {
            // Given
            var health = new HealthSystem(maxHealth: 100);

            // When
            health.TakeDamage(30);

            // Then
            Assert.AreEqual(70, health.CurrentHealth);
        }
    }
}
```

## Naming Convention

```
MethodName_StateUnderTest_ExpectedBehavior
```

| Part | Purpose | Example |
|------|---------|---------|
| **MethodName** | The method being tested | `TakeDamage` |
| **StateUnderTest** | The precondition or context | `WhenHealthIsZero` |
| **ExpectedBehavior** | What should happen | `TriggersDeathEvent` |

Examples:
- `TakeDamage_WhenHealthIsZero_TriggersDeathEvent`
- `Jump_WhenGrounded_AppliesUpwardForce`
- `AddItem_WhenInventoryFull_ReturnsFalse`
- `SetHealth_WithNegativeValue_ClampsToZero`

## Setup and Teardown

Use NUnit lifecycle attributes to share setup/cleanup across tests in a fixture:

```csharp
[TestFixture]
public class WeaponSystemTests
{
    private WeaponSystem _weapon;
    private HealthSystem _target;

    [OneTimeSetUp]
    public void OneTimeSetUp()
    {
        // Runs once before ALL tests in this fixture.
        // Use for expensive setup shared across all tests (loading data, etc.)
    }

    [SetUp]
    public void SetUp()
    {
        // Runs before EACH test — fresh state every time.
        _weapon = new WeaponSystem(damage: 25, fireRate: 0.5f);
        _target = new HealthSystem(maxHealth: 100);
    }

    [TearDown]
    public void TearDown()
    {
        // Runs after EACH test — clean up subscriptions, reset state.
    }

    [OneTimeTearDown]
    public void OneTimeTearDown()
    {
        // Runs once after ALL tests in this fixture.
    }

    [Test]
    public void Attack_WithLoadedWeapon_DealsDamageToTarget()
    {
        _weapon.Attack(_target);
        Assert.AreEqual(75, _target.CurrentHealth);
    }
}
```

**When to use which:**
- `[SetUp]` / `[TearDown]` — most tests need these. Fresh state per test prevents test pollution.
- `[OneTimeSetUp]` / `[OneTimeTearDown]` — expensive shared resources (loading ScriptableObject data, building test fixtures). Avoid for mutable state.

## Common Assertion Patterns

```csharp
// Equality
Assert.AreEqual(expected, actual);
Assert.AreNotEqual(notExpected, actual);

// Boolean
Assert.IsTrue(player.IsAlive);
Assert.IsFalse(inventory.IsFull);

// Null
Assert.IsNull(result);
Assert.IsNotNull(spawnedObject);

// Float comparison (with tolerance for floating point)
Assert.AreEqual(0.5f, healthPercent, 0.001f);

// Collection
Assert.Contains(item, inventory.Items);
Assert.AreEqual(3, inventory.Items.Count);
Assert.IsEmpty(inventory.Items);

// Exception
Assert.Throws<ArgumentException>(() => weapon.SetDamage(-1));

// String
StringAssert.Contains("damage", logMessage);

// Range (custom — useful for game values)
Assert.GreaterOrEqual(health.CurrentHealth, 0);
Assert.LessOrEqual(health.CurrentHealth, health.MaxHealth);
```

## Edge Case Patterns

Every feature should be tested against these common edge cases. Map them from GDD scenarios when available, or add defensively:

```csharp
// Zero and negative values
[Test]
public void TakeDamage_WithZeroDamage_HealthUnchanged()
{
    var health = new HealthSystem(maxHealth: 100);
    health.TakeDamage(0);
    Assert.AreEqual(100, health.CurrentHealth);
}

[Test]
public void TakeDamage_WithNegativeDamage_ClampsToZero()
{
    var health = new HealthSystem(maxHealth: 100);
    health.TakeDamage(-10);
    Assert.AreEqual(100, health.CurrentHealth);
}

// Boundary values
[Test]
public void TakeDamage_ExactlyMaxHealth_HealthReachesZero()
{
    var health = new HealthSystem(maxHealth: 100);
    health.TakeDamage(100);
    Assert.AreEqual(0, health.CurrentHealth);
}

// Overflow / exceed max
[Test]
public void Heal_BeyondMaxHealth_ClampsToMax()
{
    var health = new HealthSystem(maxHealth: 100);
    health.TakeDamage(50);
    health.Heal(999);
    Assert.AreEqual(100, health.CurrentHealth);
}

// Rapid repeated calls
[Test]
public void TakeDamage_CalledMultipleTimes_AccumulatesCorrectly()
{
    var health = new HealthSystem(maxHealth: 100);
    health.TakeDamage(10);
    health.TakeDamage(20);
    health.TakeDamage(30);
    Assert.AreEqual(40, health.CurrentHealth);
}

// State after death / already dead
[Test]
public void TakeDamage_WhenAlreadyDead_NoFurtherDamage()
{
    var health = new HealthSystem(maxHealth: 100);
    health.TakeDamage(100);
    health.TakeDamage(50);
    Assert.AreEqual(0, health.CurrentHealth);
}

// Null / missing references (when applicable)
[Test]
public void Attack_WithNullTarget_DoesNotThrow()
{
    var weapon = new WeaponSystem(damage: 25);
    Assert.DoesNotThrow(() => weapon.Attack(null));
}
```

## PlayMode Tests

PlayMode tests run inside Unity's runtime and have access to MonoBehaviour lifecycle, physics, and scene loading. Use `[UnityTest]` which returns `IEnumerator`:

```csharp
using System.Collections;
using NUnit.Framework;
using UnityEngine;
using UnityEngine.TestTools;

namespace {RootNamespace}.{FeatureName}.Tests
{
    [TestFixture]
    public class PlayerMovementPlayModeTests
    {
        private GameObject _playerObject;
        private PlayerMovement _movement;

        [SetUp]
        public void SetUp()
        {
            _playerObject = new GameObject("TestPlayer");
            _playerObject.AddComponent<Rigidbody>();
            _movement = _playerObject.AddComponent<PlayerMovement>();
        }

        [TearDown]
        public void TearDown()
        {
            Object.DestroyImmediate(_playerObject);
        }

        [UnityTest]
        public IEnumerator Jump_WhenGrounded_MovesPlayerUpward()
        {
            // Given — player at ground level
            float startY = _playerObject.transform.position.y;

            // When — jump and wait for physics
            _movement.Jump();
            yield return new WaitForFixedUpdate();
            yield return new WaitForFixedUpdate();

            // Then — player should be higher
            Assert.Greater(_playerObject.transform.position.y, startY);
        }

        [UnityTest]
        public IEnumerator TakeDamage_WhenPlayerDies_DisablesMovement()
        {
            // Given — player alive
            Assert.IsTrue(_movement.enabled);

            // When
            _movement.GetComponent<HealthSystem>().TakeDamage(999);
            yield return null; // Wait one frame for death logic

            // Then
            Assert.IsFalse(_movement.enabled);
        }
    }
}
```

**PlayMode tips:**
- `yield return null` — wait one frame (for Update logic to run).
- `yield return new WaitForFixedUpdate()` — wait for physics step.
- `yield return new WaitForSeconds(0.5f)` — wait real time (avoid in CI — use frame counts instead).
- Always `DestroyImmediate` test objects in `[TearDown]` to prevent leaks.

## Testing with DI (di-first)

When `architecture_pattern: "di-first"`, test by constructing objects directly with mock dependencies — no container needed in tests:

```csharp
[Test]
public void ScoreController_OnHit_AddsPoints()
{
    // Create mocks / stubs
    var mockAudio = new MockAudioService();
    var scoreController = new ScoreController(mockAudio);

    scoreController.OnHit(pointValue: 10);

    Assert.AreEqual(10, scoreController.TotalScore);
}
```

Interface-based DI makes testing simple because every dependency is swappable. See `uw-dependency-injection` for the interface-first pattern.

## After Writing Tests

- **Run tests:** Use Unity MCP `run_tests` if available, otherwise instruct user to use Test Runner window.
- **Code review:** Use `uw-code-review` to verify test coverage meets GDD Gherkin scenarios.
- **Debug failures:** Use `uw-unity-debugging` for systematic diagnosis of test failures.

## Rules

- Every feature needs at least one test file — use `uw-unity-feature-scaffold` to create the test assembly.
- Test naming follows `MethodName_StateUnderTest_ExpectedBehavior` convention.
- Map all GDD Gherkin scenarios to tests — each scenario = one `[Test]` method.
- Default to EditMode tests. Only use PlayMode when testing MonoBehaviour lifecycle, physics, or scene loading.
- Use `[SetUp]` for fresh state per test — avoid shared mutable state between tests.
- `DestroyImmediate` all created GameObjects in PlayMode `[TearDown]`.
- Test edge cases: zero, negative, boundary, overflow, null, rapid repeated calls, already-dead/invalid state.
- All test code must live inside a `.Tests.asmdef` (per `NAMING_CONVENTIONS.md`).
- Use `Assert.AreEqual(expected, actual, tolerance)` for float comparisons — never exact equality.
- If `ProjectConfig.yaml -> mcp.unity_mcp` is `true`, use `run_tests` to execute. Otherwise, direct user to Test Runner window.
