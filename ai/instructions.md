# Instructions for Future AI Agents

## Project Overview
"Neon Rush" is an arcade 3D street racer designed to execute either as a standalone single-file HTML5 application (`index.html`) or within the Vite development server runtime.

## Core Rules for Modification
1. **Zero Allocations in Render Loop**:
   - Never instantiate objects, arrays, or closures inside the 60Hz/120Hz render loop (`draw()`, projection routines, sprite drawing).
   - Use pre-allocated coordinate objects, reusable arrays, and static lookup tables.
2. **Separation of Physics and Rendering**:
   - `PHYSICS.step(dt)` must read `INPUT` and mutate `STATE`. It must never issue canvas drawing commands.
   - `RENDER.draw()` must read `STATE` and paint to the canvas. It must never mutate simulation state.
3. **Audio Autoplay Compliance**:
   - AudioContext must only be resumed or created on a direct user gesture (keydown, mousedown, touchstart).
4. **Resilience to Browser Environments**:
   - Always wrap `localStorage` in `try...catch`.
   - Always clamp `dt` from `requestAnimationFrame` (`Math.min(dt, 0.1)`) and limit physics accumulator loops to a maximum of 5 iterations.
5. **No External Assets**:
   - Do not add remote CDN image tags, audio file links, or external font stylesheets. All visuals must be drawn procedurally on Canvas 2D and all audio synthesised via Web Audio API oscillators.
