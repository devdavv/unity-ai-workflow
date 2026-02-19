# Unity AI Workflow — Global Constitution

> **This file is read by ALL agents before every task. It is NON-NEGOTIABLE.**
> Rules can only be relaxed via `ProjectConfig.yaml` flags or inline `// OVERRIDE:` comments.

---

## 1. Engine & Platform

| Rule | Standard | Override |
|------|----------|---------|
| **Minimum Version** | Unity 6.2+ | `ProjectConfig.yaml → unity_version` |
| **Render Pipeline** | URP | `ProjectConfig.yaml → render_pipeline` |
| **Scripting Backend** | IL2CPP | `ProjectConfig.yaml → scripting_backend` |
| **Target Platforms** | Read from ProjectConfig | — |

---

## 2. Core Technology Choices

| Rule | Standard | Override |
|------|----------|---------|
| **UI System** | UI Toolkit | `ProjectConfig.yaml → ui_system` (UIToolkit / UGUI / Mixed) |
| **Input** | New Input System only. **Never** legacy `Input.GetKey*`, `Input.GetAxis*` | — |
| **Async** | `Awaitable` (Unity 6 native) | `ProjectConfig.yaml → async_pattern` (UniTask / Coroutines) |
| **Networking** | None by default | `ProjectConfig.yaml → networking` (NGO / Mirror / Photon / Custom) |
| **Visual Scripting** | **Forbidden** for logic. Shader Graph & VFX Graph are fine | — |
| **ECS/DOTS** | Off by default | `ProjectConfig.yaml → use_ecs_dots` |

---

## 3. Code Quality

### Serialization
```csharp
// ✅ CORRECT
[SerializeField] private float _moveSpeed = 5f;

// ❌ WRONG — never expose fields as public for Inspector
public float moveSpeed = 5f;
```

### Performance
- **Cache ALL component references** in `Awake()` or `Start()`.
- **Never** use `GetComponent<T>()`, `FindObjectOfType<T>()`, or `GameObject.Find()` inside `Update()`, `FixedUpdate()`, or `LateUpdate()`.
- Prefer `TryGetComponent<T>()` over `GetComponent<T>()` when the component may not exist.

### Thread Safety
- **Never** call Unity API from background threads.
- If using async background work, always marshal results back to the main thread via `Awaitable.MainThreadAsync()` or Unity's `SynchronizationContext`.

### Debugging
- Use the project's **`GameDebug`** wrapper class with conditional compilation:
```csharp
// ✅ CORRECT — stripped from release builds
GameDebug.Log("Player spawned", GameDebug.Category.Gameplay);

// ❌ WRONG — ships in release, costs performance
Debug.Log("Player spawned");
```
- The `GameDebug` class is generated during Phase 3 (Project Setup) using the `unity-debugging` skill.

---

## 4. Documentation Standards

### XML Comments
- `/// <summary>` **only on specific public API methods** that warrant explanation.
- **Never** put XML summaries on classes. Classes should be self-descriptive via naming.
- **Never** put XML summaries on private methods unless they contain truly non-obvious logic.

```csharp
// ✅ CORRECT
public class PlayerController : MonoBehaviour { ... }

/// <summary>
/// Applies knockback force with directional influence.
/// Force is scaled by the attacker's damage multiplier.
/// </summary>
public void ApplyKnockback(Vector3 direction, float force) { ... }

// ❌ WRONG — don't describe the obvious
/// <summary>
/// The player controller class that controls the player.
/// </summary>
public class PlayerController : MonoBehaviour { ... }
```

### Coding Standards
- Follow `docs/CODING_STANDARDS.md` for formatting (K&R braces, 4-space indent, etc.).
- Follow `docs/NAMING_CONVENTIONS.md` for naming (refined collaboratively during pre-production).
- Follow `docs/GIT_CONVENTIONS.md` for commits (`type(scope): description`), branch naming, and PR format.

---

## 5. Unity-Specific Constraints

| Rule | Detail |
|------|--------|
| **Meta Files** | **Never** create, modify, or delete `.meta` files. Let Unity manage them. |
| **Version Check** | Always read `ProjectConfig.yaml` for package versions before referencing any API. An API that exists in v3.0 may not exist in v2.1. |
| **Assembly Definitions** | All feature code must live inside an `.asmdef`. No scripts in the root `Assets/Scripts/` without an assembly definition. |
| **ScriptableObjects** | Always use `[CreateAssetMenu]` attribute. Never hardcode data that belongs in a SO. |

---

## 6. AI Behavior Rules

| Rule | Detail |
|------|--------|
| **Context Loading** | Before starting work, read `AGENTS.md` to determine which agent persona and skills to load. **Announce** the loaded context to the user. |
| **Confirmation** | Always ask before proceeding with destructive actions or major decisions, unless `auto_confirm: true` in ProjectConfig. |
| **Workflow Proactivity** | When a task aligns with a known workflow (`/brainstorm`, `/plan`, `/create`, `/test`, `/debug`, `/polish`, `/setup-project`), **proactively suggest** it. Don't wait to be asked. |
| **MCP Awareness** | Check if Unity MCP / GitHub MCP / Linear MCP are available. Use them when they would improve the workflow, but never assume they are connected — always verify first. |
| **Error Recovery** | If a compilation error occurs, read the Unity console (via MCP if available) before attempting fixes. Never guess at errors. |
| **Prompt Logging** | When `prompt_logging: true` in `ProjectConfig.yaml`, append an entry to `docs/PromptLog.md` after every response. Format: date/time, user's exact prompt, ≤200-word factual summary of what was done. See `AGENTS.md → Prompt Logging` for the full format. |

---

## 7. Rule Override Mechanism

---

## 8. Dev Mode — AI Behavior by Mode

> Read `ai_mode` from `ProjectConfig.yaml` at the start of every session.
> This controls AI autonomy for the **entire project**.

| Mode | Description | When AI Confirms | Explanation Level |
|------|-------------|-----------------|-------------------|
| **assistant** | User drives. AI assists, documents, recommends. | Every action — always wait | Verbose. Always explain reasoning and tradeoffs. |
| **mix** | Collaborative. AI suggests; user confirms key decisions. **(default)** | Complex tasks, new features, architecture changes | Moderate. Explain on request. |
| **automatic** | AI works autonomously after initial onboarding Q&A. | Destructive actions only | Minimal. Announce actions but don't wait. |

### Automatic Mode — Onboarding Requirement
Before proceeding autonomously, ask ALL of the following upfront:
- One-sentence game pitch + core loop
- Target platform(s) and genre
- 1-3 reference games
- Rough feature list (can be expanded)
- Preferred packages (tweening, networking, etc.)
- MCP availability (Unity, GitHub, Linear, Notion)

Once answered, proceed through phases and features with no interruptions except for destructive operations (file deletion, force pushes, schema drops).

### Assistant Mode — Waiting Requirement
Never auto-proceed. Always surface options with tradeoffs. Always wait for the user to explicitly initiate the next step. The user builds; the AI advises.

---

Rules can be relaxed in two ways:

### A. ProjectConfig Flags
```yaml
# In docs/ProjectConfig.yaml
rule_overrides:
  allow_ugui: true             # Allows UGUI alongside UI Toolkit
  allow_coroutines_for_logic: true  # Allows coroutines for game logic
  prototype_mode: true         # See table below for exact relaxations
```

### B. Inline Override
```csharp
// OVERRIDE: allow_public_field — prototype quick iteration
public float moveSpeed = 5f;
```

### Prototype Mode — Explicit Relaxations

When `prototype_mode: true`, the following rules are relaxed. **Everything else stays enforced.**

| Rule | Normal Mode | Prototype Mode |
|------|------------|----------------|
| Assembly Definitions | Required for all feature code | Optional — scripts can live without `.asmdef` |
| Field Visibility | `[SerializeField] private` only | `public` fields allowed for rapid Inspector tuning |
| ScriptableObjects for data | Required — never hardcode data | Inline constants/magic numbers allowed |
| XML Documentation | On public API methods | Optional — skip docs for speed |
| Interface Segregation | Interfaces required for testability | Direct concrete references allowed |
| Feature Folders | Required feature-based organization | Flat `Scripts/` folder allowed |

**These rules are NEVER relaxed, even in prototype mode:**

| Rule | Always Enforced | Reason |
|------|-----------------|--------|
| **GameDebug** (never raw `Debug.Log`) | ✅ Always | Costs nothing to use correctly |
| **Cache component references** | ✅ Always | Prevents hard-to-find perf bugs |
| **No `GetComponent` in Update** | ✅ Always | Performance habit |
| **New Input System only** | ✅ Always | Legacy input doesn't port |
| **Never touch `.meta` files** | ✅ Always | Corrupts project |

> **Rule of thumb**: Prototype mode relaxes *architecture* but never *practices*. You can skip structure, but you can't skip quality.
