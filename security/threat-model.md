# Threat Model

## Assets
1. **Client Browser State & Local Storage**: Player progress, best lap times, user settings stored in `localStorage`.
2. **Audio & Graphics Processing Resources**: Client CPU/GPU cycles running canvas rasterization and Web Audio synthesis.
3. **Application Integrity & Availability**: Ensuring client runs without crashes, infinite loops, memory leaks, or UI freezing.

## Trust Boundaries
1. **Host Environment vs. Client Application**: Browser window sandbox, DOM events, and Web APIs.
2. **Untrusted User Inputs vs. Game Logic**: Keyboard, Gamepad, Touch, and Window Resize events.
3. **External Origin Storage vs. App Storage**: Browser same-origin policy boundary protecting `localStorage`.

## Actors
1. **End User / Player**: Normal interactive user playing the street racer game in a desktop or mobile browser.
2. **Malicious Extension / Script on Same Origin**: Potential XSS or malicious third-party script running in the same browser context.
3. **Local Attacker with Console Access**: User modifying DOM elements or executing arbitrary JavaScript in Developer Tools.

## Entry Points
1. **Keyboard Event Listeners** (`keydown`, `keyup`)
2. **Touch Event Listeners** (`touchstart`, `touchend`, `touchcancel`)
3. **Gamepad API Polling** (`navigator.getGamepads()`)
4. **Window Lifecycle Events** (`resize`, `visibilitychange`)
5. **Storage Deserialization** (`localStorage.getItem('neon_rush_save_data')`)

## Threats
1. **T1: Storage Tampering / Deserialization Injection**: Corrupted or maliciously crafted JSON in `localStorage` leading to runtime exceptions.
2. **T2: Denial of Service via Tab Backgrounding / Delta Time Exploitation**: Extreme `requestAnimationFrame` delta times causing frame-skip physics lockups.
3. **T3: Audio Autoplay Violations & Resource Consumption**: AudioContext running unprompted and triggering browser policy blocks or audio distortion.
4. **T4: DOM-based Cross-Site Scripting (XSS)**: Injecting unescaped text into the DOM HUD or screens.

## Attack Scenarios
- **Scenario A (Data Corruption)**: Attacker or script puts `{ "bestTimes": "malicious_string" }` into `localStorage`. Upon page load, math operations (`Math.min`, `toFixed`) throw TypeError.
- **Scenario B (Frame Rate Spikes / Tab Switch)**: User leaves tab backgrounded for 30 minutes. Upon returning, `deltaTime` is huge. Without accumulator capping, physics engine runs 100,000 loop cycles, freezing the browser tab.

## Impact & Likelihood
| Threat | Likelihood | Impact | Risk Severity |
|---|---|---|---|
| T1: Storage Tampering | Low | Low (Client-side crash) | Low |
| T2: Delta Time Accumulator Freeze | Medium | High (Tab hang / Unresponsive) | Medium |
| T3: Audio Autoplay Policy Block | Medium | Low (Silent audio) | Low |
| T4: DOM-based XSS | Very Low | High (Code execution) | Low |

## Existing Mitigations
- **Storage Guard**: Strict `try...catch` wrapper around `localStorage.getItem` and `setItem`, with type validation and numeric fallback defaults.
- **Timestep Clamping**: Fixed-step 60Hz physics accumulator with a hard cap of maximum 5 catch-up steps per frame (`dt = Math.min(rawDelta, 0.1)`).
- **Audio Gesture Gate**: `AudioContext` is instantiated/resumed only after an explicit user interaction (click, keypress, touch).
- **DOM Sanitization**: Real DOM HUD elements use `.textContent` and `.innerText` rather than `.innerHTML` for all dynamic numerical readouts.

## Missing Mitigations
- None required for standalone client application; all current client risks are mitigated.
