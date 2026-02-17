# Git Conventions

> All agents must follow these conventions when creating commits, branches, or PRs.

---

## Commit Format

Use [Conventional Commits](https://www.conventionalcommits.org/) with scope:

```
type(scope): description
```

### Types

| Type | When to Use |
|------|-------------|
| `feat` | New feature or capability |
| `fix` | Bug fix |
| `refactor` | Code restructuring (no behavior change) |
| `docs` | Documentation only |
| `test` | Adding or updating tests |
| `chore` | Build, config, tooling, dependencies |
| `style` | Formatting, whitespace (no logic change) |
| `perf` | Performance improvement |
| `ci` | CI/CD pipeline changes |

### Scopes

Use the **feature/system name** in lowercase:

```
feat(board): add gem spawning with object pooling
fix(matching): L-shape detection missing diagonal cases
refactor(powerups): extract bomb logic into strategy pattern
docs(gdd): add cascade scoring rules
test(board): add edge-case tests for 8x8 grid boundaries
chore(deps): update DOTween to 1.2.765
```

### Rules
- **Lowercase** everything — type, scope, and description
- **Imperative mood** — "add feature" not "added feature" or "adds feature"
- **No period** at the end
- **50 char limit** for the subject line (soft limit — aim for it)
- **Body** is optional — use for explaining *why*, not *what*

### Multi-line Commits
```
feat(cascade): implement chain detection with combo multiplier

Chains are detected by tracking matched gems between gravity passes.
Each chain level multiplies the score by 1.5x.
Combo counter resets when no new matches are found after a fill.
```

---

## Branch Naming

```
type/scope-short-description
```

### Examples
```
feat/board-gem-spawning
fix/matching-l-shape-detection
refactor/powerups-strategy-pattern
docs/gdd-scoring-update
chore/upgrade-unity-6.2
```

### Rules
- Use **kebab-case** (lowercase, hyphens)
- Keep it short but descriptive
- Prefix with the same types as commits

---

## Branch Strategy

| Branch | Purpose | Merges Into |
|--------|---------|-------------|
| `main` | Production-ready releases | — |
| `develop` | Integration branch | `main` |
| `feat/*` | Feature work | `develop` |
| `fix/*` | Bug fixes | `develop` or `main` (hotfix) |

### PR Rules
- **Squash merge** feature branches into `develop`
- **Merge commit** `develop` into `main` (preserves history)
- PR title follows commit format: `feat(board): add gem spawning`
- Delete branch after merge
