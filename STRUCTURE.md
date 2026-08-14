# Structure

## Runtime layers

| Layer | Responsibility |
|---|---|
| HTML document | Owns the branded shell, boot panel, status region, progress bar, drag-and-drop zone, and canvas container. |
| Legacy Emscripten `Module` | Keeps the original engine contract, package map, stdout/stderr forwarding, dependency progress, and post-run guard. |
| Boot controller | Loads `ftewebgl.js` once, switches the shell from boot mode to engine mode, and exposes retry/fullscreen actions. |
| Drop controller | Accepts directories and package files, maps them to the engine’s expected virtual paths, and lists active files before boot. |
| Visual system | Uses CSS gradients, scanlines, panels, borders, and responsive layout to create a dark industrial survival interface without adding runtime dependencies. |

## Compatibility rules

The canvas keeps the `id` and `class` expected by the WebGL build. The `Module.files` object keeps the original paths. The package assets remain untouched. The engine script continues to be appended dynamically so the original boot sequence and caching behavior are preserved.

## UI states

The shell uses four visible states: `standby`, where the user can review the active files and start the engine; `loading`, where progress and dependency status are shown; `running`, where the canvas owns the viewport; and `error`, where a recovery action and a concise diagnosis are provided.
