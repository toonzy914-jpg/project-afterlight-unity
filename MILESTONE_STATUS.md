# Verification Record

Date authored: 2026-10-26

## Environment inspection

- Checked command availability for `Unity`, `unity-editor`, `dotnet`, `mono`, `java`, `adb`, `sdkmanager` and `gradle`.
- Available: Java.
- Not found: Unity Editor, C# compiler/runtime, Android SDK manager, ADB and Gradle.

## Claims made

- Project source files and project structure were written into this archive.
- Static checks can validate expected files, brace counts and archive contents.
- Unity compilation, scene load, Play Mode, Android build/install, and device performance are **not verified** in this environment.

## Acceptance tests to run locally

1. Open the project with Unity 6; wait for package resolution and scripts to compile.
2. Confirm `Afterlight_Demo.unity` is created and `Afterlight_URP.asset` is assigned under Project Settings > Graphics and Quality.
3. Open Console; resolve all errors before gameplay testing.
4. Press Play: confirm the city renders, player walks and turns, camera orbits, jump works, and movement collides with buildings.
5. Walk to the teal ECHO GARAGE marker and press E; verify mission complete and money increases by $500.
6. Walk near the teal coupe and press E; drive with WASD, then press E to exit. Verify wanted level produces a pursuing patrol car.
7. Stop Play, restart, and verify money/mission completion persist.
8. Build an Android APK through Unity Build Profiles; install it on a physical device and test control zones, safe areas, memory, frame rate, heat and battery drain.
