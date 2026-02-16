# Phase 2: Technical Design

> **Agent**: Architect
> **Output**: Complete TDD

## Goal
Define the technical architecture before writing production code.

## Activities

### 1. Assembly Definition Graph
Design the module structure:
- Which assemblies exist
- Dependency direction (always inward)
- Test assembly setup

### 2. Design Patterns
Choose patterns for each system:
- State machines for player/AI controllers
- ScriptableObject architecture for data
- Event channels for cross-system communication
- MVVM for UI (if UI Toolkit)

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
- [ ] Assembly definition graph approved
- [ ] Design patterns chosen for all systems
- [ ] API allow/deny lists defined
- [ ] Performance targets set
