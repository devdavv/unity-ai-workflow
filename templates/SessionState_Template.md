# Session State — [Date] [HH:MM]

> **Purpose**: Paste this into a fresh Antigravity/Claude Code window to restore context without reloading the full conversation. The agent writes this when a session gets long (see RULES.md → Context Window Hygiene).

---

## Project

- **Game**: [Project name]
- **ProjectConfig**: `docs/ProjectConfig.yaml` — key settings: Unity [version], [render pipeline], [ui system], ai_mode: [assistant/mix/automatic]
- **Current Phase**: [Phase 0–5]
- **Branch**: `[current git branch]`

---

## Sprint State

**Sprint Goal**: [one sentence]

| # | Feature / Task | Status | Notes |
|---|---------------|--------|-------|
| 1 | [Feature name] | ✅ Done / 🔄 In Progress / ⬜ Pending | [Any key decisions or deferred items] |
| 2 | | | |

---

## Completed This Session

- [Feature name] — committed on `feature/[branch]`; tests passing; game feel: [integrated / TODO filed]
- [Task] — [brief outcome]

---

## In Progress

- **[Feature/Task]**: [where we left off, e.g. "Gherkin written, starting implementation"]
- Relevant files: `[path/to/file.cs]`

---

## Pending Decisions

- [ ] [Decision needed, e.g. "Confirm dash mechanic: coyote time yes/no?"]
- [ ] [Asset to source: "Dash VFX — placeholder or generate via Fal.ai?"]

---

## Key Config Changes Since Last Session

- [e.g. "Added PrimeTween to ProjectConfig.yaml third_party list"]
- [e.g. "Switched ui_system to Mixed (UIToolkit + UGUI)"]

---

## Active TODOs in Code

```
// TODO(gamefeel): [feature] — pending
// ASSET: [description needed]
```

List any unresolved `// TODO(gamefeel):` or `// ASSET:` markers here so the next session doesn't miss them.

---

## Notes for Next Session

[Anything the next agent should know before starting — gotchas, architectural constraints, user preferences expressed this session]
