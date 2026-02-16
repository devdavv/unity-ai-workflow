---
description: Guided ideation session to transform a rough idea into structured GDD sections.
---

# /brainstorm — Ideation Workflow

Transform a rough game idea into structured design document sections.

## Agent
**Game Designer** — Load `game-designer.md` persona.

## Steps

### 1. Understand the Vision
Ask the user:
- What is the **one-sentence pitch** for this game?
- What is the **core fantasy** — what does the player *feel*?
- Name **3 reference games** that capture elements of what you want.
- What **platform(s)** are you targeting?

### 2. Define the Core Loop
Build the fundamental loop together:
```
Input → Action → Feedback → Reward → Loop
```
Ask: "What does the player do most often? What makes that action satisfying?"

### 3. Write Gherkin Scenarios
Convert each mechanic into a `Given/When/Then` scenario:
```gherkin
Scenario: Player jumps
  Given the player is grounded
  When the player presses the jump button
  Then the player moves upward with force 10
  And a dust VFX plays at the player's feet
  And a jump SFX plays
```

### 4. Populate GDD Sections
Fill in the GDD template sections:
- Core Loop Definition table
- Mechanics (Gherkin scenarios)
- Entity & Balance Data tables
- Input Mapping table

### 5. Initial GFD Pass
For each mechanic, ask: "What should the player *feel*?" and start the Feedback Matrix:
- What VFX?
- What SFX?
- Camera reaction?
- Haptics?

### 6. Output
- Updated `docs/GDD.md` sections
- Draft `docs/GFD.md` Feedback Matrix
- List of open design questions for the user to think about
