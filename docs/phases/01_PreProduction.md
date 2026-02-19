# Phase 1: Pre-Production

> **Agent**: Architect + Game Designer
> **Output**: Complete ProjectConfig.yaml, finalized GDD

## Goal
Make all technical and design decisions before writing any code.

## Activities

### 1. Pre-Production Questionnaire
The Architect agent guides the user through a comprehensive questionnaire.

> **Tip — Better input = better output**: For any open-ended decision (vision, player fantasy, reference games), use **TCREI** when prompting the AI:
> - **T** — Task: What exactly do you need decided?
> - **C** — Context: Current constraints, platform, team size, prior decisions.
> - **R** — References: Examples, games, tools that inspired you.
> - **E** — Evaluate: After the AI's first answer, tell it what's right/wrong/missing.
> - **I** — Iterate: Refine until it matches your vision.
>
> If results are still off after one round, apply **RSTI**: break your prompt into bullet points, rephrase the goal, or add a hard constraint (e.g. "answer in 3 options only").

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
