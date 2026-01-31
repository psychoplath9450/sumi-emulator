# SUMI Emulator

Browser-based Game Boy emulator that mirrors the SUMI firmware implementation.

## Purpose

Test and tweak the GB emulator experience without flashing your device.

- Same emulation core as the ESP32 firmware
- Apply ROM patches (skip intro, no battle flash, etc.)
- Simulates e-ink display behavior
- Test gameplay flow and optimizations

## Usage

1. Open `index.html` in a browser
2. Drop a `.gb` ROM file (Pokemon Red/Blue recommended)
3. Use keyboard or on-screen buttons to play

## Controls

| Keyboard | Game Boy |
|----------|----------|
| Arrow keys / WASD | D-pad |
| Z / Space | A |
| X | B |
| Enter | Start |
| Shift | Select |

## ROM Patches

- **Skip Intro**: Jump past the opening animation
- **No Battle Flash**: Remove screen flashing on encounters
- **Instant Text**: Text appears immediately
- **Skip Battle Animations**: Faster combat

## E-Ink Simulation

Toggle these to preview how it looks on real hardware:

- **E-Ink Mode**: Reduces 4 grays to 2 colors
- **Paper Texture**: Simulates e-ink paper surface
- **Show Frame Changes**: Highlights pixels that changed (debug)

## Files

- `index.html` - Complete emulator
- `README.md` - This file

## Deployment

Push to GitHub and enable Pages:
```
https://yourusername.github.io/sumi-emulator/
```
