# Permissions & Access Control

## 1. System Roles
- **Single Public Player**: All users access the software under equal privileges as an arcade player.
- **Administrator Role**: Not implemented / Not required.

## 2. Browser Permissions Matrix
| Permission / API | Status | Purpose | Justification |
|---|---|---|---|
| `AudioContext` | Required upon user gesture | Procedural engine sound and SFX | Real-time interactive arcade audio |
| `LocalStorage` | Optional / Best-effort | Stage best times and audio mute state | Client-side progress persistence |
| `Gamepad API` | Optional | Gamepad controller navigation | Hardware game controller support |
| `Geolocation` | Not used | None | Unnecessary for racing game |
| `Camera / Microphone` | Not used | None | Unnecessary for racing game |
| `Notifications` | Not used | None | Unnecessary for racing game |

## 3. Sandboxed Frame Permissions
- In `metadata.json`, `requestFramePermissions` is empty (`[]`), ensuring the application can safely execute within sandboxed iframes without requiring elevated browser entitlements.
