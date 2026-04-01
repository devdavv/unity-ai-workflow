# Unity AI Workflow — Roadmap

## v0.1-v0.5: Foundation & Vision (Historical)

All core infrastructure built, iterated, and aligned:

- [x] Core repository structure, global rules & auto-routing
- [x] 8 Agent personas, 7 workflows, 9 skills, 5 templates, 6 phase guides
- [x] Coding standards, naming conventions, git conventions
- [x] Three dev modes (Assistant / Mix / Automatic)
- [x] `/uw-cmd-implement-feature` workflow — unified feature loop with game feel integrated per-feature
- [x] TCREI prompting methodology + `uw-code-review` skill + prompt logging
- [x] Verification marking system (`[VERIFIED]` / `[SYNTHESIZED]` / `[UNVERIFIED]`)
- [x] Game feel theory & shader references
- [x] Claude Code support — `CLAUDE.md`, `.claude/commands/` (9 slash commands)
- [x] Hybrid workflow support — Claude Code + Antigravity sharing `.agent/skills/`

---

## v2.0: Claude Code Native (Current)

Complete revamp based on latest AI research (Addy Osmani on AGENTS.md, ETH Zurich studies, Anthropic Skills 2.0).

- [x] **100% Claude Code native** — removed Antigravity as first-class platform
- [x] **Minimal CLAUDE.md** (~80 lines) — only non-discoverable Unity gotchas, per research showing verbose context files reduce success rates 2-3%
- [x] **Agent personas dissolved** — 8 persona files folded into skills and commands where non-discoverable
- [x] **Skills migrated** to `.claude/skills/` with structural prep for Skills 2.0 format
- [x] **Official Anthropic uw-skill-creator 2.0** — replaces custom skill-creator with full evaluation framework
- [x] **Dev modes simplified** from 3 (assistant/mix/automatic) to 2 (guided/autonomous), leveraging Claude Code native auto mode
- [x] **Commands updated** — all 9 slash commands self-contained (no agent file dependencies)
- [x] **PRD template** added alongside existing GDD/TDD/GFD templates
- [x] **Hook configurations** in `.claude/settings.json`
- [x] **`.agent/` directory removed** — entire tree deleted, all content in `.claude/`
- [ ] Run each of 12 skills through uw-skill-creator 2.0 evaluation loop (description optimization, trigger testing, context mode selection)
- [ ] Comprehensive sample project walkthrough using autonomous mode
- [ ] Community feedback integration (CONTRIBUTING.md, issue templates)

---

## Future

### Skill Optimization Sprint
- Run all 12 skills through uw-skill-creator 2.0 with evaluation framework
- Optimize descriptions for triggering accuracy (60/40 train/test split)
- Set `context: fork | inline` per skill based on token analysis
- Trim skill bodies to remove discoverable information

### Documentation Website
- Interactive examples showing prompt -> result for each workflow
- Setup walkthroughs with screenshots/video
- Project showcase: real games built with this workflow
- Best practices for token optimization

### AI Asset Generation Pipelines
- Sprite generation pipeline (Stable Diffusion -> Unity import)
- SFX generation pipeline (ElevenLabs Sound FX -> AudioClip)
- 3D model pipeline (Meshy -> FBX import -> Unity materials)

### Hook Library
- Pre-commit C# linting hooks
- Post-write format validation hooks
- Session start context injection hooks
- Community-contributed hook recipes
