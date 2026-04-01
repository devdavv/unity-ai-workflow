# Asset Resources Guide

> **For AI agents**: When suggesting assets to the user, reference this list. Always mention whether an asset is free or paid, and link to the storefront.

---

## 2D Art

| Source | Type | Notes |
|--------|------|-------|
| [Unity Asset Store](https://assetstore.unity.com) | Free + Paid | Official marketplace, largest selection |
| [Kenney.nl](https://kenney.nl/assets) | Free (CC0) | Clean, consistent art packs. Great for prototyping |
| [Itch.io](https://itch.io/game-assets) | Free + Paid | Indie artists, huge variety, affordable |
| [OpenGameArt](https://opengameart.org) | Free (CC) | Community-contributed, varying quality |
| [GameArt2D](https://www.gameart2d.com) | Free + Paid | Polished sprite sheets and UI kits |
| [CraftPix](https://craftpix.net) | Free + Paid | Game-ready 2D assets, GUI, tilesets |

## 3D Art

| Source | Type | Notes |
|--------|------|-------|
| [Unity Asset Store](https://assetstore.unity.com) | Free + Paid | Best Unity-native integration |
| [Sketchfab](https://sketchfab.com) | Free + Paid | Massive library, preview in 3D before downloading |
| [Quaternius](https://quaternius.com) | Free (CC0) | Low-poly packs, consistent style |
| [Mixamo](https://www.mixamo.com) | Free | Character models + animations (Adobe account required) |
| [Poly Pizza](https://poly.pizza) | Free (CC) | Low-poly models, Google Poly successor |
| [Turbosquid](https://www.turbosquid.com) | Free + Paid | Professional quality, higher price point |

## Audio — SFX

| Source | Type | Notes |
|--------|------|-------|
| [Freesound](https://freesound.org) | Free (CC) | Massive community library, filter by license |
| [Sonniss GDC Bundle](https://sonniss.com/gameaudiogdc) | Free | Annual GDC bundle, professional quality |
| [Mixkit](https://mixkit.co/free-sound-effects/) | Free | Curated, no attribution needed |
| [ZapSplat](https://www.zapsplat.com) | Free + Paid | Large library, free with attribution |
| [SoundSnap](https://www.soundsnap.com) | Paid (subscription) | Premium quality, unlimited downloads |

## Audio — Music

| Source | Type | Notes |
|--------|------|-------|
| [Incompetech](https://incompetech.com/music/) | Free (CC BY) | Kevin MacLeod's library, widely used |
| [Uppbeat](https://uppbeat.io) | Free + Paid | Royalty-free, curated for creators |
| [Epidemic Sound](https://www.epidemicsound.com) | Paid (subscription) | High-quality, cleared for commercial use |
| [Artlist](https://artlist.io) | Paid (subscription) | Premium production music |

## Fonts

| Source | Type | Notes |
|--------|------|-------|
| [Google Fonts](https://fonts.google.com) | Free (OFL) | Standard go-to, excellent for UI |
| [DaFont](https://www.dafont.com) | Free + Paid | Game-style fonts, check license per font |
| [Font Squirrel](https://www.fontsquirrel.com) | Free (commercial) | Pre-filtered for commercial use |
| [Fontshare](https://www.fontshare.com) | Free | ITF-curated, modern quality |

## UI Kits & Icons

| Source | Type | Notes |
|--------|------|-------|
| [Figma Community](https://www.figma.com/community) | Free | UI kits, wireframes, design systems |
| [Material Symbols](https://fonts.google.com/icons) | Free | Google's icon set, scalable |
| [Flaticon](https://www.flaticon.com) | Free + Paid | Massive icon library |
| [Game-Icons.net](https://game-icons.net) | Free (CC BY) | RPG/game-specific icons |
| [Iconify](https://iconify.design) | 200k+ icons, multiple icon sets | Free; Figma plugin available; [UNVERIFIED: verify Figma/Pencil plugin integration] |

## Tools & Generators

| Tool | Purpose | Notes |
|------|---------|-------|
| [Aseprite](https://www.aseprite.org) | Pixel art & animation | Paid ($20), industry standard |
| [Piskel](https://www.piskelapp.com) | Pixel art (browser) | Free, quick prototyping |
| [Coolors](https://coolors.co) | Color palette generator | Free, great for design systems |
| [ShaderToy](https://www.shadertoy.com) | Shader inspiration | Free, community shader library |

---

## Shaders & Procedural Graphics

> Use shaders as a **primary graphics strategy** — not just Phase 5 polish. Shaders can replace sprites, textures, and many 3D assets entirely for stylized or minimal-art games. This approach gives full creative control with no external art pipeline dependencies.
>
> **Portable stack** — built-in Unity only (no paid packages required):

| Approach | When to Use | Notes |
|----------|------------|-------|
| **URP Shader Graph** | Most cases — 2D effects, stylized materials, UI glow | Node-based, URP/HDRP native; no code required; preferred starting point |
| **Custom HLSL shaders** | Performance-critical or highly custom effects | Vertex/fragment; full control; steeper learning curve |
| **URP Render Features** | Fullscreen effects (outline, bloom, chromatic aberration, scanlines) | Applied as post-process pass; see URP Renderer Feature docs |
| **Shader Graph + VFX Graph** | Particle systems driven by shader logic | VFX Graph is the approved particle tool |
| [ShaderToy](https://www.shadertoy.com) | GLSL inspiration library | Free; port to HLSL manually; filter by complexity |

### Learning Resources [UNVERIFIED: check for current Unity 6 compatibility]
| Resource | Focus | Notes |
|----------|-------|-------|
| [Catlike Coding](https://catlikecoding.com/unity/tutorials/) | HLSL from scratch, URP pipeline deep dives | Free; most thorough Unity shader tutorials available |
| [Cyanilux](https://www.cyanilux.com/tutorials/) | Shader Graph + URP practical breakdowns | Free; author posts on 2D water, outlines, dissolves |
| [Freya Holmér](https://www.youtube.com/@acegikmo) | Math for shaders (SDFs, procedural geometry) | Free; essential for procedural shape generation |

### Optional Owned Packages
If you own these, they extend the built-in stack significantly — but the workflow does not require them for portability:

| Package | What It Does | Owner Note |
|---------|-------------|------------|
| [Shapes (Freya Holmér)](https://assetstore.unity.com/packages/tools/particles-effects/shapes-173167) | Real-time vector graphics library — draw lines, arcs, circles, polygons in code | Paid; top-4 Unity paid asset 2023; add `has_shapes: true` to ProjectConfig if owned |
| [True Shadow](https://assetstore.unity.com/packages/tools/gui/true-shadow-ui-soft-shadow-and-glow-205220) | Soft drop shadows + glow for uGUI/UI Toolkit | Paid; far superior to Unity's built-in shadow component; add `has_true_shadow: true` to ProjectConfig if owned |

### Shader-First Game Feel
Shaders are also valid **game feel tools** — not just visuals. See `.claude/skills/uw-game-feel-integrator/references/shaders-as-feel.md` for:
- Outline flash on hit
- Dissolve on death / spawn
- Chromatic aberration on damage
- UV scroll on charged/powered state
- Screen-space distortion on impact

---

## AI Generation Tools

> Use these to generate placeholder or production assets without manual creation.
> **Always verify license terms before using AI-generated assets commercially.**
> Mark AI-generated placeholders in code with `// ASSET: [description needed]`.

### AI Image / Sprite Generation

| Tool | Purpose | Notes |
|------|---------|-------|
| [Midjourney](https://midjourney.com) | Concept art, 2D sprites, backgrounds | Paid subscription; highest quality output |
| [Adobe Firefly](https://firefly.adobe.com) | Sprites, textures, UI elements | Commercial-safe by design (Adobe CC license) |
| [Leonardo.AI](https://leonardo.ai) | Game sprites, character sheets, tile sets | Free tier available; game-asset focused models |
| [Stable Diffusion](https://stability.ai) | Any image generation | Open source; self-hostable via ComfyUI or Automatic1111 |
| [Bing Image Creator](https://www.bing.com/images/create) | Quick concept art | Free; powered by DALL-E; good for rapid ideation |
| [Fal.ai](https://fal.ai) | Fast AI image generation via MCP | Free tier + paid; SDXL/Flux models; MCP-connectable for automated generation |
| [Replicate](https://replicate.com) | Hosted ML models including image gen | Pay-per-run; MCP-connectable; good for batch texture generation |

### AI 3D Model Generation

| Tool | Purpose | Notes |
|------|---------|-------|
| [Meshy](https://meshy.ai) | Text/image → 3D mesh | Game-ready exports (FBX/GLB), free tier available |
| [Kaedim](https://kaedim3d.com) | 2D image → rigged 3D model | Higher fidelity, paid |
| [Luma AI Genie](https://lumalabs.ai/genie) | Text → 3D | Free browser tool, good for props |
| [Sloyd](https://www.sloyd.ai) | Procedural 3D game assets | Unity plugin available; low-poly game assets |

### AI Audio / SFX Generation

| Tool | Purpose | Notes |
|------|---------|-------|
| [ElevenLabs](https://elevenlabs.io) | Voice acting, character TTS, narration | Best voice quality available; free tier |
| [ElevenLabs Sound Effects](https://elevenlabs.io/sound-effects) | SFX from text description | e.g. "sword swoosh with reverb"; free tier |
| [Adobe Podcast Enhance](https://podcast.adobe.com/enhance) | Clean up and denoise recorded audio | Free; great for cleaning placeholder recordings |
| [AudioCraft (Meta)](https://github.com/facebookresearch/audiocraft) | SFX and music from text | Open source; self-hosted |

### AI Music Generation

| Tool | Purpose | Notes |
|------|---------|-------|
| [Suno](https://suno.com) | Full music tracks from text prompt | Best quality; free tier (non-commercial) |
| [Udio](https://www.udio.com) | Full music tracks from text prompt | Strong alternative to Suno |
| [Mubert](https://mubert.com) | Royalty-free AI music by genre/mood | API available; designed for commercial use |

---

## Production Tools

Tools for managing your project, workflow, and team — not game assets, but essential to shipping.

### Audio Management
| Tool | Purpose | Notes |
|------|---------|-------|
| [Soundly](https://www.soundly.com) | SFX library browser + manager | Industry standard for sound designers; search, preview, drag-and-drop to Unity |
| [SoundMiner](https://store.soundminer.com) | Local SFX library management | Paid; for teams with large private SFX libraries |

### Project Management & Documentation
| Tool | Purpose | Notes |
|------|---------|-------|
| [Linear](https://linear.app) | Sprint planning, issue tracking | Fast, keyboard-first; integrates via Linear MCP |
| [Notion](https://www.notion.so) | Docs, wikis, design documents | Flexible; integrates via Notion MCP |
| [Jira](https://www.atlassian.com/software/jira) | Issue tracking (larger teams) | Heavy but standard in studios |
| [Trello](https://trello.com) | Kanban boards (solo/small teams) | Free; simple visual boards |
| [Miro](https://miro.com) | Visual game design boards, mind maps | Free tier; great for feature ideation and system diagrams |

### Version Control
| Tool | Purpose | Notes |
|------|---------|-------|
| [GitHub Desktop](https://desktop.github.com) | Git GUI | Free; easiest on-ramp for non-technical team members |
| [GitKraken](https://www.gitkraken.com) | Git GUI with visual history | Free for public repos; good for complex branch visualization |
| [Sourcetree](https://www.sourcetreeapp.com) | Git GUI (Atlassian) | Free; Windows/Mac |

### Communication & Community
| Tool | Purpose | Notes |
|------|---------|-------|
| [Discord](https://discord.com) | Team comms + game dev communities | Free; `r/gamedev`, `r/unity`, itch.io servers for feedback |

### Build & CI
| Tool | Purpose | Notes |
|------|---------|-------|
| [GitHub Actions](https://github.com/features/actions) | Automated build + test pipelines | Free for public repos; Unity-compatible runners available |
| [Unity Cloud Build](https://unity.com/products/unity-devops) | Cloud build for Unity projects | Managed service; paid |
| [GameCI](https://game.ci) | Open-source Unity CI/CD for GitHub Actions | Free; recommended Unity CI setup |

---

## Visual Communication Tools

> Use these to sketch, wireframe, or mockup **before** coding. Claude Code will suggest the right tool based on context — you don't need to choose upfront.

| Context | Tool | Notes |
|---------|------|-------|
| **Feature behavior sketch** (how a mechanic flows, state diagram, input → output) | [Miro](https://miro.com) | Free tier; infinite canvas; great for system diagrams and feature flows |
| **Quick offline sketch** | [Excalidraw](https://excalidraw.com) | Free, no account needed; hand-drawn feel; exports to PNG |
| **UI screen mockup / screen flow** | [Pencil.dev](https://pencil.dev) | Free (requires Claude Code sub); design-on-canvas → exports HTML/CSS/React; not Unity-native but good for UI layout ideation |
| **UI screen mockup (Figma)** | [Figma Community](https://www.figma.com/community) | Free tier; game UI kits available; better for pixel-accurate UI mockups |
| **Preprod game overview** (all screens + connections) | [Miro](https://miro.com) | System diagram view; link to GDD sections |
| **Icon sourcing** | [Iconify](https://iconify.design) | 200k+ icons; Figma plugin [UNVERIFIED: verify Pencil.dev plugin] |

> **Behaviour**: In Step 2 of `/uw-cmd-implement-feature`, if the feature involves new UI or a complex behavior flow, Claude Code will ask: *"Would a quick sketch help clarify this before we code? [Miro for system flow / Excalidraw for quick diagram / Pencil for UI layout]"* — answer with the tool name or 'no'.

---

## Notable Unity Packages

> Reference these when recommending packages during `/uw-cmd-setup-project` or feature planning.
> Always check version against `ProjectConfig.yaml` before using any API.

### Editor & Inspector Tools
| Package | Purpose | Notes |
|---------|---------|-------|
| [Odin Inspector](https://odininspector.com) | Powerful custom inspector system | Paid (~$55); reduces custom editor code significantly |
| [Tri Inspector](https://github.com/codewriter-packages/Tri-Inspector) | Odin-style inspector attributes | Free (MIT); great Odin alternative |
| [NaughtyAttributes](https://github.com/dbrizov/NaughtyAttributes) | Lightweight inspector attributes | Free; simpler than Odin/Tri |

### Gameplay & Architecture
| Package | Purpose | Notes |
|---------|---------|-------|
| [DOTween](http://dotween.demigiant.com) | Tween engine | Free (Pro paid); most widely used; see `references/dotween.md` |
| [PrimeTween](https://github.com/KyryloKuzyk/PrimeTween) | Zero-allocation tween engine | Free; Unity 6 native feel; see `references/primetween.md` |
| [UniTask](https://github.com/Cysharp/UniTask) | Async/await for Unity | Free; use if `Awaitable` isn't sufficient |
| [VContainer](https://vcontainer.hadashikick.jp) | Dependency injection | Free; lightweight, high-performance |
| [Reflex](https://github.com/gustavopsantos/Reflex) | Dependency injection | Free (MIT); fastest DI for Unity; minimal API |
| [Init(args)](https://assetstore.unity.com/packages/tools/utilities/init-args-200530) | DI + MonoBehaviour initialization | Paid; integrates with Unity's component model natively |
| ~~Feel (More Mountains)~~ | ~~Game feel feedback system~~ | Removed — editor-heavy, not AI-friendly; use DOTween/PrimeTween + custom code instead |

### Utilities
| Package | Purpose | Notes |
|---------|---------|-------|
| [Sirenix Odin Serializer](https://github.com/TeamSirenix/odin-serializer) | Advanced Unity serialization | Free (Apache 2.0); for complex data structures |
| [UniRx](https://github.com/neuecc/UniRx) | Reactive extensions for Unity | Free; useful for event-driven architecture |

---

## Adding New Tools

> If you find a new tool, tell the AI: *"I saw this tool [link/name] — is it viable for our Unity workflow?"*

The AI will assess:
1. **License** — Is it safe for commercial use?
2. **Unity integration** — Does it export Unity-compatible formats (FBX, PNG, WAV, OGG, etc.)?
3. **Quality level** — Prototype placeholder or production-ready?
4. **Recommendation** — Add to this doc, note as situational, or skip?
