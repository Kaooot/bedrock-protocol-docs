## ServerAuthMovementMode

<table><tr><th>Name</th><th>Index</th><th>Notes</th><tr><td>LegacyClientAuthoritativeV1_Deprecated</td><td>0</td></tr><tr><td>ClientAuthoritativeV2</td><td>1</td><td>Referred to in the rest of the documentation as 'Client Authoritative'.
This mode is the current default with previews coming soon for migrating to server authoritative
The packets sent from client to server are largely the same as for server authoritative, see their documentation for details:
- PlayerAuthInputPacket (Primary motion and input data)
- InventoryTransactionPacket (Item use, tangentially movement related)

PlayerActionPacket is sent in some cases, and in others has become a bit inside of PlayerAuthInputPacket.

The client can be repositioned with:
- MovePlayerPacket
- SetActorMotionPacket
</td></tr><tr><td>ServerAuthoritativeV3</td><td>2</td><td>Referred to in the rest of the documentation as 'Server Authoritative'
This mode is intended to become the new default after previews coming soon.
The packets from client to server are similar.
- PlayerAuthInputPacket (Primary motion and input data)
- InventoryTransactionPacket (Item use, tangentially movement related)

PlayerActionPacket is sent in some cases, and in others has become a bit inside of PlayerAuthInputPacket.

The client can be repositioned with:
- MovePlayerPacket
- CorrectPlayerMovePredictionPacket
- SetActorMotionPacket

Additionally, in this mode many client-bound packets have a 'Tick' value. These echo back the tick value that the client supplies in the PlayerAuthInputPacket.
For packets relating to a player or client predicted vehicle, the tick value should be that of the most recently processed PlayerAuthInputPacket from the player.
Specifying zero is also acceptable although may result in minor visual flickering as it may confuse client predicted actions.
</td></tr></table>