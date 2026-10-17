# Security Rules

## 1. Storage Boundaries
- **Rule SR-01: Sandboxed Local Storage Access** [Implemented - `index.html` UTIL.storage]
  - All access to browser `localStorage` must be wrapped in `try...catch` blocks to prevent exceptions in environments with restricted cookies/storage (e.g., incognito windows or sandboxed iframes).
  - Data loaded from `localStorage` must undergo strict parsing and numeric clamping before assignment to application state.

## 2. Input Validation Rules
- **Rule SR-02: Whitelisted Key Mapping** [Implemented - `index.html` INPUT]
  - Only specific whitelisted key codes (`ArrowUp`, `ArrowDown`, `ArrowLeft`, `ArrowRight`, `KeyW`, `KeyA`, `KeyS`, `KeyD`, `Space`, `Escape`, `KeyP`, `KeyM`) mutate input state flags.
  - Gamepad joystick axes must be clamped between `-1.0` and `1.0`.

## 3. DOM & Execution Safety
- **Rule SR-03: Safe Text Insertion** [Implemented - `index.html` SCREENS & HUD]
  - Dynamic user score, time values, and stage names must be rendered via `.textContent` or drawn directly onto the `<canvas>` buffer. No dynamic HTML evaluation via `.innerHTML` or `eval()`.

- **Rule SR-04: Audio Context Lifecycle** [Implemented - `index.html` AUDIO]
  - Web Audio `AudioContext` must remain uninitialized or suspended until an explicit user interaction event occurs to comply with modern browser autoplay policies.

- **Rule SR-05: Simulation Time Guard** [Implemented - `index.html` LOOP]
  - Maximum delta time per frame step is clamped to 100ms, and physics sub-steps are capped at 5 iterations per animation frame to prevent denial of service (infinite loop freeze) when returning from a backgrounded tab.
