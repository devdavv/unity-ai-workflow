# Phase 0: Ideation

> **Command**: `/uw-cmd-brainstorm`
> **Skills**: `uw-game-feel-integrator`
> **Output**: Draft GDD + Draft GFD

## Goal
Transform a rough game idea into structured design documentation.

## Activities

### 1. Vision Definition
The `/uw-cmd-brainstorm` command guides you through:
- One-sentence pitch
- Core fantasy (what does the player *feel*?)
- Target emotions
- Reference games analysis

### 2. Core Loop Design
Define the fundamental gameplay loop:
```
Input → Action → Feedback → Reward → Loop
```

### 3. Mechanics Specification (Gherkin)
Write each mechanic as `Given/When/Then` scenarios:
```gherkin
Scenario: Player jumps
  Given the player is grounded
  When the player presses jump
  Then the player moves upward with force 10
```

### 4. Initial Feel Pass
For each mechanic, define the feedback response:
- What VFX/SFX/camera/haptic reactions should occur?
- Start the GFD Feedback Matrix.

## Entry Criteria
- User has a game idea (any level of detail)

## Exit Criteria
- [ ] GDD sections 1-3 drafted (Vision, Core Loop, Mechanics)
- [ ] GFD Feedback Matrix started
- [ ] Open design questions listed
- [ ] User is confident in the direction
