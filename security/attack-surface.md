# Attack Surface Analysis

## Public Endpoints
- `/index.html` (or root `/`): Static HTML5 web page containing the client game interface and scripts.
- Dev Server Port: Runs locally on port 3000 during development.
- In production: Served as static HTML/CSS/JS assets without exposed dynamic backend RPC routes.

## Internal Endpoints
- None in the standalone game deliverable. No private admin endpoints or internal debug routes exist.

## Authentication / Authorization Surfaces
- None. The game is open access and runs without user registration or role-based access control.

## User Inputs
1. **Keyboard Events**: Handled through `window.addEventListener('keydown')` and `'keyup'`. Keys are mapped strictly to whitelisted control codes (`ArrowUp`, `ArrowDown`, `ArrowLeft`, `ArrowRight`, `KeyW`, `KeyA`, `KeyS`, `KeyD`, `Space`, `Escape`, `KeyP`, `KeyM`). Unrecognized keys are ignored.
2. **Touch Events**: On-screen buttons with dedicated event listeners. Coordinate offsets are bounded to button geometries.
3. **Gamepad Polling**: Standard numeric values from `axes[0]` and `buttons[n]` are clamped to `[-1.0, 1.0]` and `[0.0, 1.0]`.

## File Uploads
- Zero file upload mechanisms exist.

## Database Access
- Zero direct database connections. No SQL queries, NoSQL queries, or ORM interfaces are exposed to the client.

## Storage
- `localStorage` item `"neon_rush_save_data"` is read and written locally. It is sanitized with strict numeric type-checking.

## Webhooks
- None.

## Third-Party Integrations
- Zero external CDNs, third-party trackers, external fonts, or analytics libraries. The app runs completely offline.

## Admin Surfaces
- None.

## Network / Deployment Exposure
- Standard HTTPS web delivery via static file host or Vite dev server.

## Dependencies and Externally Reachable Surfaces
- Minimal runtime dependencies. Game rendering and audio processing are implemented with native browser APIs (HTML5 Canvas 2D, Web Audio API).
