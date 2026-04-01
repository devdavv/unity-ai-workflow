# Photon (PUN2 / Fusion) Patterns

## PUN2: IPunObservable for State Sync
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

## RPC Naming
- `[PunRPC]` — called via `photonView.RPC("MethodName", RpcTarget.All, args)`
