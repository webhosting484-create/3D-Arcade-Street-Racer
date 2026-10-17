# Architecture & Technical Decisions

## Decision 1: Single-File Standalone Architecture with Vite Compatibility
- **Decision**: Package the entire arcade street racer within `index.html` with inline CSS, HTML, and vanilla JS, while maintaining a clean React/Vite mount bridge in `src/App.tsx`.
- **Why Made**: Fulfills the explicit deliverable requirement ("A single index.html, all HTML, CSS and JS inline. Opens by double-clicking and plays offline") while simultaneously integrating with AI Studio's Vite dev server on port 3000.
- **Current Implementation**: `index.html` holds the standalone arcade engine; `src/App.tsx` and `src/main.tsx` provide clean Vite integration and zero compilation warnings.
- **Files Affected**: `/index.html`, `/src/App.tsx`, `/package.json`.
- **Evidence**: User prompt requirement and project structure.
- **Trade-offs**: Inline code structure requires strict discipline and banner-section modularity within a single file.

## Decision 2: 2D Canvas Perspective Road Projection over 3D WebGL
- **Decision**: Use classical pseudo-3D road projection (Lou's OutRun algorithm) on a standard HTML5 2D canvas rather than importing Three.js or writing WebGL shaders.
- **Why Made**: Guarantees zero external library dependencies, instantaneous startup with zero network lag, genuine retro 1980s/90s arcade aesthetic, and 60+ FPS on any hardware or mobile device.
- **Files Affected**: `/index.html`.
- **Trade-offs**: Geometry is constrained to segmented road projections rather than arbitrary 3D polygonal meshes.

## Decision 3: Procedural Audio Synthesis via Web Audio API
- **Decision**: Synthesize car engine RPM, screeching drift tyres, collision thuds, and checkpoint chimes using native Web Audio API oscillators, noise generators, and biquad filters.
- **Why Made**: Eliminates need for external MP3/WAV assets, enables dynamic pitch-tracking tied continuously to vehicle speed and load, and operates completely offline.
- **Files Affected**: `/index.html` (AUDIO block).
- **Trade-offs**: Requires custom audio node graph construction and careful gain scheduling.

## Decision 4: Decoupled Physics Accumulator
- **Decision**: Fixed 60Hz physics timestep with an accumulator loop decoupled from variable display refresh rates (`requestAnimationFrame`).
- **Why Made**: Prevents high-refresh-rate displays (120Hz/144Hz) from running physics too fast and low-frame-rate devices from skipping collisions.
- **Files Affected**: `/index.html` (LOOP and INIT block).
- **Trade-offs**: Slight interpolation variance if display rate fluctuates wildly, mitigated by catchup clamping.
