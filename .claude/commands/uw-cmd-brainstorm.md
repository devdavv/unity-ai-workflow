# /uw-cmd-brainstorm — Ideation Workflow

Transform a rough game idea into structured design document sections.

## Steps

### 1. Understand the Vision
Present this to the user before they answer:

> **Structure your answer using TCREI for the best results:**
> - **T** — Task: What exactly do you want this game to do?
> - **C** — Context: Genre, platform, target audience, mood, tone.
> - **R** — References: 2-3 games that inspire this. What specifically do you like about each?
> - **E** — Evaluate: After my first response, tell me what's right, wrong, or missing.
> - **I** — Iterate: We'll refine from there.

Then ask:
- What is the **one-sentence pitch** for this game?
- What is the **core fantasy** — what does the player *feel*?
- Name **3 reference games** that capture elements of what you want.
- What **platform(s)** are you targeting?

### 2. Define the Core Loop
Build the fundamental loop together:
```
Input -> Action -> Feedback -> Reward -> Loop
```
Ask: "What does the player do most often? What makes that action satisfying?"

### 3. Write Gherkin Scenarios
Convert each mechanic into `Given/When/Then`:
```gherkin
Scenario: Player jumps
  Given the player is grounded
  When the player presses the jump button
  Then the player moves upward with force 10
  And a dust VFX plays at the player's feet
  And a jump SFX plays
```

### 4. Populate GDD Sections
Fill in GDD template sections: Core Loop Definition, Mechanics (Gherkin), Entity & Balance Data tables, Input Mapping.

When suggesting mechanics, visual styles, or feel patterns — **always cite the reference game or source**. Example: *"A cascade system like Candy Crush's, where chains trigger a scored multiplier."* This helps the user search for videos/screenshots to visualize.

### 5. Initial GFD Pass
For each mechanic, ask: "What should the player *feel*?" and start the Feedback Matrix (VFX, SFX, Camera, Haptics).

When art, audio, or font assets are needed, reference `docs/ASSET_RESOURCES.md` for curated sources.

### 6. Output
- Updated `docs/GDD.md` sections
- Draft `docs/GFD.md` Feedback Matrix
- List of open design questions for the user

$ARGUMENTS
