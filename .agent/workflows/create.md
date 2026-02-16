---
description: TDD-first code generation workflow for new scripts and systems.
---

# /create — Code Creation Workflow

Generate new code following a test-driven development approach.

## Agent
**Gameplay Dev** (primary), or **UI Specialist** / **Network Engineer** depending on domain.

## Steps

### 1. Understand the Requirement
- Which **GDD scenario** does this implement?
- Read the relevant **TDD** section for architecture constraints.
- Check **ProjectConfig.yaml** for package versions and technology choices.

### 2. Scaffold the Feature
Use `unity-feature-scaffold` skill to create:
- Feature folder structure
- Assembly Definition (`.asmdef`)
- Test Assembly Definition

### 3. Write Tests First
Use `unity-test-runner` skill:
- Convert the Gherkin scenario to NUnit test(s).
- Write the test — it should **fail** because the implementation doesn't exist yet.

### 4. Implement
Write the minimal code to make the tests pass:
- Follow `RULES.md` constraints (serialization, caching, thread safety).
- Use `CODING_STANDARDS.md` formatting.
- Use `NAMING_CONVENTIONS.md` for all identifiers.
- Use `GameDebug` for any debug logging.

### 5. Run Tests
- Via Unity MCP `run_tests` if available.
- Otherwise, instruct user to run in Test Runner.
- All tests must pass before proceeding.

### 6. Commit
- Via GitHub MCP `push_files` if available.
- Feature branch: `feature/{feature-name}`
- Commit message: `feat({feature}): {description}`

### 7. Output
- Working feature with passing tests
- Feature committed to a branch
- PR created if GitHub MCP is available
