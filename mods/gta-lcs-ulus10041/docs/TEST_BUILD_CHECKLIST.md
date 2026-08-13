# ULUS-10041 Zombie Prototype Test-Build Checklist

## Build identity

Record the exact versions before testing. A passing build is meaningful only when the same versions can be reproduced.

| Field | Value to record |
|---|---|
| Game serial | `ULUS-10041` |
| Game revision/build | Record the PPSSPP game details or ISO metadata |
| PPSSPP version | Record exact version; CLEO PSP documentation requires 1.11.2 or higher for PSP game series |
| Android version/device | Record device model, Android version, RAM, and chipset |
| CLEO PSP version | Record exact release or commit |
| Sanny Builder version | Record exact version, preferably current official release |
| LCS edit mode | Confirm selected in Sanny Builder before compiling |
| Test save state | Keep a clean backup before each behavior test |

## Compilation checks

- [ ] Sanny Builder launches without configuration errors.
- [ ] The document is opened in the **LCS** edit mode, not GTA III, VC, SA, or VCS mode.
- [ ] LCS keyword definitions are present if required by the installed Sanny Builder mode.
- [ ] CLEO PSP custom opcode declarations are present if the compiler reports unknown `0DDx`, `0DFx`, or `1000` commands.
- [ ] `gtalcs.zombie_attack.txt` compiles as `.csi` with no errors or unexplained warnings.
- [ ] `gtalcs.roaming_horde.txt` compiles as `.csa` with no errors or unexplained warnings.
- [ ] The output filenames are exactly `gtalcs.zombie_attack.csi` and `gtalcs.roaming_horde.csa`.
- [ ] The source and binaries are kept together in a versioned test directory.

## Android installation checks

- [ ] PPSSPP’s memory-stick directory is identified.
- [ ] CLEO PSP is installed according to its official release instructions.
- [ ] The target directory exists: `PSP/PLUGINS/cleo/lcs/`.
- [ ] The compiled `.csi` and `.csa` files are copied into the `lcs` directory.
- [ ] The original game files are backed up and are not modified by the test package.
- [ ] The existing PPSSPP save and state files are backed up.
- [ ] PPSSPP cheats are disabled for the first CLEO-only test unless the specific test requires the survival preset.

## Runtime test order

### Test A: CLEO runtime detection

Start ULUS-10041 and verify that the CLEO menu or runtime indicator appears according to the installed CLEO PSP release. If the menu does not appear, stop here and fix installation before testing the scripts.

### Test B: Hostile pedestrian prototype

Invoke `gtalcs.zombie_attack.csi`. Observe whether ordinary pedestrians become hostile. Test for 60 seconds in a quiet area, then in a busier area. Record whether the player can still pause, load a save state, enter a vehicle, and exit the game normally.

Pass criteria:

- Pedestrians exhibit hostile behavior.
- No immediate freeze or crash occurs.
- The game remains controllable for at least 60 seconds.
- A clean save state can be restored.

### Test C: Six-pedestrian horde prototype

After Test B passes, enable `gtalcs.roaming_horde.csa` and restart from a clean save state. Confirm that no more than six prototype pedestrians are active at the initial spawn. Observe pursuit for ten minutes in at least two locations, including one open area and one dense area.

Pass criteria:

- The game loads into the world without a freeze.
- The horde remains capped at six tracked pedestrians.
- The pedestrians pursue the player rather than remaining permanently idle.
- Entering a vehicle, pausing, and loading a state does not permanently break the game.
- PPSSPP does not show sustained severe slowdown or audio desynchronization.
- Removing the `.csa` file and restarting restores normal gameplay.

### Test D: Survival preset interaction

Only after the CLEO prototypes are stable should the `ULUS10041.ini` survival preset be enabled. Test one cheat at a time, starting with health and armor. Then test ammunition and never-wanted behavior. Do not enable every code during the first combined test.

## Performance log

Record observations rather than relying on memory.

| Scenario | FPS/slowdown | Audio | Freeze/crash | Notes |
|---|---|---|---|---|
| Vanilla game, quiet area | | | | |
| Hostile pedestrian prototype | | | | |
| Six-pedestrian horde, open area | | | | |
| Six-pedestrian horde, dense area | | | | |
| Horde plus survival preset | | | | |

## Rollback procedure

If the game freezes or behaves incorrectly, force-close PPSSPP, remove the compiled `.csa` and `.csi` files from `PSP/PLUGINS/cleo/lcs/`, restore the clean save state, and restart without the cheat file. Do not continue using a save state created during a crash or uncontrolled-spawn condition.

If the problem occurs only with the horde script, retain the smaller `zombie_attack.csi` test and report the Android device, PPSSPP version, CLEO PSP version, game revision, location, and approximate time before failure. Do not increase the horde cap until the problem is understood.

## Acceptance status

- [ ] Compilation verified on a real Sanny Builder installation.
- [ ] CLEO runtime detected on Android PPSSPP.
- [ ] Hostile pedestrian prototype passed Test B.
- [ ] Six-pedestrian horde passed Test C.
- [ ] Survival preset passed Test D.
- [ ] Rollback procedure verified.
- [ ] Exact tested versions recorded in the project issue or release notes.

## References

[1]: https://sannybuilder.com/ "Sanny Builder official website and download"
[2]: https://tutorial.sannybuilder.com/sanny-builder/ "Sanny Builder official compilation tutorial"
[3]: https://github.com/cleolibrary/GTALCS.GTAVCS.PSP.CLEO "CLEO PSP / CLEO Android official source and documentation"
