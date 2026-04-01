# /uw-cmd-create — Code Creation Workflow

Generate isolated code following a test-driven development approach.

> **Scope**: Utilities, helpers, data classes, editor tools, and backend systems with no direct game feel.
> For gameplay features: use `/uw-cmd-implement-feature` instead.

## Skills
Load `uw-unity-feature-scaffold` from `.claude/skills/uw-unity-feature-scaffold/SKILL.md`.
Load `uw-unity-test-runner` from `.claude/skills/uw-unity-test-runner/SKILL.md`.

## Steps

### 1. Understand the Requirement
- Which **GDD scenario** does this implement?
- Read the relevant **TDD** section for architecture constraints.
- Check `docs/ProjectConfig.yaml` for package versions and technology choices.

### 2. Scaffold the Feature
Use `uw-unity-feature-scaffold` skill:
- Feature folder structure
- Assembly Definition (`.asmdef`)
- Test Assembly Definition

### 3. Write Tests First
Use `uw-unity-test-runner` skill:
- Convert the Gherkin scenario to NUnit test(s).
- Write the test — it should **fail** because the implementation doesn't exist yet.

### 4. Implement
Write the minimal code to make tests pass:
- Follow `CLAUDE.md` rules (serialization, caching, thread safety)
- Follow `docs/CODING_STANDARDS.md` formatting
- Follow `docs/NAMING_CONVENTIONS.md` for identifiers
- Use `GameDebug` for debug logging

### 5. Run Tests
- Via Unity MCP `run_tests` if available.
- Otherwise, instruct user to run in Test Runner.
- All tests must pass before proceeding.

### 6. Commit
- Feature branch: `feature/{feature-name}`
- Commit message: `feat({feature}): {description}`
- Follow `docs/GIT_CONVENTIONS.md`

### 7. Output
- Working feature with passing tests
- Feature committed to a branch

$ARGUMENTS
