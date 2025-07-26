## PlayerActionType

<table><tr><th>Name</th><th>Index</th><th>Notes</th><tr><td>Unknown</td><td>-1</td><td>Unused </td></tr><tr><td>StartDestroyBlock</td><td>0</td><td>Sent in Player Auth Input Block Actions with position and facing </td></tr><tr><td>AbortDestroyBlock</td><td>1</td><td>Sent in Player Auth Input Block Actions with position and facing </td></tr><tr><td>StopDestroyBlock</td><td>2</td><td>Sent in Player Auth Input Block Actions without additional data </td></tr><tr><td>GetUpdatedBlock</td><td>3</td><td>Unused </td></tr><tr><td>DropItem</td><td>4</td><td>Unused </td></tr><tr><td>StartSleeping</td><td>5</td><td>Sent in Player Action </td></tr><tr><td>StopSleeping</td><td>6</td><td>Sent in Player Action </td></tr><tr><td>Respawn</td><td>7</td><td>Sent in Player Action </td></tr><tr><td>StartJump</td><td>8</td><td>Set on the tick that a player triggers a jump.
Corresponds to Player Auth Input InputData::StartJumping bit 31 </td></tr><tr><td>StartSprinting</td><td>9</td><td>Set when the player wants to start sprinting, like double tapping forward.
Server is expected to respond with SetActorDataPacket with ActorFlags::SPRINTING and an UpdateAttributesPacket to apply the sprint boost.
Corresponds to Player Auth Input InputData::StartSprinting bit 25 </td></tr><tr><td>StopSprinting</td><td>10</td><td>Sent when the player wants to stop sprinting, like releasing the forward input while sprinting.
Server is expected to respond with SetActorDataPacket with ActorFlags::SPRINTING and an UpdateAttributesPacket clearing the sprint speed boost.
Corresponds to Player Auth Input InputData::StopSprinting bit 26 </td></tr><tr><td>StartSneaking</td><td>11</td><td>Sent when the player wants to start sneaking like pressing shift.
Server is expected to respond with SetActorDataPacket with ActorFlags::SNEAKING true if accepted, false if rejected, and a bounding box update.
Corresponds to Player Auth Input InputData::StartSneaking bit 27 </td></tr><tr><td>StopSneaking</td><td>12</td><td>Sent when the player wants to stop sneaking like releasing shift.
Server is expected to respond with SetActorDataPacket with ActorFlags::SNEAKING false if accepted, true if rejected, and a bounding box update.
Corresponds to Player Auth Input InputData::StopSneaking bit 28 </td></tr><tr><td>CreativeDestroyBlock</td><td>13</td><td>Sent when trying to destroy a block in creative like left clicking on it. Expects server to destroy the block and optionally send new block or chunk information.
Used to be a ChangeDimension action.
Sent in Player Action. </td></tr><tr><td>ChangeDimensionAck</td><td>14</td><td>Sent in Player Action, this is the one case of the server sending an action to the client to start a dimension change. </td></tr><tr><td>StartGliding</td><td>15</td><td>Sent when the player wants to start elytra gliding like pressing spacebar in air.
Server is expected to respond with SetActorDataPacket with ActorFlags::GLIDING true if accepted or false if rejected, and a bounding box update.
Corresponds to Player Auth Input InputData::StartGliding bit 32 </td></tr><tr><td>StopGliding</td><td>16</td><td>Sent when the player is elytra gliding but expects it to stop, like when touching the ground.
Server is expected to respond with SetActorDataPacket with ActorFlags::GLIDING false if accepted or true if rejected, and a bounding box update.
Corresponds to Player Auth Input InputData::StopGliding bit 33 </td></tr><tr><td>DenyDestroyBlock</td><td>17</td><td>Sent when the client thinks they aren't allowed to break a block at the location and want the deny particle effect.
Sent in Player Action in EDU </td></tr><tr><td>CrackBlock</td><td>18</td><td>Client expects a LevelEventPacket with the appropriate crack event to be broadcast in response.
Only sent if server auth block breaking is disabled in StartGamePacket.
Sent in Player Auth Input Block Actions with position and facing. </td></tr><tr><td>ChangeSkin</td><td>19</td><td>Unused </td></tr><tr><td>DEPRECATED_UpdatedEnchantingSeed</td><td>20</td><td>Sent in Player Action if ItemStackNetManager is disabled </td></tr><tr><td>StartSwimming</td><td>21</td><td>Sent when the player wants to enter swimming mode like pressing control while moving forward in water.
Server is expected to respond with SetActorDataPacket with ActorFlags::SWIMMING set to true if accepted or false if rejected, and a bounding box update.
Corresponds to Player Auth Input InputData::StartSwimming bit 29 </td></tr><tr><td>StopSwimming</td><td>22</td><td>Sent when the player wants to exit swimming mode like when releasing the forward input while swimming.
Server is expected to respond with SetActorDataPacket with ActorFlags::SWIMMMING set to false if accepted or true if rejected, and a bounding box update.
Corresponds to Player Auth Input InputData::StopSwimming bit 30 </td></tr><tr><td>StartSpinAttack</td><td>23</td><td>Sent on the tick that the client predicts a riptide spin attack starting. It is accompanied by an InventoryTransactionPacket of type ComplexInventoryTransaction::Type::ItemUseTransaction.
Server is expected to send a SetActorDataPacket with ActorFlags::DAMAGENEARBYMOBS set to true if accepted or false if rejected along with a bounding box update.
Sent in Player Action but will soon turn into a Player Auth Input InputData bit </td></tr><tr><td>StopSpinAttack</td><td>24</td><td>Sent when the client thinks a riptide spin attack has ended.
Server is expected to send a SetActorDataPacket with ActorFlags::DAMAGENEARBYMOBS set to false if accepted or true if rejected along with a bounding box update.
Sent in Player Action but will soon turn into a Player Auth Input InputData bit </td></tr><tr><td>InteractWithBlock</td><td>25</td><td>Unused </td></tr><tr><td>PredictDestroyBlock</td><td>26</td><td>Sent in Player Auth Input Block Actions with position and facing.
Used for the client to inform the server that it predicted the player destroying a block.
The server may respond with block, chunk, or item information if it disagrees, or send no response to imply agreement.
Only used when server-auth block breaking toggle is on as specified in StartGamePacket </td></tr><tr><td>ContinueDestroyBlock</td><td>27</td><td>Sent in Player Auth Input Block Actions with position and facing.
Used to inform the server that the client's current block changed for block destruction.
The server is expected to use this to progress the block destruction and await an upcoming PredictDestroyBlock action.
They are also expected to broadcast LevelEventPackets for the block cracking of the block being destroyed.
Only sent when server-auth block breaking toggle is on as specified in StartGamePacket </td></tr><tr><td>StartItemUseOn</td><td>28</td><td>Sent upon starting right click and hold style item use.
Sent in Player Action.
Server can expect this to arrive with an InventoryTransactionPacket with ItemUseInventoryTransaction in it.</td></tr><tr><td>StopItemUseOn</td><td>29</td><td>Sent upon releasing right click and hold style item use. This is for canceling the action, not the same as firing a bow which would be InventoryTransactionPacket with ItemUseInventoryTransaction.
Sent in Player Action </td></tr><tr><td>HandledTeleport</td><td>30</td><td>Used to inform the server that we have received a MovePlayerPacket causing a teleport, and re-enable client auth movement.
The server should ignore any client predicted positions from the moment a MovePlayerPacket was sent until receipt of this action.
Corresponds to Player Auth Input InputData::HandledTeleport bit 37 </td></tr><tr><td>MissedSwing</td><td>31</td><td>Sent when client wants to play the arm swing animation like for left click.
Server is expected to broadcast a LevelSoundEventPacket with LevelSoundEvent::AttackNoDamage.
Corresponds to Player Auth Input InputData::MissedSwing bit 39 </td></tr><tr><td>StartCrawling</td><td>32</td><td>Sent when the player is standing and thinks there is not enough space to stand.
Server is expected to respond with a SetActorDataPacket containing a bounding box update.
Server is expected to respond with SetActorDataPacket with ActorFlags::CRAWLING set to true if accepted, or false if rejected.
Corresponds to Player Auth Input InputData::StartCrawling bit 40 </td></tr><tr><td>StopCrawling</td><td>33</td><td>Sent when the player was crawling and thinks there is space to stand.
Server is expected to respond with a SetActorDataPacket containing a bounding box update.
Server is expected to respond with SetActorDataPacket with ActorFlags::CRAWLING set to false if accepted, or true if rejected.
Corresponds to Player Auth Input InputData::StopCrawling bit 41 </td></tr><tr><td>StartFlying</td><td>34</td><td>Sent when the player expects flight to be toggled on like double tap spacebar.
Server is expected to respond with an UpdateAbilitiesPacket to accept or reject this.
Corresponds to Player Auth Input InputData::StartFlying bit 42 </td></tr><tr><td>StopFlying</td><td>35</td><td>Sent when the player expects flight to be toggled off like double tap spacebar.
Server is expected to respond with an UpdateAbilitiesPacket to accept or reject this.
Corresponds to Player Auth Input InputData::StopFlying bit 43 </td></tr><tr><td>DEPRECATED_ClientAckServerData</td><td>36</td><td>Corresponds to Player Auth Input InputData::ClientAckServerData bit 44
Not sent when using server authoritative movement as specified in StartGamePacket
This is now deprecated because only server authoritative movement exist </td></tr><tr><td>StartUsingItem</td><td>37</td></tr><tr><td>Count</td><td>38</td><td>Unused</td></tr></table>