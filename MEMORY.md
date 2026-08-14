# Memory

## Existing project
- Repository: `chelsaewill87-sudo/nzp-team.github.io`
- Current HEAD: `ffdc998` on `main`.
- Root is a static GitHub Pages/WebGL shell around `ftewebgl.js` + `ftewebgl.wasm`, with `default.fmf`, `nzp/game.pk3`, and `nzp/progs.pk3`.
- The repository also contains PSP/GTA-LCS prototype notes under `mods/gta-lcs-ulus10041/`.
- The repository is very large because Git history contains multi-gigabyte binary objects; avoid unnecessary binary rewrites.

## Current live experience
- GitHub Pages redirects to `https://nzp.gay/`.
- The current player-facing page is essentially a black canvas with a small loading/progress status and minimal legacy controls.
- The current HTML has old metadata, a plain black body, a hidden canvas, a basic download-progress bar, drag-and-drop file support, and a dynamically loaded `ftewebgl.js` engine.

## Remaster direction
- Preserve the existing WebGL engine and file-loading behavior.
- Remaster the surrounding shell into a polished dark industrial survival interface: charcoal/steel palette, crimson emergency accents, warm amber action state, readable HUD-style loading screen, responsive layout, accessible controls, and clear status/error messaging.
- Generated visual reference: `reference.png` in the repository root. It depicts a first-person industrial corridor, shotgun foreground, three zombies, concrete cover, ammo pickup, and a compact survival HUD.

## Local visual verification
- The local shell renders successfully at `http://localhost:4173/` with the dark panel, red accent rule, amber eyebrow, large NZ:P wordmark, progress bar, fullscreen control, and active loadout list.
- The existing WebGL canvas is visible behind the shell and appears to boot, while the shell remains readable during initialization.
- `git diff --check` and `node --check` pass for the rewritten inline script.

## Boot-transition verification
The corrected local preview now fades the branded shell away and leaves the running WebGL main menu visible. Browser inspection shows the canvas as the active primary element and the fullscreen utility remains available. The engine reaches its main menu with Solo, Cooperative, Configuration, Character Bios, and Credits entries.
