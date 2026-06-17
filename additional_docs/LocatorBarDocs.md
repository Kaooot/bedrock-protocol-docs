# Server::WaypointPayload vanilla behavior analysis R26U1:

Version: 26.10

ServerWaypointGroup::Action is referred to as an "action"

## enum VanillaWaypointManagerConstants::ImageType:

* Added Square (0)
* Added Circle (1)
* Added SmallSquare (2)
* Added SmallStar (3)
* Added TinySquare (4) [couldn't find an official name, so I went with this one]
* Added TinyStar (5) [couldn't find an official name, so I went with this one]

* used for texture id, changes depending on the distance (client <--> waypoint), increases as the distance increases
    * starts at SmallSquare (2)

## Update Flag

* updates for everything that's not read-only
* mUpdateFlag is a bitset that informs about the changed fields
    * 0 for action=Remove (none/flags empty)
    * enum VanillaWaypointManagerConstants::UpdateFlag: [not an official naming]
        * Added WorldPos (0)
        * Added IsVisible (1)
        * Added TextureId (2)
        * Added Color (3) [untested]
        * Added ClientPositionAuthority (4)

## Notes

* three types of waypoints (location, actor, player): type determines which location the waypoint is linked to, actor
  and player are the only types that use the actorUniqueID field
* actorUniqueID is designed to be read-only which is why it can't be updated and doesn't have an update flag
* clientPositionAuthority seems to be true until the location is outside the client's render distance
* visibility rules can influence the isVisible value, see docs for EntityVisibilityRules or PlayerVisibilityRules.
  They're read-only too.