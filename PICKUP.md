# PICKUP.md - Start Here

**Last Updated**: 2026-03-04  
**Project**: Custom ESP32 controller for Naturals2Go N2G4000 entree vending unit

---

## TL;DR

We have a vending machine entree unit with 10 spiral motors and no controller. We've photographed the internals, identified all the motors and wiring, researched how vending motors work, and designed an ESP32-based controller. **No power has been applied yet.** The next step is multimeter testing to verify voltages and pinouts before buying parts.

---

## What We Know (Verified from Photos + Manual)

| Fact | Source | Confidence |
|------|--------|------------|
| 10 DC gear motors total (5 per tray × 2 trays) | Photos | High |
| 3-wire harness per motor (Red, Black, White/Yellow) | Photos | High |
| Each motor has an integrated microswitch (home switch) | Photos + Manual | High |
| 2 large white multi-pin connectors on right side | Photos | High |
| Door safety microswitch on door frame | Photos | High |
| No power supply inside the entree unit | Photos | High |
| Spirals rotate to push product forward, one rotation = one vend | Manual | High |
| Home switch uses cam mechanism on motor shaft | Research | High |
| Service manual exists: Seaga NTG4000 (44 pages) | ManualsLib | Verified |

## What We Assume (Needs Multimeter Verification)

| Assumption | Basis | Risk if Wrong |
|------------|-------|---------------|
| Motors are 12V DC | Standard for vending machines | Could be 24V → wrong power supply |
| Red wire = motor power (+) | Color convention | Could damage motor if reversed |
| Black wire = motor ground (-) | Color convention | Could damage motor if reversed |
| White wire = home switch signal | 3-wire convention | Could be wired differently |
| Home switch is NO at home position | Common in 3-wire assemblies | Software logic would be inverted |
| Motors draw ~500mA each | Typical for this size | Undersized driver if higher |
| Motor grounds are common (shared) | Typical design | Affects driver circuit design |
| Connector pinout follows tray grouping | Logical assumption | Must trace every wire |

**⚠️ DO NOT connect any controller until ALL assumptions above are verified with a multimeter.**

---

## What's Been Built / Written

### Documentation (all committed to repo)
- `hardware/photos/initial-inspection/vending-pics/` - 23 photos of internals (IMG_9496 through IMG_9533)
- `hardware/COMPONENT_INVENTORY.md` - Full parts inventory from photo analysis
- `hardware/WIRING_DIAGRAM.md` - System diagrams and estimated pinouts
- `docs/phase1-hardware-analysis/01-initial-inspection.md` - Inspection checklist with findings
- `docs/phase1-hardware-analysis/03-motor-and-control-system.md` - **Deep research document** covering:
  - How vending spiral motors work (dispense cycle)
  - Motor assembly anatomy (DC gear motor, gearbox, cam, home switch)
  - Home switch cam/microswitch interaction in detail
  - 2-wire (self-latching) vs 3-wire (separate signal) motor assemblies
  - How the original VMC board controls motors
  - Tray harness architecture
  - Critical gotchas (MOSFET braking issue, back-EMF, switch bounce, stall current)
- `docs/phase3-motor-control/esp32-controller-design.md` - **Controller design** covering:
  - 3 approaches compared (direct GPIO, shift register, relay module)
  - Recommended approach: ESP32 + 16-channel relay module for Phase 1
  - Complete pin assignments
  - Motor driver circuits (relay and MOSFET versions)
  - Home switch input circuits (pull-up, voltage divider, optocoupler options)
  - Power architecture (12V PSU → buck converter → 5V for ESP32)
  - Software state machine
  - Working starter firmware code (config.h, motor.h, motor.cpp, main.cpp)
  - Shopping list (~$54)
  - 4-step testing plan

### Code (in design doc, not yet in firmware/ directory)
- `config.h` - Pin definitions and timing constants
- `motor.h` / `motor.cpp` - Motor control class with vend() function
- `main.cpp` - Serial interface for testing (send 0-9 to vend)

### What Has NOT Been Built
- No physical circuits
- No firmware flashed to any board
- No power applied to the vending unit
- No multimeter measurements taken
- No parts purchased

---

## Immediate Next Steps

### Step 1: Multimeter Verification (MUST DO FIRST)

With the unit **unplugged**, use a multimeter to:

1. **Motor winding resistance**: Put meter on each motor's Red and Black wires (expect 10-50Ω for 12V motor, 50-200Ω for 24V)
2. **Home switch continuity**: Check between White wire and Black wire with switch pressed and unpressed to determine NO/NC
3. **Wire tracing**: Use continuity mode to trace each motor's 3 wires to specific pins on the main connectors
4. **Ground continuity**: Check if all Black wires are connected together (common ground)
5. **Door switch**: Check NO/NC state when door is open vs closed

Record all findings in `docs/phase2-safety-testing/01-multimeter-verification.md`.

### Step 2: Order Parts

Once voltages are confirmed, order:
- ESP32 DevKit V1 (~$8)
- 16-channel relay module 5V (~$12)
- 12V 5A power supply (~$12)
- LM2596 buck converter 12V→5V (~$3)
- Breadboard + jumper wires (~$8)
- 10kΩ and 1kΩ resistor packs (~$6)
- PC817 optocouplers 10-pack (~$5) - if switch signals are 12V

### Step 3: Single Motor Test

Wire ONE motor through ONE relay to the ESP32. Flash the test firmware. Send `0` over serial. Verify the motor starts, rotates one full turn, and stops at home.

### Step 4: Scale Up

Add remaining motors one at a time. Then add UI (display + keypad).

---

## Key Design Decisions Made

| Decision | Rationale |
|----------|-----------|
| ESP32 over Arduino | WiFi/BLE for CTF scenarios, more GPIO, more RAM |
| Relay module for Phase 1 | Solves motor braking problem, simple, debuggable |
| 3-wire motor assumption | Photos show 3 distinct wires per motor |
| Direct GPIO for switches | Only 10 inputs needed, ESP32 has enough pins |
| PlatformIO over Arduino IDE | Better for version control and CI |
| Safety-first phased approach | Can't undo fried components |

## Key Design Decisions NOT Yet Made

| Decision | Options | Depends On |
|----------|---------|------------|
| MOSFET vs relay (final) | Relay is Phase 1; MOSFET for Phase 2 if braking is manageable | Motor behavior during testing |
| Switch input circuit | Direct pull-up vs optocoupler | Actual switch signal voltage |
| UI type | OLED + keypad vs touchscreen vs web-only | Use case priority |
| Payment integration | MDB protocol vs custom vs none | Whether this is used as real vending |
| Enclosure | Mount inside unit vs external box | Physical space available |

---

## File Map for AI Context

If you're an AI picking this up, here's what to read and in what order:

1. **This file** (PICKUP.md) - You're here
2. **[Motor Deep Dive](docs/phase1-hardware-analysis/03-motor-and-control-system.md)** - Understand the hardware
3. **[ESP32 Design](docs/phase3-motor-control/esp32-controller-design.md)** - Understand the planned controller
4. **[Component Inventory](hardware/COMPONENT_INVENTORY.md)** - What's physically in the unit
5. **[Wiring Diagram](hardware/WIRING_DIAGRAM.md)** - How it's all connected
6. **[Project Plan](docs/PROJECT_PLAN.md)** - Full 14-task plan with success criteria
7. **Photos** in `hardware/photos/initial-inspection/vending-pics/` - 23 JPGs of the actual hardware

## GitHub Repository

- **Owner**: carlfugate
- **Repo**: https://github.com/carlfugate/vending-machine-controller
- **Branch**: main
- **Local path**: ~/Documents/GitHub/vending-machine-controller
