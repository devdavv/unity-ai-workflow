# NGO (Netcode for GameObjects) Patterns

## NetworkVariable for State Sync
```csharp
using Unity.Netcode;

public class PlayerHealth : NetworkBehaviour
{
    private readonly NetworkVariable<int> _health = new(
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
        // Update UI, play effects
    }

    [Rpc(SendTo.Server)]
    public void TakeDamageServerRpc(int amount)
    {
        if (!IsServer) return;
        _health.Value = Mathf.Max(0, _health.Value - amount);
    }
}
```

## RPC Naming
- `[Rpc(SendTo.Server)]` → `{Action}ServerRpc`
- `[Rpc(SendTo.ClientsAndHost)]` → `{Action}ClientRpc`
