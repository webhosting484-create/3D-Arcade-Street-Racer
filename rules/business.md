# Business Rules

## 1. Game Progression & Win Condition
- **Rule BR-01: Five Stage Structure** [Implemented - `index.html` CONFIG.stages]
  - The game consists of exactly 5 sequential stages:
    1. Stage 1: "CYBER EXPRESSWAY"
    2. Stage 2: "NEON HARBOR BRIDGE"
    3. Stage 3: "DOWNTOWN TUNNELS & FLYOVERS"
    4. Stage 4: "MIDNIGHT HIGHWAY S-RUN"
    5. Stage 5: "METROPOLIS SUMMIT FINAL"
  - The player advances to the next stage upon reaching the finish line of the current stage before the checkpoint countdown timer reaches zero.

- **Rule BR-02: Checkpoint Countdown Timer** [Implemented - `index.html` CONFIG & PHYSICS]
  - Each stage begins with an allocated checkpoint time (e.g. 45s for Stage 1, 42s for Stage 2, 40s for Stage 3, 38s for Stage 4, 36s for Stage 5).
  - The timer counts down continuously during active gameplay.
  - If the timer reaches 0 before the player crosses the finish line, the game transitions immediately to the `GAME OVER` state.

- **Rule BR-03: Victory Condition** [Implemented - `index.html` SCREENS]
  - Crossing the finish line of Stage 5 before the timer expires triggers the Victory ("STREET LEGEND") screen, displaying overall completion time.

## 2. Vehicle Physics & Driving Mechanics
- **Rule BR-04: Acceleration and Top Speed** [Implemented - `index.html` CONFIG.player]
  - Forward throttle accelerates the vehicle up to `CONFIG.topSpeed` (12,000 world units/sec).
  - Releasing throttle applies coasting drag deceleration.
  - Applying brake key applies rapid braking deceleration (`CONFIG.braking`).

- **Rule BR-05: Off-Road Drag** [Implemented - `index.html` PHYSICS]
  - When the player vehicle's lateral position $|x| > 1.0$ (off the paved asphalt onto the verge), the car experiences severe off-road deceleration (`CONFIG.offRoadDecel`) capping speed to a maximum of 30% of top speed, and induces camera rumble.

- **Rule BR-06: Centrifugal Cornering Force** [Implemented - `index.html` PHYSICS]
  - When entering a curved segment, the car is laterally pushed outward proportional to `(speed / topSpeed) * segment.curve * centrifugal`. The player must steer into the curve to maintain lane position.

- **Rule BR-07: Hazard & Traffic Collisions** [Implemented - `index.html` PHYSICS]
  - **Rival Traffic Collision**: Colliding with a rival vehicle reduces player speed by 50%, triggers a crash impact sound, and initiates a temporary spin recovery state (0.5s) where steering is softened.
  - **Oil Slick Hazard**: Driving over an oil slick triggers an immediate loss of traction, a lateral skid, and a speed penalty.
  - **Road Cone Hazard**: Striking a road cone scatters the cone sprite and incurs a minor speed penalty.

## 3. High Score & Record Keeping
- **Rule BR-08: Local Record Persistence** [Implemented - `index.html` STATE.save]
  - Best completion times for all 5 stages are saved locally in the browser.
  - When a stage is cleared faster than the recorded best time, the new time is stored.
