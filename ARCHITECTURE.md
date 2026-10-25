# PROJECT AFTERLIGHT — Architecture and Production Plan

## Engine and dependencies

- Unity Editor: Unity 6 (project template pinned to `6000.0.64f1`; open using a compatible Unity 6 installation).
- Render pipeline: URP package `17.0.4`.
- Runtime packages: Unity built-in modules; no paid or external asset dependency.
- Input: uses Unity Input System when enabled, with a legacy Input Manager fallback. The package manifest includes Input System 1.17.0; one input backend must be enabled under Player Settings > Active Input Handling.
- Build target: Android ARM64 first; configure build profiles and keystore on the developer machine.

## Runtime composition

`GameBootstrap` owns the vertical-slice lifecycle and wires the systems together. `DistrictGenerator` creates an original urban block from primitive geometry and shared materials. `PlayerController` + `OrbitCamera` provide third-person locomotion. `VehicleController` handles a drivable rigidbody and driver handoff. `TrafficMover` provides distance-unaware prototype traffic. `WantedSystem` triggers a single pursuing patrol car, while `PoliceChaseAI` moves toward the active player/vehicle target. `MissionSystem` stores completion state; `SaveSystem` serializes money and completion to versioned JSON. `MobileInput` maps keyboard/mouse and touch gestures to gameplay actions.

## Ownership rules

- `GameBootstrap`: composition root, startup values, top-level interactions and autosave cadence.
- `DistrictGenerator`: temporary environment construction only; no quest progression logic.
- `PlayerController` and `VehicleController`: their own locomotion state only.
- `MissionSystem`, `WantedSystem`, and `SaveSystem`: independent state services.
- Visuals are deliberately placeholders. Replace generated geometry with prefabs without changing mission/save interfaces.

## Current milestone status

### Milestone 1 — Foundation (implemented in source; engine verification pending)

- [x] Project folders, package manifest, project version marker.
- [x] Editor setup menu/first-open bootstrap for a demo scene and URP pipeline asset.
- [x] Procedural test district, directional light and basic day/night rotation.
- [x] Player CharacterController, orbit camera, keyboard/touch input layer.
- [ ] Verify C# compilation and Play Mode inside Unity (Unity not installed in the authoring environment).
- [ ] Verify touch controls across aspect ratios and Android safe areas.

### Milestone 2 — Small vertical slice (prototype systems added, validation pending)

- [x] One drivable car and enter/exit handoff.
- [x] Three looping traffic placeholder cars.
- [x] Initial wanted level and one chasing patrol car.
- [x] One complete interaction-based mission and $500 reward.
- [x] Versioned JSON persistence.
- [ ] Add proper police line-of-sight/search/arrest states.
- [ ] Add PlayMode tests and build/install APK on physical Android hardware.
- [ ] Profile FPS, CPU/GPU, memory and thermals on a defined mid-range device.

## Recommended next milestones

1. Fix any editor compilation or URP setup issues reported by the first Unity open; save a clean baseline commit.
2. Replace the prototype capsule and primitives with original licensed character, car, building and animation assets.
3. Upgrade driving to wheel/suspension-based handling (WheelColliders or a custom suspension solution), add collisions/damage and traffic avoidance.
4. Add NavMesh pedestrian AI and deterministic police perception/pursuit states with automated state-transition tests.
5. Expand mission definition to ScriptableObjects with objective chains, checkpoints, failure/restart and unlock rewards.
6. Add a streaming district grid, pooled props, occlusion/LOD, addressable content only when measured useful, and mobile graphics presets.
7. Add combat/inventory/shops only after movement, driving and saving have regression coverage.

## Main technical risks

- **URP asset initialization:** Unity versions can differ in renderer initialization behavior. The Editor setup uses URP's built-in renderer initialization API, but must be confirmed in the target Editor.
- **Input configuration:** `MobileInput` compiles the Unity Input System branch when `ENABLE_INPUT_SYSTEM` is enabled, and the legacy branch when `ENABLE_LEGACY_INPUT_MANAGER` is enabled. Confirm Active Input Handling in Player Settings after package resolution; this has not been checked in the Editor here.
- **Mobile physics/performance:** many primitive building windows create draw calls; merge/bake or replace with atlased models before scaling the map.
- **Vehicle feel:** the current one-collider car is an arcade placeholder, not a production vehicle simulation.
- **Police/traffic behavior:** these are intentionally simplistic, have no pedestrian navmesh, traffic rules, arrest system or line-of-sight logic.
- **Persistence:** save data is local and unencrypted; this is acceptable for an offline prototype but not anti-cheat resistant.
- **Build:** no APK can be truthfully delivered until Unity and Android build tools compile, package and launch the project.
