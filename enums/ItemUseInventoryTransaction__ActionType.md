## ItemUseInventoryTransaction::ActionType

<table><tr><th>Name</th><th>Index</th><th>Notes</th><tr><td>Place</td><td>0</td><td>Right click item use on a surface like placing a block </td></tr><tr><td>Use</td><td>1</td><td>Start right click and hold style item use or potentially interact with nothing.
If it is a usable item like food the server is expected to send a SetActorDataPacket with ActorFlags::USINGITEM along with the transaction response.
While using an item, movement speed is slowed which will be reflected in the move vector in Player Auth Input. </td></tr><tr><td>Destroy</td><td>2</td><td>Block breaking like left click
When using server auth block breaking as specified in StartGamePacket this is never sent.
Instead, block actions are supplied in Player Auth Input. </td></tr></table>