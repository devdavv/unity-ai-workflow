---
name: Network Setup
description: Generates networking boilerplate adapted to the project's chosen networking package (NGO, Mirror, Photon).
---

# Network Setup

Generates networking code patterns adapted to the project's chosen package. **Always read `ProjectConfig.yaml → networking`** before generating any code.

## Prerequisites
- Read `docs/ProjectConfig.yaml → networking` to determine the package.
- Read `docs/TDD_Template.md` for the authority matrix (who owns what state).
- If `networking: "none"`, do NOT generate any networking code.

## Package-Specific Patterns

### Netcode for GameObjects (NGO)

**NetworkVariable for state sync:**
```csharp
using Unity.Netcode;

public class PlayerHealth : NetworkBehaviour
{
    [SerializeField] private NetworkVariable<int> _health = new(
        100,
        NetworkVariableReadPermission.Everyone,
        NetworkVariableWritePermission.Server
    );

    public override void OnNetworkSpawn()
    {
        _health.OnValueChanged += OnHealthChanged;
    }

    private void OnHealthChanged(int previous, int current)
    {
        // Update UI, play effects, etc.
    }

    [Rpc(SendTo.Server)]
    public void TakeDamageServerRpc(int amount)
    {
        if (!IsServer) return;
        _health.Value = Mathf.Max(0, _health.Value - amount);
    }
}
```

**Naming conventions for RPCs:**
- `[Rpc(SendTo.Server)]` — `{Action}ServerRpc`
- `[Rpc(SendTo.ClientsAndHost)]` — `{Action}ClientRpc`

### Mirror

**SyncVar for state sync:**
```csharp
using Mirror;

public class PlayerHealth : NetworkBehaviour
{
    [SyncVar(hook = nameof(OnHealthChanged))]
    private int _health = 100;

    private void OnHealthChanged(int oldValue, int newValue)
    {
        // Update UI, play effects, etc.
    }

    [Command]
    public void CmdTakeDamage(int amount)
    {
        _health = Mathf.Max(0, _health - amount);
    }
}
```

### Photon (PUN2 / Fusion)

**IPunObservable for state sync:**
```csharp
using Photon.Pun;

public class PlayerHealth : MonoBehaviourPunCallbacks, IPunObservable
{
    private int _health = 100;

    public void OnPhotonSerializeView(PhotonStream stream, PhotonMessageInfo info)
    {
        if (stream.IsWriting)
            stream.SendNext(_health);
        else
            _health = (int)stream.ReceiveNext();
    }

    [PunRPC]
    public void TakeDamage(int amount)
    {
        if (!photonView.IsMine) return;
        _health = Mathf.Max(0, _health - amount);
    }
}
```

## Authority Matrix Template

| Entity | Owner | Read | Write | Sync Method |
|--------|-------|------|-------|-------------|
| Player Health | Server | Everyone | Server | NetworkVariable |
| Player Position | Server | Everyone | Server | NetworkTransform |
| Chat Messages | Client | Everyone | Owner | RPC |

## Important Notes
- Always use **server-authoritative** patterns unless the TDD explicitly specifies peer-to-peer.
- Never trust client input — validate on the server.
- Check package version in ProjectConfig before using any API.
