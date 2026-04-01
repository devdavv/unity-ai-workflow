# /uw-cmd-implement-feature — Feature Development Loop

Build a feature complete: interrogation -> TDD implementation -> game feel -> documentation — all in one pass.

## Skills
Load `uw-game-feel-integrator` from `.claude/skills/uw-game-feel-integrator/SKILL.md`.

## Why One Loop?
Polish is not Phase 5. Every feature ships complete: code + feel + documentation.
A feature is only "done" when it plays well, not just when it compiles.
*"Too much juice is easier to fix than too little."*

---

## Steps

### 1. Feature Brief
State the feature in one sentence. Get user confirmation before interrogating.

Example: *"We're implementing the double-jump mechanic. The player can jump a second time while airborne."*

### 2. Deep Interrogation
Ask the user these questions. Don't skip them — answers shape implementation and feel.

> **Structure your answers with TCREI for best results:**
> - **T** — Task: What exactly should this feature do?
> - **C** — Context: Constraints, current systems it touches, scope
> - **R** — References: Games, repos, tutorials that do this well
> - **E** — Evaluate: After first pass, what's right/wrong/missing?
> - **I** — Iterate: Refine from there

**Reference games:**
> "Name 1-3 games that do this mechanic well. What do you like about how they handle it?"

**Code references:**
> "Do you know of any GitHub repos, Unity packages, or tutorials that implement this? I can use them as a starting point."

**Behavior & edge cases:**
> "What should happen at the limits?"

**Scope:**
> "Is this prototype/spike quality, or production-ready?"

**Visual sketch (optional):**
> If the feature involves new UI or complex flows, offer visual communication tools. Reference `docs/ASSET_RESOURCES.md -> Visual Communication Tools`.

### 3. Game Feel Questions (per-feature GFD pass)
Ask upfront. Don't defer. These answers update the GFD before a line of code is written.

> "Let's lock in the feel for this feature now. Answer roughly — we'll tune values during implementation."

- **Emotion**: "What should the player FEEL when this triggers?"
- **VFX**: "Any visual feedback?"
- **SFX**: "SFX mood?"
- **Camera**: "Any camera reaction?"
- **Haptics**: "Controller rumble?"
- **Hitstop/Time scale**: "Any moment of drama?"
- **Game feel timing**: "Integrate game feel NOW (recommended) or flag for later?"

**If user says "later":**
- Add `// TODO(gamefeel): [feature name] — pending` in the main script
- Add a pending row to GFD Feedback Matrix
- Don't prompt about this again unless `/uw-cmd-polish` is run

### 4. Asset Identification
Based on game feel answers, identify what's needed before coding:
- List each required asset: VFX, SFX, animation, material, shader effect, sprite, texture
- Reference `docs/ASSET_RESOURCES.md` for sourcing — cite the reference game or source when suggesting assets
- Placeholder policy: if assets aren't ready, use placeholders marked with `// ASSET: [description needed]`

> **Verification**: For every recommended asset, package, or tool, mark as `[VERIFIED: source]` or `[UNVERIFIED: training data]`. Never present unverified information as fact.

### 5. Update GDD + GFD (before coding)
**GDD:** Add/update the Gherkin scenario for this feature.
**GFD:** Add a Feedback Matrix row with answers from Step 3.

### 6. Implementation (TDD-first)
Use `uw-unity-feature-scaffold` skill (`.claude/skills/uw-unity-feature-scaffold/SKILL.md`) and `uw-unity-test-runner` skill (`.claude/skills/uw-unity-test-runner/SKILL.md`):

1. **Scaffold** — Create feature folder, `.asmdef`, test assembly
2. **Write failing test** — Convert the Gherkin scenario to NUnit
3. **Implement** — Minimal code to pass the test
4. Follow `CLAUDE.md` rules (caching, serialization, GameDebug, no GetComponent in Update)
5. Follow `docs/CODING_STANDARDS.md` + `docs/NAMING_CONVENTIONS.md`

**Architecture-dependent skills** — load the relevant skill based on what the feature needs:
- **Game data (weapons, enemies, items):** Use `uw-scriptable-object-arch` for SO data containers and event channels.
- **DI services/commands:** If `ProjectConfig.yaml -> architecture_pattern: "di-first"`, use `uw-dependency-injection` for service registration and cross-feature Commands.
- **State management:** If the feature involves discrete states (game flow, character states, AI phases), use `uw-state-machine`.
- **UI screens/HUD:** If the feature has a UI component, use `uw-ui-toolkit-binder` (or uGUI if `ProjectConfig.yaml -> ui_system: "UGUI"`).
- **Networking:** If the feature involves multiplayer sync, use `uw-network-setup`.
- **Editor tools:** If the component has spatial data, complex configuration, or designer-facing parameters, use `uw-unity-editor-tools` for custom inspectors/gizmos.

### 7. Game Feel Integration (if "now" from Step 3)
Use `uw-game-feel-integrator` skill, referencing the GFD row from Step 5:
- Apply **Rule of Three**: visual + audio + kinesthetic minimum
- Implement in order: audio -> VFX -> camera -> tween -> hitstop
- Start values **exaggerated**, tune down during playtest
- Ensure: tweens killed on object destroy, particles pooled, no GC in Update

### 8. Run Tests
- Via Unity MCP `run_tests` if available
- Otherwise: instruct user to run in Test Runner
- All tests must pass before committing

### 8.5. Code Review
Run `uw-code-review` skill (`.claude/skills/uw-code-review/SKILL.md`) on all new/modified files.
- In **autonomous** mode: run review automatically and fix flagged issues
- In **guided** mode: ask before running

Do not commit until all review issues are resolved.

### 9. Commit
```
feat({feature}): implement {name} with feel
```
Branch: `feature/{feature-name}`
Follow `docs/GIT_CONVENTIONS.md`.

If `prompt_logging: true` in ProjectConfig, append a log entry to `docs/PromptLog.md`.

### 10. Output Checklist
```
- Feature brief confirmed
- Deep interrogation complete
- GDD Gherkin scenario added/updated
- GFD Feedback Matrix row filled (or pending row filed)
- Assets identified (acquired or marked as placeholder)
- Tests written and passing
- Code review passed
- Game feel integrated (or TODO filed)
- Committed on feature branch
- PromptLog updated (if enabled)
```

$ARGUMENTS
