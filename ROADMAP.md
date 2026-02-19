# Unity AI Workflow — Roadmap

## ✅ v0.1–v0.4: Foundation (Complete)

All core infrastructure is built and working:

- [x] Core repository structure (`.agent/`, `templates/`, `docs/`)
- [x] Global rules & auto-routing (`RULES.md`, `AGENTS.md`)
- [x] 8 Agent personas (Architect, Gameplay Dev, UI Specialist, Game Designer, QA Tester, Network Engineer, Art Director, Tool Developer)
- [x] 7 Workflows (`/brainstorm`, `/plan`, `/create`, `/test`, `/debug`, `/polish`, `/setup-project`)
- [x] 9 Skills (feature scaffold, UI toolkit binder, scriptable-object arch, game feel integrator, network setup, test runner, debugging, project setup, editor tools)
- [x] 5 Templates (GDD, GFD, TDD, SprintPlan, ProjectConfig)
- [x] 6 Phase guides (00 Ideation → 05 Polish)
- [x] Coding standards, naming conventions, git conventions
- [x] Asset resources, MCP setup docs, design principles

---

## ✅ v0.5: Vision Alignment (Complete)

Advancing the repository from a foundation to a complete, opinionated workflow system:

- [x] Three Dev Modes (Assistant / Mix / Automatic) — `ProjectConfig.yaml`, `RULES.md`, `AGENTS.md`
- [x] `/implement-feature` workflow — unified feature loop with game feel integrated per-feature
- [x] AI Generation Tools section — `docs/ASSET_RESOURCES.md` (Midjourney, Meshy, ElevenLabs, Suno, Fal.ai, Replicate, Iconify)
- [x] Phase 4 Production rewritten around the feature loop
- [x] `/setup-project` now includes dev mode selection + automatic mode onboarding Q&A
- [x] README rewritten to lead with dev modes and integrated game feel philosophy + resources/credits section
- [x] Removed FEEL/MMFeedbacks as default — not AI-friendly; DOTween/PrimeTween + custom code is the approach
- [x] Cinemachine de-defaulted — opt-in only, custom camera as default
- [x] TCREI prompting methodology + `/code-review` skill + prompt logging — `AGENTS.md`, `RULES.md`
- [x] Verification marking system (`[VERIFIED]` / `[SYNTHESIZED]` / `[UNVERIFIED]`) — `RULES.md`, `implement-feature.md`
- [x] Game feel theory & shader references — `game-feel-integrator/references/` (ADSR envelope, Three Pillars, shaders-as-feel)
- [x] Visual Communication Tools + Shaders & Procedural Graphics — `docs/ASSET_RESOURCES.md`
- [x] MCP setup enriched (Tavily, claude-mem) — `docs/MCP_SETUP.md`
- [x] Claude Code Skills Ecosystem section — `docs/TOOLING.md`
- [x] SessionState handoff template — `templates/SessionState_Template.md`
- [x] Skill descriptions validated against Anthropic skill-creator trigger format
- [x] Phase 5 Polish guide clarified as "final tuning" not "first-time polish"

---

## 🔧 v1.0: Public Release & Multi-IDE Support (Current)

- [x] Claude Code support — `CLAUDE.md` (auto-loaded instructions), `.claude/commands/` (9 slash commands), dual-native architecture
- [x] Hybrid workflow support — Claude Code + Antigravity sharing `.agent/skills/`, docs, and templates as common brain
- [ ] Cursor support (`.cursor/rules/`, `.mdc` format conversion)
- [ ] Comprehensive sample project walkthrough using automatic mode (a complete game end-to-end)
- [ ] Community feedback integration (CONTRIBUTING.md, issue templates)
- [ ] `skill-creator` skill for generating new Unity-specific skills from within the workflow

---

## 🔭 Future / Side Projects

### Documentation Website
- Interactive examples showing prompt → result for each workflow
- Setup walkthroughs with screenshots/video
- Project showcase: real games built with this workflow + the prompts used
- Side-by-side comparison: Claude Code vs Antigravity vs hybrid usage
- Best practices for token optimization across AI tools

### AI Asset Generation Pipelines
- Sprite generation pipeline (Stable Diffusion → Unity import)
- SFX generation pipeline (ElevenLabs Sound FX → AudioClip)
- 3D model pipeline (Meshy → FBX import → Unity materials)
- Curated asset pack recommendations per game genre
