# Coding Standards

> Refined collaboratively during Phase 1 (Pre-Production). These are starting defaults — customize to your team's preferences.

## Formatting

| Rule | Standard |
|------|----------|
| **Braces** | K&R style (opening brace on same line) |
| **Indentation** | 4 spaces (no tabs) |
| **Line Length** | Soft limit 120 characters |
| **Blank Lines** | One blank line between methods, two between sections |

## Class Structure Order

```csharp
public class ExampleBehaviour : MonoBehaviour
{
    // 1. Constants
    private const float DEFAULT_SPEED = 5f;

    // 2. Static fields
    private static int s_instanceCount;

    // 3. Serialized fields (Inspector)
    [Header("Movement")]
    [SerializeField] private float _moveSpeed = DEFAULT_SPEED;
    [SerializeField] private float _jumpForce = 10f;

    // 4. Private fields
    private Rigidbody _rb;
    private bool _isGrounded;

    // 5. Properties
    public float MoveSpeed => _moveSpeed;

    // 6. Unity Lifecycle (in execution order)
    private void Awake() {
        _rb = GetComponent<Rigidbody>();
    }

    private void OnEnable() { }
    private void Start() { }
    private void Update() { }
    private void FixedUpdate() { }
    private void LateUpdate() { }
    private void OnDisable() { }
    private void OnDestroy() { }

    // 7. Public methods
    public void Jump() { }

    // 8. Private methods
    private void ApplyMovement() { }

    // 9. Event handlers
    private void OnDamageReceived(int amount) { }
}
```

## Serialization Rules

```csharp
// ✅ CORRECT
[SerializeField] private float _moveSpeed = 5f;
[SerializeField] private GameObject _projectilePrefab;

// ❌ WRONG — never public for Inspector
public float moveSpeed = 5f;

// ❌ WRONG — never [HideInInspector] on public
[HideInInspector] public float speed;
```

## Null Safety

```csharp
// ✅ CORRECT — TryGetComponent
if (TryGetComponent<Rigidbody>(out var rb)) {
    rb.AddForce(Vector3.up);
}

// ✅ CORRECT — null check before use
if (_targetTransform != null) {
    transform.LookAt(_targetTransform);
}

// ❌ WRONG — crash-prone
GetComponent<Rigidbody>().AddForce(Vector3.up);
```

## String Usage

```csharp
// ✅ CORRECT — interpolation
GameDebug.Log($"Player {playerName} took {damage} damage");

// ❌ WRONG — concatenation (allocates)
GameDebug.Log("Player " + playerName + " took " + damage + " damage");
```

## Async Patterns

```csharp
// ✅ CORRECT — Awaitable (Unity 6+)
private async Awaitable LoadLevelAsync() {
    await SceneManager.LoadSceneAsync("Level1");
}

// ❌ AVOID — Coroutines for logic (use only for animations/tweens if needed)
private IEnumerator LoadLevel() {
    yield return SceneManager.LoadSceneAsync("Level1");
}
```
