# Authorization Service Documentation

Version: 1.21.101

Documentation for the Minecraft: Bedrock Edition authorization service.

Development of the new login system started during v1.21.90. The first login token payload has been seen in
v1.21.93 (819) which sent the login token payload for the first time. It was later removed.

https://authorization.franchise.minecraft-services.net/

## Session setup

``POST api/v1.0/session/start``

A request to initiate the authorization process by informing the service of various device and user-specific details.
The following example shows properties for the Windows platform. The capabilities list includes "RayTracing" if the
device supports it. Authentication with the Xbox Token (tokenType: Xbox) is also possible but the client seems to prefer
the PlayFab session ticket from the
corresponding [Client/LoginWithXbox](https://learn.microsoft.com/en-us/rest/api/playfab/client/authentication/login-with-xbox?view=playfab-rest)
request.

```json
{
  "device": {
    "applicationType": "MinecraftPE",
    "capabilities": [
      "RayTracing"
    ],
    "gameVersion": "",
    "id": "Device ID UUID v4",
    "isPreview": false,
    "memory": "1024",
    "platform": "Windows10",
    "playFabTitleId": "PlayFab TitleID",
    "storePlatform": "uwp.store",
    "treatmentOverrides": null,
    "type": "Windows10"
  },
  "user": {
    "language": "en",
    "languageCode": "en-US",
    "regionCode": "US",
    "token": "TOKEN",
    "tokenType": "PlayFab"
  }
}
```

Success Response:

```json
{
  "result": {
    "authorizationHeader": "MCToken TOKEN",
    "validUntil": "timestamp",
    "issuedAt": "timestamp",
    "treatments": [
      "treatment1",
      "treatment2"
    ],
    "configurations": {
      "minecraft": {
        "id": "minecraft",
        "parameters": {
          "PARAM": "VALUE"
        }
      }
    },
    "treatmentContext": "context"
  }
}
```

## Multiplayer Session Start

Used to retrieve the Login Token issued from the authorization service. See LoginPacket Token field. Requires the
MCToken authorization header from the initial session setup.

``POST api/v1.0/multiplayer/session/start``

Needs the Client Public Key, as used in ``multiplayer.minecraft.net/authenticate``.

```json
{
  "publicKey": "CLIENT PUBLIC KEY"
}
```

Success Response:

```json
{
  "result": {
    "signedToken": "TOKEN",
    "validUntil": "timestamp",
    "issuedAt": "timestamp"
  }
}
```

Version 26.30 no longer sends the chain certificate payload to servers.
Servers are expected to validate the Login Token for PlayerAuthenticationType==Full and Guest using the provided
configuration endpoint:
https://authorization.franchise.minecraft-services.net/.well-known/openid-configuration

Example claims:

| claim | description                   |
|-------|-------------------------------|
| aud   | audience                      |
| iss   | issuer                        |
| exp   | expiration timestamp seconds  |
| ipt   | Platform Type (PlayFab)       |
| iat   | issued timestamp seconds      |
| mid   | Minecraft ID                  |
| tid   | PlayFab Title ID              |
| pfcd  | PlayFab Account Creation Date |
| cpk   | Client Public Key             |
| ap    | Account Permissions           |
| xname | Display Name                  |
| xid   | XUID                          |