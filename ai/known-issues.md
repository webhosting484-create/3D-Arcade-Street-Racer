# Known Issues & Risks

## Known Issues Log

### ISSUE-01: Browser Audio Autoplay Policy
- **Severity**: Low (Mitigated)
- **Evidence**: Modern browsers block `AudioContext` from playing sounds until a user gesture occurs.
- **Affected Area**: Web Audio graph in `AUDIO.init()`.
- **Current Behavior**: Audio starts automatically on first keypress, mouse click, or touch interaction.
- **Status**: Resolved / Handled by design.

### ISSUE-02: DevicePixelRatio on High-DPI Displays
- **Severity**: Low (Mitigated)
- **Evidence**: On Retina displays, a canvas without `devicePixelRatio` scaling appears blurry or misaligned.
- **Affected Area**: Canvas sizing and coordinate transform in `RENDER`.
- **Current Behavior**: Canvas dimensions are scaled by `window.devicePixelRatio` and CSS dimensions are matched to `100vw` / `100vh`.
- **Status**: Resolved / Handled by design.

### ISSUE-03: LocalStorage Restricted Environments
- **Severity**: Low (Mitigated)
- **Evidence**: Sandboxed iframes without `allow-same-origin` or private browsing modes may throw `SecurityError` upon accessing `window.localStorage`.
- **Affected Area**: Save and load routines.
- **Current Behavior**: Wrapped in guarded `try...catch`; in-memory fallbacks are used when storage throws an error.
- **Status**: Resolved / Handled by design.

## Technical Debt & Limitations
1. No online multiplayer or shared global leaderboard (operates purely locally).
2. Canvas 2D rasterization speed depends on resolution; heavy 4K full-screen scaling is handled with adaptive buffer sizing if needed.
