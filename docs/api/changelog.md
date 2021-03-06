---
id: changelog
title: Changelog
sidebar_label: Changelog
---

Follow this entry for ongoing updates and changes to the project or tools:

## Version 0.136 (September 2019)

* [Teamfight Tactics Events](overwolf-games-events-tft#docsNav)
  * New info update:
    * local_player_damage

## Version 0.134 (August 2019)

* New API: [overwolf.profile.subscription](overwolf-profile.subscription).  
  Provides functions and events to help with user subscription management.
  
* [Teamfight Tactics Events](https://overwolf.github.io/docs/api/overwolf-games-events-tft#docsNav)
  * New updates:
    * store
    * board
    * bench
    * carousel
    
* [League of Legends Launcher Events](https://overwolf.github.io/docs/api/overwolf-games-launchers-events-lol#docsNav)
  * New update:
    * end_game
    
* [Hearhstone game events](https://overwolf.github.io/docs/api/overwolf-games-events-heartstone#docsNav)
  * New events:
    * match_type
    * match_start/end
    * match_outcome
* [Rainbow Six: Siege game events](https://overwolf.github.io/docs/api/overwolf-games-events-rainbow-six#docsNav)
  * New update:
    * game_mode
* [CS: GO game events](https://overwolf.github.io/docs/api/overwolf-games-events-csgo#docsNav)
  * New update:
    * replay
    * server_info
    * game_mode
* [PUBG game events](https://overwolf.github.io/docs/api/overwolf-games-events-pubg#docsNav)
  * New update
    * team_location
    * health

## Version 0.133 (July 2019)

* [minimum-gep-version](manifest-json#meta-minimum-gep) - New manifest flag. Allow extensions to set a minimum GEP version in manifest, this works similarly to minimum-overwolf-version.

* [overwolf.extensions.updateExtension()](overwolf-extensions#updateextensioncallback) - new method. Tries to download an update for the calling extension.

* [overwolf.extensions.onExtensionUpdateStateChanged](overwolf-extensions#onextensionupdatestatechanged) - new event. Notify when the app was updated.

## Version 0.132 (July 2019)

* [overwolf.os.getRegionInfo()](overwolf-os#getregioninfocallback) - new method. Returns regional information about the user.
  
* [overwolf.windows.setMinSize()](overwolf-windows#setminsizewindowid-width-height-callback) - new method. Overrides the window's defined minimum size.

* [Teamfight Tactics Game Events](overwolf-games-events-tft) - TFT game events are now available. This game-mode is officially supported and more events will be added soon!

## Version 0.131 (June 2019)

* [LoL Launcher events](overwolf-games-launchers-events-lol)
  * New update:
    * lobby_info
* [PUBG game events](overwolf-games-events-pubg)
  * New updates:
    * safe_zone
    * blue_zone
    * red_zone
* [Fortnite game events](overwolf-games-events-fortnite)
  * New update:
    * shield
