---
name: uw-network-setup
description: Generates networking boilerplate adapted to the project's chosen networking package (NGO, Mirror, Photon). Use when setting up multiplayer systems, syncing state, writing RPCs, building lobbies, or designing an authority matrix. Triggers on requests like "add multiplayer", "sync player health", "create an RPC", "set up lobbies", "make this networked", "add co-op", "replicate this across clients", "spawn networked objects", "handle player disconnect", "create a matchmaking system", or any task involving network state synchronization, remote procedure calls, or client-server architecture. Always reads ProjectConfig.yaml -> networking before generating code. If networking is "none", do NOT generate networking code.
---

# Network Setup

Generate networking code adapted to the project's chosen package.

## Before You Start

1. Read `docs/ProjectConfig.yaml` for:
   - `networking` — the package name (`"ngo"`, `"mirror"`, `"photon"`, or `"none"`). If `"none"`, stop — do not generate networking code.
   - `architecture_pattern` — `"so-first"` or `"di-first"` (affects how network services are wired).
   - `mcp.unity_mcp` — if `true`, call `refresh_unity` after creating files.
2. Read `docs/TDD_Template.md` for the authority matrix (who owns what data).
3. Read `docs/CODING_STANDARDS.md` for async patterns (`Awaitable` + `CancellationToken`) and class structure.
4. Read `docs/NAMING_CONVENTIONS.md` for file/class naming.

## Package Quick Reference

Each package has a detailed reference file. Read the relevant one before generating code:

- **NGO (Netcode for GameObjects)**: [references/ngo.md](references/ngo.md) — Unity's first-party solution. Uses `NetworkVariable`, `[Rpc]` attributes.
- **Mirror**: [references/mirror.md](references/mirror.md) — Community open-source. Uses `[SyncVar]`, `[Command]`/`[ClientRpc]`.
- **Photon (PUN2/Fusion)**: [references/photon.md](references/photon.md) — Cloud-hosted. Uses `IPunObservable`, `[PunRPC]`.

## Core Patterns (All Packages)

### State Synchronization

Every package has a mechanism for syncing state from server/host to clients. The pattern is the same — only the syntax differs.

| Package | Sync Mechanism | Change Callback | Example |
|---------|---------------|-----------------|---------|
| **NGO** | `NetworkVariable<T>` | `.OnValueChanged += handler` | `_health.OnValueChanged += OnHealthChanged;` |
| **Mirror** | `[SyncVar(hook = nameof(...))]` | Hook method | `[SyncVar(hook = nameof(OnHealthChanged))]` |
| **Photon** | `IPunObservable` | `OnPhotonSerializeView` | Manual read/write in serialize callback |

### Remote Procedure Calls (RPCs)

RPCs let clients request actions on the server, or the server push updates to clients.

**NGO:**
```csharp
[Rpc(SendTo.Server)]
public void RequestDamageServerRpc(int amount)
{
    // Runs on server — validate before applying
    if (amount < 0 || amount > MAX_DAMAGE) return;
    _health.Value = Mathf.Max(0, _health.Value - amount);
}

[Rpc(SendTo.ClientsAndHost)]
public void PlayHitEffectClientRpc()
{
    // Runs on all clients — visual/audio only, no game state
}
```

**Mirror:**
```csharp
[Command]
public void CmdRequestDamage(int amount)
{
    if (amount < 0 || amount > MAX_DAMAGE) return;
    _health = Mathf.Max(0, _health - amount);
    RpcPlayHitEffect();
}

[ClientRpc]
public void RpcPlayHitEffect()
{
    // Visual/audio feedback on all clients
}
```

**Photon:**
```csharp
public void RequestDamage(int amount)
{
    photonView.RPC(nameof(RpcTakeDamage), RpcTarget.MasterClient, amount);
}

[PunRPC]
private void RpcTakeDamage(int amount, PhotonMessageInfo info)
{
    if (!PhotonNetwork.IsMasterClient) return;
    if (amount < 0 || amount > MAX_DAMAGE) return;
    _health = Mathf.Max(0, _health - amount);
}
```

### Network Object Spawning

Spawning networked objects follows the same principle: only the server/host creates the authoritative instance, and the network layer replicates it to clients.

**NGO:**
```csharp
// Server only — spawn a networked prefab
var instance = Instantiate(_projectilePrefab, spawnPoint.position, spawnPoint.rotation);
instance.GetComponent<NetworkObject>().Spawn();
```

**Mirror:**
```csharp
// Server only
var instance = Instantiate(_projectilePrefab, spawnPoint.position, spawnPoint.rotation);
NetworkServer.Spawn(instance);
```

**Photon:**
```csharp
// Room owner or any client (Photon handles ownership)
PhotonNetwork.Instantiate(_projectilePrefabName, spawnPoint.position, spawnPoint.rotation);
```

## Authority Matrix Template

Define who owns, reads, and writes each piece of networked state. Fill this in before writing code — it prevents authority bugs.

| Entity | Owner | Read | Write | Sync Method | Validation |
|--------|-------|------|-------|-------------|------------|
| Player Health | Server | Everyone | Server | NetworkVariable / SyncVar | Clamp 0-max, rate limit |
| Player Position | Server | Everyone | Server | NetworkTransform | Speed cap, teleport check |
| Player Input | Client | Server | Owner | RPC (client -> server) | Sanitize on server |
| Chat Messages | Client | Everyone | Owner | RPC | Length limit, rate limit |
| Game Timer | Server | Everyone | Server | NetworkVariable / SyncVar | Server-authoritative only |
| Inventory | Server | Owner | Server | RPC + NetworkVariable | Validate item exists |

## Abstraction Layer (Optional)

When the project might switch networking packages later, or when using DI (`architecture_pattern: "di-first"`), wrap networking behind an interface:

```csharp
public interface INetworkService
{
    bool IsServer { get; }
    bool IsClient { get; }
    bool IsConnected { get; }
    void SendToServer<T>(T message) where T : struct;
    void SendToClients<T>(T message) where T : struct;
}
```

Register the concrete implementation in the DI container (see `uw-dependency-injection`). Game systems depend on `INetworkService`, not the package directly. This makes testing easier too — mock the interface in EditMode tests.

## Custom Type Serialization

When syncing custom structs over the network, each package has its own serialization approach:

**NGO** — implement `INetworkSerializable`:
```csharp
public struct DamageEvent : INetworkSerializable
{
    public int Amount;
    public ulong AttackerId;

    public void NetworkSerialize<T>(BufferSerializer<T> serializer) where T : IReaderWriter
    {
        serializer.SerializeValue(ref Amount);
        serializer.SerializeValue(ref AttackerId);
    }
}
```

**Mirror** — implement custom `Reader`/`Writer` extensions or use `[SyncVar]` with supported types.

**Photon** — serialize manually in `OnPhotonSerializeView` or register custom types with `PhotonPeer.RegisterType`.

## Client Prediction & Lag Compensation

For responsive gameplay (especially movement and shooting), client-side prediction avoids waiting for server confirmation:

1. **Client predicts** — apply input locally immediately.
2. **Server validates** — process the input authoritatively, send correction.
3. **Client reconciles** — if server state differs from prediction, snap or interpolate to correct position.

This is complex. NGO has built-in `NetworkTransform` prediction. Mirror has community prediction examples. Photon Fusion has built-in state authority with rollback. For custom prediction, start with movement only and expand from there.

## After Setup

- **DI integration:** If `architecture_pattern: "di-first"`, use `uw-dependency-injection` to register network services in the container.
- **Write tests:** Use `uw-unity-test-runner` — test game logic in EditMode by mocking `INetworkService`. Test actual networking in PlayMode.
- **Authority matrix:** Fill in `docs/TDD_Template.md` with the authority matrix before writing any sync code.

## Rules

- **Server-authoritative** unless `docs/TDD_Template.md` explicitly specifies peer-to-peer.
- **Never trust client input** — always validate on the server (bounds check, rate limit, permission check).
- Cache `NetworkBehaviour` references in `Awake()` or `OnNetworkSpawn()` — never `GetComponent` in `Update`.
- Use `[SerializeField] private` for prefab references and configuration — never public fields.
- Check the package version in `ProjectConfig.yaml` before using any API — breaking changes happen between major versions.
- Use `Awaitable` with `CancellationToken` for async network operations (connection, scene loading) per `CODING_STANDARDS.md`.
- All networking code must live inside an `.asmdef`.
- If `ProjectConfig.yaml -> mcp.unity_mcp` is `true`, call `refresh_unity` after creating files.
