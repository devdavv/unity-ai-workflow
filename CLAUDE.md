# Unity AI Workflow

AI-first Unity 6.2+ game development workflow. This repo is pure infrastructure — skills, commands, docs, and templates. No Unity project inside. Copy it into your game workspace root and Claude Code reads it as its brain.

## Workspace Layout

```
MyGame/                        ← Open THIS folder
├── CLAUDE.md                  ← This file (auto-loaded)
├── .claude/
│   ├── commands/              ← Slash commands (/uw-cmd-implement-feature, /uw-cmd-brainstorm, etc.)
│   ├── skills/                ← Reusable knowledge modules (13 skills)
│   └── settings.json          ← Hooks & permissions
├── docs/                      ← Living project docs (GDD, TDD, GFD, ProjectConfig, standards)
├── templates/                 ← Pristine starters (GDD, GFD, TDD, PRD, Sprint, Session)
└── MyGameUnity/               ← Unity project (Assets/, Packages/, ProjectSettings/)
```

## Project Config

Read `docs/ProjectConfig.yaml` at the start of every session. It contains Unity version, packages, render pipeline, UI system, architecture pattern, and dev mode.

## Dev Modes

| Mode | Behavior |
|------|----------|
| **guided** *(default)* | Collaborative. Explain decisions, confirm before major changes. |
| **autonomous** | After onboarding Q&A, proceed with minimal interruption. Only pause for destructive actions. Enable Claude Code auto mode for best results. |

## Non-Negotiable Rules

These are things LLMs consistently get wrong in Unity. Always enforced.

- `[SerializeField] private` only — never expose fields as `public` for the Inspector
- Cache ALL component references in `Awake()` or `Start()`
- **Never** `GetComponent<T>()`, `FindObjectOfType<T>()`, or `GameObject.Find()` in `Update()`, `FixedUpdate()`, or `LateUpdate()`
- Use `GameDebug` wrapper (stripped from release) — never raw `Debug.Log()`
- **New Input System only** — never legacy `Input.GetKey*` or `Input.GetAxis*`
- `Awaitable` for async (Unity 6 native) — check ProjectConfig for overrides
- All feature code must live inside an `.asmdef`
- Always `[CreateAssetMenu]` on ScriptableObjects
- **Never** create, modify, or delete `.meta` files
- UI Toolkit recommended — check `ProjectConfig.yaml → ui_system` for override (UGUI/Mixed valid)
- Visual Scripting forbidden for logic — Shader Graph & VFX Graph are fine
- Never call Unity API from background threads
- `/// <summary>` only on public API methods that warrant explanation — never on classes

## Verification Marking

When recommending any external tool, package, repo, tutorial, or statistic:

- `[VERIFIED: source]` — accessed/confirmed this session
- `[SYNTHESIZED: source]` — AI-researched, not primary source
- `[UNVERIFIED: training data]` — needs web search to verify

Never present unverified information as fact.

## Coding Standards

Follow `docs/CODING_STANDARDS.md`, `docs/NAMING_CONVENTIONS.md`, and `docs/GIT_CONVENTIONS.md`.

## Session Hygiene

After a full `/uw-cmd-implement-feature` loop OR 3+ major decisions, offer:
> "This session is getting long. Want me to write a SessionState summary you can paste into a fresh conversation?"

Use `templates/SessionState_Template.md` as the format.

## Prompt Logging

If `docs/ProjectConfig.yaml → prompt_logging` is `true`, append to `docs/PromptLog.md` after every response:
```
## [YYYY-MM-DD HH:MM] — <Workflow or Task Type>
**Prompt:** <user's exact words>
**Summary:** <≤200 word factual summary>
---
```
