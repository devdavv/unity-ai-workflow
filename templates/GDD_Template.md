---
title: "[Project Name] — Game Design Document"
version: "0.1.0"
date: ""
author: ""
status: "Draft"
---

# Game Design Document

> **How to use this template**: Work through each section via `/uw-cmd-brainstorm`. Claude Code will ask guiding questions and help you fill in each section. Leave sections blank if they don't apply to your project.

---

## 1. Vision

### One-Sentence Pitch
<!-- What is this game in one sentence? -->

### Core Fantasy
<!-- What does the player FEEL? What power/experience does the game deliver? -->

### Target Emotions
<!-- List 3-5 emotions the player should experience (e.g., tension, triumph, curiosity) -->

### Reference Games
| Game | What to take from it |
|------|---------------------|
| | |

---

## 2. Core Loop

```
[Player Input] → [Action] → [Feedback] → [Reward/Consequence] → [Loop]
```

### Loop Definition Table

| Phase | System | Description |
|-------|--------|-------------|
| Input | | What the player physically does |
| Action | | What happens in the game world |
| Feedback | | Visual/audio/haptic response |
| Reward | | Why the player keeps doing it |

---

## 3. Mechanics

> Write each mechanic as a Gherkin scenario. These map directly to unit tests.

### 3.1 [Mechanic Name]

```gherkin
Feature: [Mechanic Name]

  Scenario: [Happy path]
    Given [initial state]
    When [player action]
    Then [expected result]
    And [feedback that plays]

  Scenario: [Edge case]
    Given [initial state]
    When [unusual action]
    Then [expected result]
```

### 3.2 [Mechanic Name]

```gherkin
Feature: [Mechanic Name]

  Scenario: [Description]
    Given [initial state]
    When [player action]
    Then [expected result]
```

---

## 4. Entities & Balance Data

### Player Stats

| Stat | Default | Min | Max | Notes |
|------|---------|-----|-----|-------|
| Health | | | | |
| Speed | | | | |
| | | | | |

### Entity Catalog

| Entity | Type | Description | Key Stats |
|--------|------|-------------|-----------|
| | | | |

---

## 5. Input Mapping

| Action | Keyboard/Mouse | Gamepad | Touch | Notes |
|--------|---------------|---------|-------|-------|
| Move | WASD | Left Stick | Virtual Joystick | |
| Jump | Space | A/Cross | Tap | |
| | | | | |

---

## 6. Progression & Economy

### Progression System
<!-- How does the player advance? Levels, unlocks, skill trees? -->

### Economy
<!-- Currency, costs, earn rates -->

---

## 7. Level / World Structure

### World Overview
<!-- High-level map of areas, levels, or worlds -->

### Level Design Principles
<!-- What makes a "good" level in this game? -->

---

## 8. Open Design Questions

<!-- List anything still undecided — revisit these during development -->

1. 
2. 
3. 
