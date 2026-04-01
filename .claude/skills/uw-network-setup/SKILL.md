---
name: uw-network-setup
description: Generates networking boilerplate adapted to the project's chosen networking package (NGO, Mirror, Photon). Use when setting up multiplayer systems, syncing state, writing RPCs, or building an authority matrix. Triggers on requests like "add multiplayer", "sync player health", "create an RPC", "set up lobbies", or any networking task. Always reads ProjectConfig.yaml → networking before generating code. If networking is "none", do NOT generate networking code.
---

# Network Setup

Generate networking code adapted to the project's chosen package.

## Before You Start
1. Read `docs/ProjectConfig.yaml → networking` to determine the package.
2. Read `docs/TDD_Template.md` for the authority matrix.
3. If `networking: "none"`, do NOT generate any networking code.

## Package-Specific Patterns
- **NGO (Netcode for GameObjects)**: See [references/ngo.md](references/ngo.md)
- **Mirror**: See [references/mirror.md](references/mirror.md)
- **Photon (PUN2/Fusion)**: See [references/photon.md](references/photon.md)

## Authority Matrix Template

| Entity | Owner | Read | Write | Sync Method |
|--------|-------|------|-------|-------------|
| Player Health | Server | Everyone | Server | NetworkVariable / SyncVar |
| Player Position | Server | Everyone | Server | NetworkTransform |
| Chat Messages | Client | Everyone | Owner | RPC |

## Rules
- Always **server-authoritative** unless TDD specifies peer-to-peer.
- Never trust client input — validate on the server.
- Check package version in ProjectConfig before using any API.
