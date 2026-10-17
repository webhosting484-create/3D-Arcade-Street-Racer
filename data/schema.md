# Data Schema

## 1. Overview
The project is a client-side retro 3D arcade street racing application ("Neon Rush"). All runtime gameplay state (player car velocity, position, stage geometry, rival vehicles, particle/skid marks, and timing) is managed in-memory within the client application loop. Persistent data is restricted to client-side Web Storage (`localStorage`) for saving player best stage lap times and high scores. No external SQL, NoSQL, or cloud database is connected in the current project implementation.

## 2. Data Storage Technology
- **Client In-Memory State**: JavaScript object structures holding road segment coordinates, camera coordinates, physics state, rival vehicles, audio state, and UI view status.
- **Browser Web Storage (`localStorage`)**: Used for persistent local storage of best lap times, completed stage records, and audio preference flags.
- **Backend / Database Engine**: None currently connected (Not verified from the current project).

## 3. Entities / Tables

### LocalStorage Record: `neon_rush_save_data`
**Purpose:**
Persists user progress, stage completion records, best lap times, and user preferences across browser sessions.

**Fields:**
| Field | Type | Required | Default | Description |
|---|---|---|---|---|
| `bestTimes` | Array<number> | Yes | `[0, 0, 0, 0, 0]` | Best stage completion times in seconds for each of the 5 stages (0 indicates uncompleted) |
| `highScore` | number | Yes | `0` | Highest arcade score achieved across runs |
| `soundMuted` | boolean | Yes | `false` | User audio mute preference flag |
| `version` | number | Yes | `1` | Save schema version identifier |

**Primary Key:**
- Key name in `localStorage`: `"neon_rush_save_data"`

**Foreign Keys:**
- None (isolated client-side key-value record).

**Unique Constraints:**
- Single browser-scoped key per origin.

**Indexes:**
- None (browser key-value store).

**Status / Enum Values:**
- None.

**Security / Access Notes:**
- Stored in browser `localStorage`. Accessible via client-side JavaScript on the same origin. No sensitive personal or authentication data is stored.

---

### In-Memory Entity: `CarState`
**Purpose:**
Represents the current real-time physical simulation state of the player's vehicle.

**Fields:**
| Field | Type | Required | Default | Description |
|---|---|---|---|---|
| `x` | number | Yes | `0` | Normalized lateral position on the road (-1.0 to 1.0 represents the road lanes; > 1.0 is off-road) |
| `y` | number | Yes | `0` | Elevation / world Y position calculated from road segment elevation |
| `z` | number | Yes | `0` | Longitudinal position along the stage track in world units |
| `speed` | number | Yes | `0` | Current forward velocity in world units per second |
| `maxSpeed` | number | Yes | `12000` | Top velocity achievable under standard engine acceleration |
| `accel` | number | Yes | `4200` | Engine acceleration force applied per second |
| `braking` | number | Yes | `9000` | Braking deceleration force applied per second |
| `offRoadDecel`| number | Yes | `8000` | Drag deceleration applied when off asphalt |
| `centrifugal` | number | Yes | `0.32` | Centrifugal lateral displacement multiplier during curves |
| `spin` | number | Yes | `0` | Spin duration timer following a collision |
| `drift` | number | Yes | `0` | Drift intensity factor for tyre smoke and audio squeal |

**Primary Key:**
- Singleton instance in `STATE.car`.

**Foreign Keys:**
- Refers to current `Segment` index via `Math.floor(car.z / segLength) % segments.length`.

**Unique Constraints:**
- Single player instance per game session.

**Indexes:**
- Direct array offset lookup into road segments.

**Status / Enum Values:**
- `spin > 0`: Vehicle in collision spin recovery mode.
- `Math.abs(x) > 1.0`: Vehicle is driving off-road on verge/shoulder.

**Security / Access Notes:**
- Transient in-memory state; lost upon page refresh.

---

### In-Memory Entity: `RivalCar`
**Purpose:**
Represents active AI-controlled rival traffic vehicles navigating the expressway.

**Fields:**
| Field | Type | Required | Default | Description |
|---|---|---|---|---|
| `id` | number | Yes | auto-assigned | Unique numerical identifier for the rival vehicle |
| `x` | number | Yes | `-0.8 to 0.8` | Normalized lateral road lane coordinate |
| `z` | number | Yes | `world units` | Longitudinal position along the track |
| `speed` | number | Yes | `6000 to 9500` | Forward cruising speed |
| `colorIndex` | number | Yes | `0 to 3` | Palette index for sprite rendering (hot magenta, cyber yellow, lime, synth violet) |
| `laneTarget` | number | Yes | `lane offset` | Target lateral coordinate during lane change behavior |
| `laneTimer` | number | Yes | `seconds` | Countdown to next lane change decision |

**Primary Key:**
- `id` (integer).

**Foreign Keys:**
- Assigned to road segment at `Math.floor(z / segLength)`.

**Unique Constraints:**
- `id` unique per active stage session.

**Indexes:**
- None.

**Status / Enum Values:**
- `colorIndex`: `0` (Hot Magenta), `1` (Cyber Yellow), `2` (Toxic Lime), `3` (Synth Violet).

**Security / Access Notes:**
- Ephemeral simulation state.

---

### In-Memory Entity: `RoadSegment`
**Purpose:**
Represents a discrete 200-unit slice of the pseudo-3D perspective road.

**Fields:**
| Field | Type | Required | Default | Description |
|---|---|---|---|---|
| `index` | number | Yes | `0..N` | Sequential index of the segment along the track |
| `p1` | Point3D | Yes | `{x,y,z}` | Near edge world and projected screen coordinates |
| `p2` | Point3D | Yes | `{x,y,z}` | Far edge world and projected screen coordinates |
| `curve` | number | Yes | `-3..3` | Curvature factor for horizontal track bending |
| `color` | ColorSet | Yes | object | RGBA palette for grass verge, rumble strips, road surface, and lane markings |
| `sprites` | Array<SpriteRef>| Yes| `[]` | Roadside objects attached to this segment |
| `hazard` | string / null | Yes | `null` | Hazard identifier: `'slick'`, `'cone'`, or `null` |

**Primary Key:**
- `index` (integer).

**Foreign Keys:**
- None.

**Unique Constraints:**
- `index` is strictly unique and monotonic per stage track.

**Indexes:**
- Contiguous array indexing `segments[index]`.

**Status / Enum Values:**
- `hazard`: `'slick'`, `'cone'`, or `null`.

**Security / Access Notes:**
- Procedurally generated per stage from deterministic stage configuration tables.

---

## 4. Authentication Data
- No user authentication system, identity provider, JWT token, or user account model is implemented in the current codebase.
- User presence is entirely anonymous and local to the web client.

## 5. Security
- Client-side storage boundaries: LocalStorage data is isolated to the origin domain by browser same-origin policy.
- All input validation on loaded best times uses `Number.isFinite()` and range clamps to prevent corrupted state.

## 6. Storage / Files
- No file upload, blob storage, cloud bucket, or media filesystem is configured.
- All visual assets and audio effects are generated algorithmically at runtime using HTML5 Canvas 2D and Web Audio API synthesizer nodes.

## 7. Database Functions / Triggers
- None (no server-side database in the current project).

## 8. Important Constraints
- `bestTimes` array length must strictly equal the stage count (5 stages).
- All times must be non-negative floating point numbers.
- Car velocity is capped at `CONFIG.topSpeed` under forward throttle, and clamped to `0` when stationary.

## 9. Unknown / Unverified Information
- Server-side database tables: Not verified from the current project (no relational or document database files found).
- Remote multiplayer persistence: Not verified from the current project.
