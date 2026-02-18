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

## Tools & Generators

| Tool | Purpose | Notes |
|------|---------|-------|
| [Aseprite](https://www.aseprite.org) | Pixel art & animation | Paid ($20), industry standard |
| [Piskel](https://www.piskelapp.com) | Pixel art (browser) | Free, quick prototyping |
| [Coolors](https://coolors.co) | Color palette generator | Free, great for design systems |
| [ShaderToy](https://www.shadertoy.com) | Shader inspiration | Free, community shader library |

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

## Adding New Tools

> If you find a new tool, tell the AI: *"I saw this tool [link/name] — is it viable for our Unity workflow?"*

The AI will assess:
1. **License** — Is it safe for commercial use?
2. **Unity integration** — Does it export Unity-compatible formats (FBX, PNG, WAV, OGG, etc.)?
3. **Quality level** — Prototype placeholder or production-ready?
4. **Recommendation** — Add to this doc, note as situational, or skip?
