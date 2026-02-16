# Mirror Patterns

## SyncVar for State Sync
```csharp
using Mirror;

public class PlayerHealth : NetworkBehaviour
{
    [SyncVar(hook = nameof(OnHealthChanged))]
    private int _health = 100;

    private void OnHealthChanged(int oldValue, int newValue)
    {
        // Update UI, play effects
    }

    [Command]
    public void CmdTakeDamage(int amount)
    {
        _health = Mathf.Max(0, _health - amount);
    }
}
```

## RPC Naming
- `[Command]` → `Cmd{Action}` (client → server)
- `[ClientRpc]` → `Rpc{Action}` (server → clients)
- `[TargetRpc]` → `Target{Action}` (server → specific client)
