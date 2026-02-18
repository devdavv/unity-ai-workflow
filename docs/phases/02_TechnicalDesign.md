# Phase 2: Technical Design

> **Agent**: Architect
> **Output**: Complete TDD

## Goal
Define the technical architecture before writing production code. All architecture is documented with **Mermaid diagrams** in the TDD — so the user can see the structure visually before a single line of code is written.

> **Diagramming tool**: [Mermaid](https://mermaid.js.org) — text-based, renders on GitHub, AI-generatable.
> For free-form exploration before committing, use [Miro](https://miro.com) boards.

## Activities

### 1. Assembly Definition Graph
Design the module structure and generate a **Mermaid `graph TD`** diagram:
- Which assemblies exist
- Dependency direction (always inward — features → core, never reverse)
- Test assembly setup
- Output: Mermaid diagram committed to TDD

### 2. Design Patterns
Choose patterns for each system and diagram stateful ones:
- State machines for player/AI controllers → **Mermaid `stateDiagram-v2`** per controller
- ScriptableObject architecture for data
- Event channels for cross-system communication
- MVVM for UI (if UI Toolkit)
- Output: one state diagram per stateful system in TDD

### 3. Data Architecture
Define how data flows and persists:
- ScriptableObject catalog
- Save/load strategy
- Configuration management

### 4. Networking Architecture (if applicable)
Based on `ProjectConfig.yaml → networking`:
- Authority matrix
- State sync strategy
- RPC patterns

### 5. API Constraints
Define allowed and denied APIs:
- Package version boundaries
- Performance-critical paths
- Unity API best practices

### 6. Performance Targets
Set measurable targets:
- Frame rate per platform
- Memory budget
- Load time limits
- Draw call budget

## Entry Criteria
- Phase 1 complete (ProjectConfig filled)

## Exit Criteria
- [ ] TDD complete and reviewed by user
- [ ] Assembly definition graph as Mermaid diagram — approved
- [ ] State diagrams for all stateful systems (player, AI, game manager)
- [ ] Screen flow diagram complete (if UI-heavy game)
- [ ] Design patterns chosen for all systems
- [ ] API allow/deny lists defined
- [ ] Performance targets set
