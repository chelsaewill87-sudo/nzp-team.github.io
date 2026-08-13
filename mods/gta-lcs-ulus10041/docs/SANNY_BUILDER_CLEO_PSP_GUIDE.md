# Sanny Builder and CLEO PSP Compilation Guide

## Scope

This guide explains how to compile the original CLEO PSP source files in this project for a legally owned USA copy of *Grand Theft Auto: Liberty City Stories* with serial **ULUS-10041**. The project contains no GTA LCS ISO, executable, textures, audio, or other game assets.

The two source files are:

| Source file | Purpose | Directive | Expected output |
|---|---|---|---|
| `cleo-src/gtalcs.zombie_attack.txt` | Activates the game’s existing hostile-pedestrian behavior | `{$CLEO .csi}` | `gtalcs.zombie_attack.csi` |
| `cleo-src/gtalcs.roaming_horde.txt` | Spawns a capped group of six nearby pedestrians and assigns a kill-player objective | `{$CLEO .csa}` | `gtalcs.roaming_horde.csa` |

These are prototypes. They must be compiled and tested on the user’s exact ULUS-10041 build before increasing the horde cap or adding more complex mechanics.

## Requirements

Use **Sanny Builder 4.2.0 or a later compatible release** from the official [Sanny Builder website][1]. The project’s public CLEO PSP documentation states that PSP game support requires **PPSSPP 1.11.2 or higher**, supports LCS `ULUS10041`, and loads scripts from `PSP/PLUGINS/cleo/lcs/`.[2]

You also need the CLEO PSP/CLEO Android package installed in the PPSSPP memory-stick directory. Do not download DLLs, PRX files, game images, or scripts from untrusted mod mirrors. Use the project’s official CLEO PSP repository and inspect releases before installing.[2]

## Step 1: Install Sanny Builder

Download Sanny Builder from the official site and extract it on a Windows computer. Sanny Builder is a Windows desktop tool; the Android PPSSPP device is used for runtime testing, not for the primary compilation step. Launch Sanny Builder once so it creates its normal configuration directories.

The official Sanny Builder tutorial describes compilation as converting source text into the binary format read by the game. In the editor, compile with **Tools → Compile** or press **F6**. The resulting binary is written beside the source file unless the configuration specifies another output location.[3]

## Step 2: Prepare the LCS edit mode

Open the project’s `.txt` file in Sanny Builder and select the LCS edit mode in the selector at the bottom-right of the editor. The mode must be for **Liberty City Stories**, not GTA III, Vice City, San Andreas, VCS, or a generic CLEO mode.

CLEO PSP’s documentation explains that its custom opcodes must be added to the relevant Sanny Builder `SCM.INI`, and that LCS/VCS compilation may require basic keyword definitions. The required LCS keyword definitions are:

```ini
; keywords.txt for lcs
0001=wait
0002=goto
0002=jump
004d=jf
004e=end_thread
0050=gosub
0051=return
00db=if
03a9=thread
```

If the current Sanny Builder LCS mode already contains these definitions, do not duplicate them. If it does not, create or update the LCS mode’s keyword file according to the CLEO PSP documentation.[2]

## Step 3: Add the CLEO PSP custom opcodes

If Sanny Builder reports unknown opcodes such as `0DD5`, `0DD8`, `0DDA`, `0DDE`, `0DF2`, or `0DF4`, add the CLEO PSP declarations to the LCS mode’s `SCM.INI`. The declarations below are the ones used by the current prototypes and are documented by CLEO PSP.[2]

```ini
; CLEO PSP / CLEO ANDROID extensions for LCS
0DD0=2,%1d% = get_label_addr %2p%
0DD1=2,%1d% = get_func_addr_by_cstr_name %2d%
0DD5=1,%1d% = get_platform
0DD8=4,%1d% = read_mem_addr %2d% size %3d% add_ib %4d%
0DDA=3,%1d% = get_pattern_addr_cstr %2d% index %3d%
0DDB=3,get_game_ver_ex name_hash %1d% ver_hash %2d% ver_code %3d%
0DDC=2,set_mutex_var %1d% to %2d%
0DDD=2,%1d% = get_mutex_var %2d%
0DDE=-1,call_func %1d% add_ib %2d%
0DF2=2,create_menu %1d% items %2d%
0DF3=0,delete_menu
0DF4=2,%1d% = get_menu_touched_item_index maxtime %2d%
0DF5=1,set_menu_active_item_index %1d%
0DF6=1,%1d% = get_menu_active_item_index
1000=-1,opcode_func
```

Do not add declarations to a different game mode. Keep a backup of the original Sanny Builder configuration so it can be restored if another GTA project stops compiling.

## Step 4: Compile the prototypes

Open `gtalcs.zombie_attack.txt` first. Confirm the first line is `{$CLEO .csi}` and that the LCS edit mode is selected. Run **Tools → Compile** or press **F6**. A successful compile should create a `.csi` file beside the source. Treat any compiler warning as a test blocker until it is understood.

Next, open `gtalcs.roaming_horde.txt`. Confirm the first line is `{$CLEO .csa}` and compile it with the same LCS mode. A successful compile should create a `.csa` file. The `.csa` source is intended to start after the game loads, while `.csi` scripts are normally invoked from the CLEO menu.[2]

The expected files are:

```text
gtalcs.zombie_attack.csi
gtalcs.roaming_horde.csa
```

Do not rename the files to `.prx`. A `.prx` is a native plugin and requires a separate PSP/Android plugin build; these prototypes are CLEO scripts.

## Step 5: Install CLEO PSP and the compiled scripts on Android

On the Android PPSSPP memory stick, create or use:

```text
PSP/PLUGINS/cleo/lcs/
```

Install the CLEO PSP runtime according to its official release instructions. Then copy the compiled files into the `lcs` directory:

```text
PSP/PLUGINS/cleo/lcs/gtalcs.zombie_attack.csi
PSP/PLUGINS/cleo/lcs/gtalcs.roaming_horde.csa
```

Keep the source `.txt` files on the development computer. PPSSPP does not execute the source text directly.

Start PPSSPP, load the legally owned ULUS-10041 game, and confirm that the CLEO runtime is active. CLEO PSP documentation states that `.csa` scripts start after the game loads and `.csi` scripts can be invoked from the in-game CLEO menu. On PSP-style controls, press **Start**, then **Start** again to open the CLEO menu; on Android, use the CLEO menu gesture/interface provided by the installed runtime.[2]

## Step 6: Safe first test

Begin with a fresh save or a backed-up save state. Test only `gtalcs.zombie_attack.csi` first. Confirm that ordinary pedestrians become hostile and that the game remains responsive after 60 seconds. Stop the test if the game freezes, crashes, corrupts a save, or produces uncontrolled spawning.

Only after the first script is stable should you test `gtalcs.roaming_horde.csa`. The current source intentionally caps the group at six pedestrians, refreshes pursuit objectives once per second, and removes the tracked pedestrians when the player handle becomes invalid. Do not increase the cap until the six-pedestrian build has been tested for at least ten minutes across multiple locations.

## Troubleshooting

| Symptom | Likely cause | Corrective action |
|---|---|---|
| Unknown opcode during compilation | CLEO PSP declarations are missing from the LCS mode | Add the documented declarations to the LCS mode and restart Sanny Builder |
| Unknown `thread`, `if`, or `wait` syntax | LCS keyword definitions are missing or the wrong edit mode is selected | Select LCS mode and add only the documented LCS keyword definitions |
| `.csi` or `.csa` is ignored | CLEO PSP is not installed, the file is in the wrong folder, or the game ID is not LCS | Verify `PSP/PLUGINS/cleo/lcs/`, CLEO runtime installation, and ULUS-10041 targeting |
| Game starts but no pedestrians attack | The source was compiled with the wrong mode, or the build’s cheat/objective behavior differs | Recompile in LCS mode and test the smaller `zombie_attack.csi` script first |
| Freeze or crash after horde starts | Six pedestrians may still exceed the tested build’s safe budget, or a game-state edge case was hit | Remove the `.csa`, reload a clean save state, and test with one or two pedestrians before changing code |
| Android cannot access the folder | Scoped storage or PPSSPP storage mode is restricting access | Use PPSSPP’s selected memory-stick directory and grant Android file access; do not place files in an unrelated download folder |

## What this guide does not provide

This guide does not provide a precompiled binary because the runtime cannot be verified here against the user’s exact Android PPSSPP version, CLEO PSP build, ULUS-10041 revision, and save state. A binary that compiles but has not been tested can cause freezes or unintended game behavior. The source and checklist are therefore the safer test-build deliverables.

The prototypes also do not copy NZ:P or Days Gone assets. They use the GTA LCS engine’s own pedestrians and documented script operations to create an original hostile-horde experiment.

## References

[1]: https://sannybuilder.com/ "Sanny Builder official website and download"
[2]: https://github.com/cleolibrary/GTALCS.GTAVCS.PSP.CLEO "CLEO PSP / CLEO Android official source and documentation"
[3]: https://tutorial.sannybuilder.com/sanny-builder/ "Sanny Builder official compilation tutorial"
[4]: https://docs.sannybuilder.com/scm-documentation/lcs "Sanny Builder Liberty City Stories documentation"
