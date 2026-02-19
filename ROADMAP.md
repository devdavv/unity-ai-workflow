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

## 🔧 v0.5: Vision Alignment (Current)

Advancing the repository from a foundation to a complete, opinionated workflow system:

- [x] Three Dev Modes (Assistant / Mix / Automatic) — `ProjectConfig.yaml`, `RULES.md`, `AGENTS.md`
- [x] `/implement-feature` workflow — unified feature loop with game feel integrated per-feature
- [x] AI Generation Tools section — `docs/ASSET_RESOURCES.md` (Midjourney, Meshy, ElevenLabs, Suno, Fal.ai, Replicate, Iconify)
- [x] Phase 4 Production rewritten around the feature loop
- [x] `/setup-project` now includes dev mode selection + automatic mode onboarding Q&A
- [x] README rewritten to lead with dev modes and integrated game feel philosophy
- [x] TCREI prompting methodology + `/code-review` skill + prompt logging — `AGENTS.md`, `RULES.md`
- [x] Verification marking system (`[VERIFIED]` / `[SYNTHESIZED]` / `[UNVERIFIED]`) — `RULES.md`, `implement-feature.md`
- [x] Game feel theory & shader references — `game-feel-integrator/references/` (ADSR envelope, Three Pillars, shaders-as-feel)
- [x] Visual Communication Tools + Shaders & Procedural Graphics — `docs/ASSET_RESOURCES.md`
- [x] MCP setup enriched (Tavily, claude-mem) — `docs/MCP_SETUP.md`
- [x] Claude Code Skills Ecosystem section — `docs/TOOLING.md`
- [x] SessionState handoff template — `templates/SessionState_Template.md`
- [ ] Validate all skill descriptions follow Anthropic skill-creator trigger format
- [ ] Phase 5 Polish guide — update to clarify role as "final tuning" not "first-time polish"

---

## 🚀 v1.0: Public Release & Multi-IDE Support

- [ ] Claude Code support (`.claude/skills/`, CLAUDE.md, system prompt integration)
- [ ] Cursor support (`.cursor/rules/`, `.mdc` format conversion)
- [ ] Comprehensive sample project walkthrough using automatic mode (a complete game end-to-end)
- [ ] Community feedback integration
- [ ] `skill-creator` skill for generating new Unity-specific skills from within the workflow

---

## 🔭 Future / Side Projects

Tools for generating game assets with AI — potentially separate tooling or a skill:
- Sprite generation pipeline (Stable Diffusion → Unity import)
- SFX generation pipeline (ElevenLabs Sound FX → AudioClip)
- 3D model pipeline (Meshy → FBX import → Unity materials)
- Curated asset pack recommendations per game genre
