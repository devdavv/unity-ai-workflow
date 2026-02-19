---
description: Unified feature development loop. Implements a gameplay feature AND integrates game feel in the same pass — no deferring polish to later. Use for all new gameplay features during Phase 4 Production. Triggers on "implement this feature", "build X mechanic", "add Y system", "create the jump", "add a dash", or any request to create a new gameplay feature. Preferred over /create for anything player-facing.
---

# /implement-feature — Feature Development Loop

Build a feature complete: interrogation → TDD implementation → game feel → documentation — all in one pass.

## Agents
**Gameplay Dev** (primary, implementation) + **Game Designer** (game feel questions)

## Why One Loop?
Polish is not Phase 5. Every feature ships complete: code + feel + documentation.
A feature is only "done" when it plays well, not just when it compiles.
*"Too much juice is easier to fix than too little."*

---

## Steps

### 1. Feature Brief
State the feature in one sentence. Get user confirmation before interrogating.

Example: *"We're implementing the double-jump mechanic. The player can jump a second time while airborne."*

---

### 2. Deep Interrogation
Ask the user these questions. Don't skip them — answers shape implementation and feel.

**Reference games:**
> "Name 1-3 games that do this mechanic well. What do you like about how they handle it?"

**Code references:**
> "Do you know of any GitHub repos, Unity packages, or tutorials that implement this? I can use them as a starting point."

**Behavior & edge cases:**
> "What should happen at the limits?
> - Can the player jump again after wall-jumping?
> - What cancels the second jump? (landing only? or also coyote time?)
> - Any cooldown, limited charges, or special conditions?"

**Scope:**
> "Is this prototype/spike quality, or production-ready?"
> *(Note: scope affects test coverage depth, not code standards — RULES.md always applies)*

**Visual sketch (optional):**
> If the feature involves new UI screens, a complex behavior flow, or a non-obvious state machine, offer:
> *"Would a quick sketch help clarify this before we code? I can suggest the right tool: Miro for a system/flow diagram, Excalidraw for a quick diagram, or Pencil.dev for a UI layout mockup."*
> Skip this offer for simple, single-script features. Reference `docs/ASSET_RESOURCES.md → Visual Communication Tools` for tool guidance.

---

### 3. Game Feel Questions (per-feature GFD pass)
Ask upfront. Don't defer. These answers update the GFD before a line of code is written.

> "Let's lock in the feel for this feature now. Answer roughly — we'll tune values during implementation."

- **Emotion**: "What should the player FEEL when this triggers?" *(power, freedom, vulnerability, precision, speed...)*
- **VFX**: "Any visual feedback? Even rough: 'some burst', 'trail', 'screen flash', 'particles at feet'?"
- **SFX**: "SFX mood? *(punchy, whoosh, soft, mechanical, organic, magical?)*"
- **Camera**: "Any camera reaction? *(shake, zoom in/out, FOV pulse, nothing?)*"
- **Haptics**: "Controller rumble? *(none, subtle, strong?)*"
- **Hitstop/Time scale**: "Any moment of drama? *(freeze frame, slow-mo burst, nothing?)*"
- **Game feel timing**: "Integrate game feel NOW (recommended) or flag for later?"

**If user says "later":**
- Add `// TODO(gamefeel): [feature name] — pending` in the main script
- Add a pending row to GFD Feedback Matrix now (with empty values to fill later)
- AI will not prompt about this again unless `/polish` is run

---

### 4. Asset Identification
Based on game feel answers, identify what's needed before coding starts:

- List each required asset: VFX particle, SFX clip, animation, material, **shader effect**, sprite, texture, etc.
  - **Shader effects** are first-class assets: outline flash, dissolve, chromatic aberration, UV scroll, distortion, glow. See `docs/ASSET_RESOURCES.md → Shaders & Procedural Graphics` and `game-feel-integrator/references/shaders-as-feel.md`
- Reference `docs/ASSET_RESOURCES.md` for where to get them
- In **automatic mode**: suggest specific sources and mark as placeholders if not yet acquired
- In **mix/assistant mode**: ask the user which sources they prefer, or whether to use placeholders

> Placeholder policy: if assets aren't ready, implement with placeholder (`SFX_placeholder`, white cube, default particle) and mark with `// ASSET: [description needed]`

> **Knowledge freshness & verification**: For every asset, package, tutorial, or tool recommended in this step, mark it as `[VERIFIED: source]` (accessed this session) or `[UNVERIFIED: training data]`. For `[UNVERIFIED]` items, either:
> - Query **Tavily MCP** inline (if connected) to confirm current version and status before proceeding, or
> - Prompt: *"This recommendation is from training data. Want to do a quick Perplexity or web search to confirm it's current?"*
> Never present unverified package versions or tool availability as fact.

---

### 5. Update GDD + GFD (before coding)
Update documentation to lock in decisions:

**GDD:**
- Add/update the Gherkin scenario for this feature:
```gherkin
Scenario: Player double-jumps
  Given the player has already jumped once
  And the player is airborne
  When the player presses Jump again
  Then the player receives upward force equal to jump_force * 0.85
  And a dust burst VFX plays at the player's position
  And a jump SFX plays with pitch 1.1
```

**GFD Feedback Matrix:**
Add a row for this feature's event with the answers from Step 3.

---

### 6. Implementation (TDD-first)
Use `unity-feature-scaffold` + `unity-test-runner` skills:

1. **Scaffold** — Create feature folder, `.asmdef`, test assembly
2. **Write failing test** — Convert the Gherkin scenario to NUnit
3. **Implement** — Minimal code to pass the test
4. Follow `RULES.md` (caching, serialization, GameDebug, no GetComponent in Update)
5. Follow `CODING_STANDARDS.md` + `NAMING_CONVENTIONS.md`

---

### 7. Game Feel Integration (if "now" from Step 3)
Use `game-feel-integrator` skill, referencing the GFD row from Step 5:

- Apply **Rule of Three**: visual + audio + kinesthetic minimum
- Implement in order: audio → VFX → camera → tween → hitstop
- Start values **exaggerated**, tune down during playtest
- Ensure: tweens killed on object destroy, particles pooled, no GC in Update

---

### 8. Run Tests
- Via Unity MCP `run_tests` if available
- Otherwise: instruct user to run in Test Runner window
- All tests must pass before committing

---

### 8.5. Code Review
Run the `code-review` skill on all new/modified files before committing.

- In **automatic** mode: run review automatically and fix any flagged issues.
- In **mix/assistant** mode: ask *"Run a quick code review before committing?"*

The review checks RULES.md compliance, naming conventions, performance anti-patterns, architecture, test coverage against Gherkin scenarios, and game feel TODO status.

**Do not commit until all issues from the review are resolved.**

---

### 9. Commit
Commit the complete feature:
```
feat({feature}): implement {name} with feel
```
Or split if preferred:
```
feat({feature}): implement {name}
style({feature}): add game feel (vfx, sfx, camera)
```
Branch: `feature/{feature-name}`

If `prompt_logging: true` in `ProjectConfig.yaml`, also append a log entry to `docs/PromptLog.md` (or stage it in the same commit as `chore(log): log implement-feature session`).

---

### 10. Output Checklist
```
✅ Feature brief confirmed
✅ Deep interrogation complete
✅ GDD Gherkin scenario added/updated
✅ GFD Feedback Matrix row filled (or pending row filed)
✅ Assets identified (acquired or marked as placeholder)
✅ Tests written and passing
✅ Code review passed (code-review skill)
✅ Game feel integrated (or TODO filed)
✅ Committed on feature branch
✅ PromptLog.md updated (if prompt_logging: true)
```
