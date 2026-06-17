# Party Docs

Version: 26.30

Documentation for the client's SocialDrawer PartyTab.

## Network Packets

Party information is included in the Login Connection Request, see the relevant fields 'PartyId' and 'IsPartyLeader'.

PartyChangedPacket: sent as soon as the party changes. Is an empty optional when it is deleted. It is not sent when you
join the server with a party - see connection request - but only if you create it yourself during the session.

SendPartyDestinationCookiePacket: Sent by the server to a client with a party destination cookie.

PartyDestinationCookieResponsePacket: Sent by the client to the server with a party destination cookie response.

## Multiplayer Party Service

https://secondary.multiplayer.minecraft-services.net/

### Searching

``POST api/v1.0/party/findJoinable``:

Joinable party search endpoint, returns a list of joinable parties.

```json
{
  "xboxToken": "XBOX TOKEN",
  "maxResults": 50,
  "includeFullParties": false
}
```

Example response:

```json
{
  "result": []
}
```

### Creation

Note that the party leader is a member too. PartyId format: ``{UUID V4}.r-20260323``

``POST api/v1.0/party/create``:

```json
{
  "privacy": "closed",
  "restrictInvitesToLeader": false
}
```

Example response:

```json
{
  "result": {
    "id": "PartyId",
    "connectionString": "party connection string",
    "leader": {
      "memberData": {
        "Pfid": "leader playfab id",
        "clientVersion": "leader client version",
        "Joined": "MM/dd/yyy HH:mm:ss"
      },
      "id": "leader id",
      "xuid": "leader XUID"
    },
    "privacy": "Closed",
    "maxPlayers": 15,
    "partyData": {
      "Created": "MM/dd/yyyy HH:mm:ss"
    },
    "members": [
      {
        "memberData": {
          "Pfid": "member playfab id",
          "clientVersion": "member client version",
          "Joined": "MM/dd/yyyy HH:mm:ss"
        },
        "id": "member id",
        "xuid": "member XUID"
      }
    ],
    "restrictInvitesToLeader": false
  }
}
```

**PlayFab Lobby Creation for the party:**

https://{ID}.playfabapi.com/Lobby/SubscribeToLobbyResource

See https://{ID}.playfabapi.com/Lobby/GetLobby, LobbyId = PartyId

### Destinations

Change the destination of the party. Local world example:

``POST /api/v1.0/party/ID/destination/p2p``

```json
{
  "xblSessionHandleId": "UUID v4",
  "destinationScanText": "My World"
}
```

Reset (also sent on party creation):
``DELETE /api/v1.0/party/ID/destination``

### Invites

Sends an invitation to another player.

``POST /api/v1.0/party/ID/invite``

```json
{
  "playerId": "invited player XUID",
  "xboxToken": "XBOX TOKEN"
}
```

### Leave

``POST /api/v1.0/party/ID/leave``

Party leave endpoint.