# NZ:P Clone Remaster Plan

## Goal

Modernize the player-facing GitHub Pages shell for the existing NZ:P WebGL build while keeping the original engine, package files, drag-and-drop support, and direct-play behavior intact.

## Risk slices

| Slice | Risk | Verification |
|---|---|---|
| WebGL boot compatibility | The legacy engine expects a global `Module`, an existing canvas, and exact package paths. | Confirm `ftewebgl.js` still loads, the progress state advances, and the canvas remains the engine target. |
| Responsive shell | A branded overlay must not interfere with pointer input or resize behavior. | Check desktop and narrow viewport layouts; confirm the canvas fills the viewport after boot. |
| File drop support | Custom UI can accidentally hide or replace the old file-drop workflow. | Drop-zone copy remains available before boot and updates the active file list. |
| Failure recovery | WebGL/package failures need readable feedback instead of a frozen black screen. | Force a script error or inspect the visible error state and reload action. |

## Build scope

The implementation is intentionally a focused remaster of the static shell rather than a replacement of the compiled engine. It adds a coherent survival-horror visual system, a branded boot panel, accessible status messaging, an animated progress treatment, a launch/retry affordance, keyboard hints, responsive styling, and a fullscreen toggle. It does not alter the proprietary game packages or the WebGL runtime.

## Verification criteria

The page must load without console-breaking syntax errors, preserve the existing `Module.files` paths, dynamically load `ftewebgl.js` exactly once, retain drag-and-drop package support, expose the canvas at full viewport size after boot, and present useful status text during both loading and failure states. The UI should remain legible at desktop and mobile widths and respect reduced-motion preferences.
