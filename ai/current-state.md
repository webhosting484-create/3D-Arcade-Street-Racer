# Current State Snapshot

## Project Purpose
"Neon Rush - 3D Arcade Street Racer" is a high-octane retro 3D arcade street racing game with pseudo-3D perspective road projection, procedural synth audio, multi-stage checkpoint progression, rival traffic AI, and rain-slick neon night visuals.

## Technology Stack
- **Languages**: TypeScript, JavaScript (ES2022), HTML5, CSS3.
- **Framework & Bundler**: Vite (Frontend build & dev server), React 19 (for modular component shell), Express (server runtime if deployed).
- **Styling**: Tailwind CSS & custom responsive CSS.
- **Rendering**: HTML5 Canvas 2D Perspective Road Engine (zero external WebGL/canvas library dependencies).
- **Audio Engine**: Web Audio API (procedural multi-oscillator engine synthesis, white noise tyre screech, synthesized FM collision and chime tones).
- **Persistence**: Browser `localStorage`.

## Directory Structure
```
/
├── .env.example
├── .gitignore
├── ai/
│   ├── current-state.md
│   ├── decisions.md
│   ├── instructions.md
│   └── known-issues.md
├── data/
│   ├── data-flow.md
│   ├── relationships.md
│   └── schema.md
├── index.html
├── metadata.json
├── package.json
├── rules/
│   ├── business.md
│   ├── permissions.md
│   └── security.md
├── security/
│   ├── attack-surface.md
│   ├── secrets.md
│   ├── security-checklist.md
│   └── threat-model.md
├── src/
│   ├── App.tsx
│   ├── index.css
│   └── main.tsx
├── tsconfig.json
└── vite.config.ts
```

## Major Components
1. **Perspective Road Projection Engine**: Calculates trapezoidal road segments, curvature offsets, vertical hills, and distance fog.
2. **Dynamic Canvas Sprite Pipeline**: Renders player car (wedge coupe with racing stripes and glowing taillights), rival cars, neon skyscrapers, streetlights, overhead gantries, oil slicks, and traffic cones with depth scaling.
3. **Web Audio Synthesizer**: Procedural engine drone with dynamic pitch/filter tracking, tire screech, collision thump, and chime.
4. **Real DOM HUD & Screen Overlays**: Title screen, in-game HUD (speedometer, checkpoint clock, minimap, lap time), stage result card, pause menu, and game over / victory screens.
5. **Gamepad & Touch Input Subsystem**: Zero-latency input processing for keyboard, mobile touch controls, and standard gamepads.

## Current Feature Status
- 5 Complete Stages with progressive curve and hill variations: Verified.
- Standalone offline execution via single `index.html`: Verified.
- Fixed 60Hz physics step: Verified.
- Synthesized Web Audio without external audio files: Verified.
- Local best-time persistence: Verified.
