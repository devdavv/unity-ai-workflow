# Game Feel — Theory & References

> Foundational reading for understanding *why* game feel works, not just *how* to implement it. Use this to make intentional design decisions rather than adding feel arbitrarily.

---

## Loic Jacob — Generating Strong & Consistent Sensations

**Source**: [Notion](https://loicjacob.notion.site/generate-strong-and-consistent-sensations-in-the-gameplay) — [SYNTHESIZED: Google AI Studio research — content aligns with secondary sources; direct Notion verification still pending]

Jacob's work treats game feel as an underdeveloped discipline. Most developers approach it intuitively; his goal is to make it **systematic and teachable** — defining sensations first, then assigning consistent multi-channel signifiers to produce them.

### Core Methodology: "Sensations First" (Top-Down)

1. **Define the Sensation** before any implementation (e.g., "Heavy," "Sharp," "Stretchy," "Vulnerable")
2. **Assign Signifiers**: every sensation must have a consistent set of visual/audio/haptic markers
3. **The Sensation Loop**: Input → Action → Sensation → Perception → New Input

### ADSR Envelope of Game Feel

Borrowed from audio synthesis — describes the *timing shape* of a sensation:

| Phase | Meaning | Example |
|-------|---------|---------|
| **Attack** | How fast feedback reaches peak | "Sharp" hit = 0ms; "Heavy" swing = slow ramp |
| **Decay/Sustain** | How long it holds at peak | Hitstop duration, freeze-frame |
| **Release** | How it fades out | Particle dissipation, shake damping curve |

> **ADSR Sync Rule**: Attack and Release durations must match across all three channels (Visual, Audio, Kinesthetic). If SFX fades over 0.5s, particles and screen shake must mirror that 0.5s.

### Three Pillars of Impact

For any impact event (hit, landing, explosion), Jacob identifies three non-negotiable requirements:

1. **The Pause** (Temporal) — hitstop / time-scale shift. Gives the brain time to register the event's importance.
2. **The Shake** (Kinesthetic) — the *object itself*, not just the camera. Both the attacker and the target should react.
3. **The Flash** (Visual) — single-frame or multi-frame silhouette change (white/red fill flash).

### Sensation Vocabulary

Use these when prompting the Game Designer or Art Director to define a mechanic's feel:

| Sensation | Characteristics |
|-----------|----------------|
| **Weight** | High gravity, slow ramp-up, heavy screen shake, low-pitched SFX |
| **Sharpness** | 0ms response, high-pitched snappy SFX, instant visual flashes |
| **Elasticity** | Overshoot then settle (OutBack easing), squash/stretch, "boing" SFX |
| **Friction** | Rumble during movement, spark particles, deceleration over time |

**Takeaway for this workflow**: The GFD Feedback Matrix is the practical tool. Jacob's framing is the *why* — every row in the matrix is an opportunity to create a sensation, not just a technical checkbox.

---

## "Principles of Virtual Sensation"

**Source**: [gamedeveloper.com](https://www.gamedeveloper.com/design/principles-of-virtual-sensation) — ✅ VERIFIED

A peer article exploring how virtual sensations function as core player experience. Explores the relationship between technical feedback and psychological/emotional response. Recommended reading alongside the GFD.

---

## Steve Swink — *Game Feel: A Game Designer's Guide to Virtual Sensation* (2008)

**Source**: [amazon.com/Game-Feel](https://www.amazon.com/Game-Feel-Designers-Sensation-Kaufmann/dp/0123743281) — ✅ VERIFIED (published work)

The foundational book on game feel. Swink defines "game feel" as the real-time control of virtual objects, emphasizing three components:

1. **Real-time control** — the player acts, the game responds immediately
2. **Simulated space** — physics and movement feel consistent and believable
3. **Polish** — the layer of visual/audio/haptic feedback that amplifies perception

**Key concepts to apply**:
- **Input latency** kills feel — keep controller-to-action under 1 frame
- **Juicy feedback** is not decoration — it communicates state and reinforces mechanics
- **Tuning is design** — small value changes dramatically affect perceived weight and power

---

## Applied Principles in This Workflow

| Principle | How It's Applied |
|-----------|-----------------|
| Rule of Three (visual + audio + kinesthetic) | Required minimum per GFD Feedback Matrix row |
| Start exaggerated, tune down | Enforced in `game-feel-integrator/SKILL.md` — values are always dialed back from a bold starting point |
| Feel questions asked upfront | Step 3 of `/implement-feature` — never defer feel to Phase 5 |
| Shader-driven feel = valid feel channel | See `references/shaders-as-feel.md` — outline flash, dissolve, etc. are kinesthetic + visual |
| Hitstop profiles | `ShakeProfile` + `HitstopProfile` structs in SKILL.md — standardized across all features |
| ADSR timing sync across channels | Attack and Release durations must match across Visual, Audio, and Kinesthetic. If SFX fades over 0.5s, particles and screen shake must mirror that 0.5s. |

---

## Quick Reference: Feel Design Questions

Before implementing feel for any event, answer:
- **What sensation should this create?** (power, precision, vulnerability, speed, weight, magic...)
- **Which 3 channels carry it?** (visual, audio, kinesthetic — pick the dominant one first)
- **What's the timing signature?** (instant snap, short burst, sustained pulse, slow fade)
- **How does it contrast with the game's baseline feel?** (a subtle hit should feel different from a critical hit)
