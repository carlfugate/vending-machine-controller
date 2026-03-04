# AGENT.md

## Project Overview

Custom ESP32-based controller for a Naturals2Go N2G4000 vending machine entree expansion unit. The unit has 10 DC spiral motors with home switches but no controller — we're building one from scratch.

## Key Context

- **Hardware**: 10 DC gear motors, 3-wire each (Red=power, Black=ground, White=home switch signal), 2 main harness connectors, 1 door safety switch
- **Controller**: ESP32 DevKit + 16-channel relay module (Phase 1), custom MOSFET board later
- **Firmware**: C++ via PlatformIO, targeting ESP32
- **Use cases**: CTF events at security conferences, conference activations, normal vending
- **Safety-first**: No power has been applied yet. Multimeter verification of all voltages/pinouts is required before any hardware testing.

## Current Status (as of 2026-03-04)

- ✅ Hardware photographed and documented (23 photos)
- ✅ Motors, wiring, and connectors identified from photos
- ✅ Deep research on vending motor operation complete
- ✅ ESP32 controller designed (pin assignments, circuits, firmware sketched)
- ⏳ **BLOCKED**: Multimeter verification of motor voltage, switch type, and connector pinout
- ⏳ No parts purchased, no circuits built, no firmware flashed

**Read [PICKUP.md](PICKUP.md) for full current state, what's verified vs assumed, and exact next steps.**

## Project Structure

```
PICKUP.md                          # Current state and next steps — READ FIRST
README.md                          # Project overview and status
docs/
  PROJECT_PLAN.md                  # Full 14-task implementation plan
  phase1-hardware-analysis/
    03-motor-and-control-system.md # How vending motors work (deep research)
  phase3-motor-control/
    esp32-controller-design.md     # ESP32 design, circuits, firmware code
hardware/
  COMPONENT_INVENTORY.md           # Parts list from photo analysis
  WIRING_DIAGRAM.md                # System wiring diagrams (estimated)
  photos/                          # 23 JPGs of internals
firmware/
  esp32/                           # (empty — code is in design doc for now)
```

## Coding Conventions

- **Language**: C++ (Arduino framework on ESP32)
- **Build system**: PlatformIO
- **Style**: Minimal, safety-focused — every motor operation must have a timeout and door switch check
- **Pin definitions**: Centralized in `config.h`
- **Motor control**: Encapsulated in `motor.h`/`motor.cpp`
- **No blocking delays** in production code (use millis()-based timing)
- **Serial debug output** on all operations for testing phase

## Important Constraints

- **Never assume voltages** — all values in docs are estimates until multimeter-verified
- **One motor at a time** — no simultaneous motor operation (power budget, safety)
- **Timeout on every motor run** — 8 second max, then force stop
- **Door switch is a hard interlock** — if door opens mid-vend, stop immediately
- **Flyback diodes required** on every motor — back-EMF will destroy MOSFETs/relays without them
- **Logic-level MOSFETs only** — ESP32 is 3.3V, standard MOSFETs won't fully turn on

## Key Technical Decisions

| Decision | Choice | Why |
|----------|--------|-----|
| MCU | ESP32 | WiFi/BLE for CTF mode, enough GPIO for direct drive |
| Motor driver (Phase 1) | 16-ch relay module | Solves braking problem, simple, debuggable |
| Motor driver (Phase 2) | IRLZ44N MOSFETs | Logic-level, low Rds(on), cheap |
| Switch input | Direct GPIO with pull-up (or optocoupler if 12V signal) | Depends on verification |
| Framework | Arduino on PlatformIO | Fast iteration, good ESP32 support |

## What NOT to Do

- Don't write firmware that applies power to motors without timeout protection
- Don't assume the wiring diagram pinout is correct — it's estimated from photos
- Don't add payment/MDB integration yet — get basic motor control working first
- Don't use GPIO 0, 1, 3, 6-11 on ESP32 (boot/flash/serial conflicts)
