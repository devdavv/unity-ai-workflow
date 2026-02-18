# Phase 1: Pre-Production

> **Agent**: Architect + Game Designer
> **Output**: Complete ProjectConfig.yaml, finalized GDD

## Goal
Make all technical and design decisions before writing any code.

## Activities

### 1. Pre-Production Questionnaire
The Architect agent guides the user through a comprehensive questionnaire:

| Question | Decision | ProjectConfig Field |
|----------|----------|-------------------|
| Dev mode? | Assistant / Mix / Automatic | `ai_mode` |
| Unity version? | 6.2.0f1 | `unity_version` |
| Render pipeline? | URP / HDRP | `render_pipeline` |
| UI system? | UIToolkit / UGUI | `ui_system` |
| Networking? | None / NGO / Mirror / Photon | `networking` |
| Folder strategy? | Feature-based / Type-based | `folder_strategy` |
| Async pattern? | Awaitable / UniTask | `async_pattern` |
| Target platforms? | PC, Mobile, Console | `target_platforms` |
| Third-party packages? | DOTween, Cinemachine, etc. | `third_party` |
| Task management? | Linear / Jira / None | `task_management` |
| MCP integrations? | Unity / GitHub / Linear | `mcp` |

### 2. ProjectConfig Completion
Fill out `ProjectConfig.yaml` with all decisions.

### 3. GDD Finalization
Complete remaining GDD sections:
- Entity & Balance Data tables
- Input Mapping
- Level/World structure
- Progression system

### 4. MCP Setup
Configure chosen MCP integrations:
- Unity MCP — install in Unity project
- GitHub MCP — configure in IDE
- Linear / Notion — connect accounts

## Entry Criteria
- Phase 0 complete (GDD + GFD drafted)

## Exit Criteria
- [ ] ProjectConfig.yaml fully filled
- [ ] GDD complete and reviewed
- [ ] GFD Feedback Matrix complete
- [ ] MCP integrations configured
- [ ] Naming conventions agreed upon
