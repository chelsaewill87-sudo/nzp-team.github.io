# GTA LCS Zombie Survival Menu — Android PPSSPP

This folder contains a **CWCheat-style configuration** for a legally owned USA copy of *Grand Theft Auto: Liberty City Stories* with serial **ULUS-10041**. It does not contain the game, ISO, executable, textures, audio, or other copyrighted game assets.

## What this version does

The configuration provides a survival-oriented preset using verified baseline cheats: infinite health, infinite armor, never wanted, optional armed pedestrians, Molotovs, SMG and M16/AK ammunition, shotgun ammunition, and optional maximum wanted level.

This package now also includes two **original CLEO PSP source prototypes**. `gtalcs.zombie_attack.txt` invokes GTA LCS’s existing “Peds Attack You” cheat input (`112112LS`). `gtalcs.roaming_horde.txt` creates a capped group of six random pedestrians around the player and repeatedly assigns them the verified LCS kill-player-on-foot objective (`01CF`), producing a first Days Gone–inspired roaming/chase layer. These are source files that require compilation and in-game testing. They do not add custom zombie models, round counters, spawn-point zoning, barricades, perks, or advanced pathfinding yet.

| Menu entry | Purpose | Status |
|---|---|---|
| Infinite Health | Prevents normal player health depletion | Verified baseline code |
| Infinite Armor | Keeps armor value high | Verified baseline code |
| Never Wanted | Removes police pressure for survival testing | Verified baseline code |
| Pedestrians Have Weapons | Optional chaos/challenge setting | Verified baseline code |
| Molotovs / weapon ammo | Supplies survival weapons and ammunition | Verified baseline code |
| Hostile pedestrian prototype | Activates the game’s existing pedestrian-attack behavior | Included as CLEO PSP source; requires compilation and testing |
| Capped roaming horde | Spawns up to six nearby pedestrians and refreshes pursuit objectives | Included as CLEO PSP source; requires compilation and stability testing |
| Large persistent hordes, round logic, barricades, perks, and advanced pathfinding | Full Days Gone/NZ:P-style mode | Not yet implemented |

## CLEO prototype installation and build status

The `.txt` file is **source code**, not a ready-to-run `.csi` binary. Compile it with Sanny Builder using the CLEO PSP LCS opcode definitions described by the [CLEO PSP project](https://github.com/cleolibrary/GTALCS.GTAVCS.PSP.CLEO). The resulting `.csi` should be copied to `PSP/PLUGINS/cleo/lcs/` on the PPSSPP memory stick. CLEO PSP documentation lists ULUS10041 support and PPSSPP 1.11.2 or higher as requirements. Test it on a backup save and enable only one behavior at a time.

The prototypes deliberately use documented LCS/CLEO operations rather than fabricated memory addresses. The horde is capped at six pedestrians and refreshes objectives once per second to balance chase behavior against PPSSPP stability. Test on the exact ULUS-10041 build before increasing the cap or adding respawn waves.

## Installation on Android PPSSPP

First, back up your existing PPSSPP cheat file. In PPSSPP, enable cheats in **System settings**. Then open the game’s in-game pause/menu screen and choose **Cheats** followed by **Edit cheat file**, if available. Alternatively, copy `ULUS10041.ini` into PPSSPP’s `PSP/Cheats/` directory. On Android, the exact storage path depends on the PPSSPP storage-permission mode and your chosen memory-stick directory; use PPSSPP’s own “Edit cheat file” action when possible because it opens the correct file directly.

After copying the file, restart or reload the game, open its Cheats menu, and enable only the entries you want. Begin with **Infinite Health**, **Infinite Armor**, and one ammunition entry. Save the game before testing, because incompatible codes can cause instability. If the game freezes, disable the last code, reload the save state, and test one code at a time.

## Compatibility

This file targets the USA release **ULUS-10041**. Do not use it with `ULES`, `ULJM`, or another revision unless the addresses are independently verified for that build. A code that works on one region or revision may fail on another.

## Legal and distribution note

Use this configuration only with a game copy you are legally entitled to use. Do not redistribute the GTA LCS game or extracted game assets. This repository addition contains only user-facing configuration and documentation.

## References

[1]: https://almarsguides.com/retro/walkthroughs/psp/games/grandtheftautolibertycitystories/cwcheat/usa/ "Grand Theft Auto: Liberty City Stories CWCheats (USA)"
[2]: https://almarsguides.com/computer/programs/emulators/PPSSPP/HowToUsePSPCodes/ "How to Use & Add Codes on PPSSPP (v1.20.4) Emulator"

The baseline CWCheat entries and ULUS-10041 targeting were cross-checked against [1]. PPSSPP cheat enabling and `.ini` file workflow were cross-checked against [2].
