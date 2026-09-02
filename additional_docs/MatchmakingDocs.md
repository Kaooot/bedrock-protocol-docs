# Matchmaking Packets (protocol 2207 / 1.26.60.21)

The two new matchmaking packets belong to Bedrock's Parties / Live Events system — the same area as the
`SendPartyDestinationCookie`, `PartyDestinationCookieResponse`, `ServerStoreInfo`,
`ServerPresenceInfo` and `PartyChanged` packets from the last few versions. They're not gameplay packets; they drive the
flow of finding and joining a featured-server destination.

## The packets

| Packet                               | ID  | Recipient | Body                                                            |
|--------------------------------------|-----|-----------|-----------------------------------------------------------------|
| `ClientboundMatchmakingStatePacket`  | 353 | CLIENT    | `MatchmakingState` state (u8) + Destination Name (string, ≤100) |
| `ServerboundMatchmakingCancelPacket` | 356 | SERVER    | *(empty)*                                                       |

`MatchmakingState`: `Idle` = 0, `Matchmaking` = 1, `MatchFound` = 2.

## What they do

When a player (or a whole party) asks to join a destination — a featured/partner server or live-service world — a
matchmaking ticket gets opened behind the scenes to place them.

`ClientboundMatchmakingStatePacket` is how the server keeps the client updated on that ticket. The state walks through
`Idle` → `Matchmaking` → `MatchFound`, and the destination name says where they're being matched. This is what backs the
Searching / Match found screen the player sees.

`ServerboundMatchmakingCancelPacket` is the player backing out — it cancels the pending ticket. There's no payload; the
packet itself is the signal.

So the whole thing is really just: kick off a search for a destination, watch the progress, and cancel it if you change
your mind. It sits right next to the party destination-cookie handshake rather than anything in normal play.

## Scripting and parties

The matchmaking flow isn't just wire traffic — it's wired into the server-side Script API so add-ons can react to it.
When a player sends the cancel packet, the engine turns it into a scripting after-event that add-on scripts can
subscribe to, so a script can run its own logic the moment someone backs out of matchmaking (clean up their queue slot,
notify their party, log it, etc.). Matchmaking lives in the engine's live-events utilities, which is the layer that
deals with joining featured/live-service destinations.

The tie to parties is the other half of the same feature. Parties have their own scripting hooks — most notably a party
destination-cookie response event that fires when the response to a
`SendPartyDestinationCookie` comes back. A destination cookie is the token identifying the server or realm the party
wants to travel to together, so it's what a whole party rides on to reach a destination.

Put together: a party agrees on a destination (the cookie handshake), the individual players get matchmade toward it
(the state packet), and either side can drop out (the cancel packet) — and each of those moments is exposed to scripts
as an event, so add-ons can drive the party/destination experience rather than leaving it entirely to the client UI.