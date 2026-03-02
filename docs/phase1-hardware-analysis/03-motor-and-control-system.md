# Motor and Control System Deep Dive

**Date**: 2026-03-02  
**Status**: Research Complete  
**Applies To**: Naturals2Go N2G4000 Entree Expansion Unit

---

## Table of Contents

1. [How Vending Machine Spiral Motors Work](#how-vending-machine-spiral-motors-work)
2. [Motor Assembly Anatomy](#motor-assembly-anatomy)
3. [The Home Switch Mechanism](#the-home-switch-mechanism)
4. [Wiring: 2-Wire vs 3-Wire Motor Assemblies](#wiring-2-wire-vs-3-wire-motor-assemblies)
5. [How the Original VMC Controls Motors](#how-the-original-vmc-controls-motors)
6. [Tray Harness Architecture](#tray-harness-architecture)
7. [What We See in Our Unit](#what-we-see-in-our-unit)
8. [Controlling These Motors with an ESP32](#controlling-these-motors-with-an-esp32)
9. [Critical Gotchas](#critical-gotchas)
10. [References](#references)

---

## How Vending Machine Spiral Motors Work

Vending machine spiral (coil) motors are fundamentally simple: a small DC gear motor turns a helical coil (spiral) that pushes product forward. When the spiral completes one full rotation, one product is dispensed off the front of the tray and drops into the delivery bin.

The key challenge is **stopping the motor at exactly the right position** after one full rotation. This is where the home switch comes in.

### The Dispense Cycle

```
1. IDLE (Home Position)
   - Motor is OFF
   - Spiral is aligned so products rest in coil pockets
   - Home switch is in its resting state

2. VEND COMMAND
   - Controller applies power to motor
   - Motor begins turning the spiral
   - Product begins moving forward

3. ROTATION IN PROGRESS
   - Motor continues running
   - Home switch changes state (cam presses or releases it)
   - Product advances through the spiral

4. APPROACHING HOME
   - Spiral nears one full rotation
   - Product falls off the front of the tray
   - Home switch returns to resting state

5. MOTOR STOPS
   - Controller detects home switch state change
   - Power is removed from motor
   - Spiral is back at home position, ready for next vend
```

The entire cycle takes approximately **2-5 seconds** depending on motor speed and load.

---

## Motor Assembly Anatomy

Each motor assembly in the NTG4000 consists of:

### 1. DC Gear Motor
- **Type**: Permanent magnet DC motor with integral gearbox
- **Voltage**: 12V DC (some vending machines use 24V - must verify)
- **Current**: Typically 200-800mA under load
- **Speed**: Geared down to approximately 1-2 RPM at the spiral shaft
- **Direction**: Single direction only (no need for reversal)
- **Flyback diode**: Usually integrated on the motor PCB to suppress voltage spikes when motor is switched off

### 2. Gearbox
- Reduces motor RPM to appropriate spiral rotation speed
- Provides torque multiplication for pushing heavy products
- Typically a multi-stage spur gear or worm gear reduction
- The output shaft connects to the spiral coil driver

### 3. Coil Driver
- The mechanical coupling between the gearbox output and the spiral
- Has 3 adjustment positions for different product sizes (per the NTG4000 manual)
- The spiral clips onto the coil driver

### 4. Home Switch (Microswitch + Cam)
- A small microswitch actuated by a cam on the motor/gearbox shaft
- The cam is shaped so the switch changes state at a specific point in the rotation
- This is the position feedback mechanism

---

## The Home Switch Mechanism

This is the most important thing to understand for building a custom controller.

### How the Cam and Switch Interact

```
        Motor Shaft Rotation (360°)
        ┌──────────────────────────────────────────┐
        │                                          │
   0°   │  HOME POSITION                     360°  │
   ▼    │  ▼                                  ▼    │
   ┌────┼──┬──────────────────────────────────┬────┤
   │CAM │  │         CAM LOBE                 │    │
   │OFF │  │◄────── CAM PRESSING SWITCH ─────►│    │
   │    │  │                                  │    │
   └────┼──┴──────────────────────────────────┴────┤
        │                                          │
        │  ~10°        ~340° of rotation      ~10° │
        └──────────────────────────────────────────┘

Switch State During Rotation:
   Home (0°):     Switch NOT pressed (NC closed, NO open)
   ~10° - ~350°:  Switch PRESSED by cam (NC open, NO closed)
   ~350° - 360°:  Switch released, back to home state
```

### Two Common Switch Configurations

**Configuration A: Switch OPEN at home (most common in 3-wire)**
- At home position: cam is NOT pressing the switch
- During rotation: cam presses the switch
- Motor runs until switch returns to unpressed state
- The controller monitors the switch signal wire

**Configuration B: Switch CLOSED at home (common in 2-wire self-latching)**
- At home position: cam IS pressing the switch
- During rotation: cam releases the switch
- The switch itself is wired in series with the motor to create a self-latching circuit

### Microswitch Terminals

Standard microswitches have 3 terminals:
```
┌─────────────────────┐
│    MICROSWITCH       │
│                      │
│  COM ─── Common      │  Always connected to one of the other two
│  NC  ─── Normally    │  Connected to COM when NOT pressed
│          Closed      │
│  NO  ─── Normally    │  Connected to COM when pressed
│          Open        │
└─────────────────────┘
```

---

## Wiring: 2-Wire vs 3-Wire Motor Assemblies

This is a critical distinction that affects how we build the controller.

### 2-Wire Motor Assembly (Self-Latching)

Older and simpler designs use only 2 external wires. The home switch is wired internally to create a self-latching circuit:

```
                    ┌──────────────────────────┐
                    │     Motor Assembly        │
                    │                           │
    Wire 1 (+) ────┤──┬──── Motor (+) ────┐    │
                    │  │                   │    │
                    │  │    ┌─ Diode ──┐   │    │
                    │  │    │          │   │    │
                    │  └────┤ NC ──COM ├───┘    │
                    │       │  Switch  │        │
                    │       └──────────┘        │
                    │                           │
    Wire 2 (-) ────┤──────── Motor (-) ────────┤
                    │                           │
                    └──────────────────────────┘

How it works:
1. Apply +12V briefly to Wire 1
2. Current flows through diode to motor, motor starts
3. Cam moves off switch, NC closes, latching power through switch
4. Motor continues running through the NC path
5. Cam returns to home, NC opens, motor stops
6. Remove external power

Problem: No separate signal to tell the controller when motor stopped.
The controller must detect current drop or use timing.
```

### 3-Wire Motor Assembly (Separate Signal)

More modern designs (like what appears to be in the NTG4000) use 3 wires:

```
                    ┌──────────────────────────┐
                    │     Motor Assembly        │
                    │                           │
    Wire 1 (Red)  ──┤──────── Motor (+) ───┐    │
    Motor Power     │                      │    │
                    │                      │    │
    Wire 2 (Black)──┤──────── Motor (-) ───┘    │
    Motor Ground    │                           │
                    │       ┌──────────┐        │
                    │       │  Switch   │        │
    Wire 3 (White)──┤───────┤ COM      │        │
    Switch Signal   │       │          │        │
                    │       │ NC ──────┤── GND  │
                    │       │ NO ──────┤── GND  │
                    │       └──────────┘        │
                    │                           │
                    └──────────────────────────┘

How it works:
1. Controller applies +12V to Wire 1 (Red) to run motor
2. Controller reads Wire 3 (White) to monitor switch state
3. When switch state indicates home position reached, controller
   removes power from Wire 1
4. Motor stops at home position

Advantage: Clean separation of power and signal.
Controller has direct feedback on motor position.
```

### Why 3-Wire is Better for Custom Controllers

With 3-wire assemblies:
- Motor power is fully controlled by the external circuit
- Home switch signal is a clean digital input
- No need to detect current changes or use timing hacks
- Easy to interface with Arduino/ESP32 GPIO
- Can implement software debouncing on the switch signal

---

## How the Original VMC Controls Motors

The Seaga NTG4000 uses a VMC (Vending Machine Controller) board that:

### Motor Control Architecture

```
┌─────────────────────────────────────────────────────┐
│                    VMC Board                         │
│                                                      │
│  ┌──────────┐    ┌──────────────┐    ┌───────────┐  │
│  │ Micro-   │    │ Motor Driver │    │ Power     │  │
│  │ controller├───►│ Array       ├───►│ Output    │  │
│  │ (CPU)    │    │ (MOSFETs or │    │ Connectors│  │
│  │          │◄───┤  Darlingtons)│    │           │  │
│  │          │    └──────────────┘    └───────────┘  │
│  │          │                                        │
│  │          │◄─── Home Switch Inputs (10 lines)     │
│  │          │◄─── Door Switch Input                  │
│  │          │◄─── MDB Bus (payment systems)          │
│  │          │◄─── Keypad Input                       │
│  │          │───► Display Output                     │
│  └──────────┘                                        │
└─────────────────────────────────────────────────────┘
```

### Motor Selection and Driving

The VMC uses a **matrix or direct-drive** approach:

**Direct Drive** (most likely for 10 motors):
- Each motor has its own MOSFET or Darlington transistor
- Microcontroller GPIO → Gate/Base → MOSFET/Transistor → Motor
- 10 output pins for 10 motors
- 10 input pins for 10 home switches

**Matrix Drive** (used for larger machines with many motors):
- Row/column addressing to select motors
- Reduces pin count but adds complexity
- Less likely for only 10 motors

### Vend Sequence (from VMC perspective)

```
1. User selects product (keypad input)
2. VMC checks:
   - Payment received (MDB bus)
   - Product not sold out (home switch state)
   - Door closed (door switch)
3. VMC activates motor driver for selected motor
4. VMC monitors home switch for that motor
5. When home switch indicates full rotation:
   - VMC deactivates motor driver
   - VMC confirms vend complete
   - VMC updates audit counters
6. If home switch doesn't trigger within timeout (~8-10 seconds):
   - VMC stops motor
   - VMC reports motor error
   - VMC may attempt retry
```

---

## Tray Harness Architecture

Each tray in the NTG4000 has a **tray harness** - a wiring loom that connects all 5 motors on that tray to a single connector.

### Tray Harness Wiring

```
TRAY HARNESS (5-Wide)
┌─────────────────────────────────────────────────────────┐
│                                                          │
│  Motor 1 ──── Red ────┐                                 │
│              Black ────┤                                 │
│              White ────┤                                 │
│                        │                                 │
│  Motor 2 ──── Red ────┤                                 │
│              Black ────┤     ┌──────────────────┐       │
│              White ────┤     │                  │       │
│                        ├────►│  Tray Connector  │       │
│  Motor 3 ──── Red ────┤     │  (15+ pins)      │       │
│              Black ────┤     │                  │       │
│              White ────┤     └──────────────────┘       │
│                        │            │                    │
│  Motor 4 ──── Red ────┤            │                    │
│              Black ────┤            ▼                    │
│              White ────┤     To Main Harness             │
│                        │     Connector                   │
│  Motor 5 ──── Red ────┤                                 │
│              Black ────┤                                 │
│              White ────┘                                 │
│                                                          │
└─────────────────────────────────────────────────────────┘

Pin allocation per tray (estimated):
  5 × Motor Power (Red)    = 5 pins
  5 × Motor Ground (Black) = 5 pins (may share common ground = 1 pin)
  5 × Home Switch (White)  = 5 pins
  Total: 11-15 pins per tray
```

### Main Harness

The main harness connects the tray harnesses to the main connectors that go to the VMC in the main cabinet:

```
┌──────────┐     ┌──────────────┐     ┌──────────────────┐
│ Top Tray │     │              │     │                  │
│ Harness  ├────►│  Main        ├────►│  Main Connector  │
│ (5 motors)│    │  Harness     │     │  (to VMC in      │
└──────────┘     │              │     │   main cabinet)  │
                 │              │     │                  │
┌──────────┐     │              │     │  Connector 1     │
│ Bottom   │     │              │     │  Connector 2     │
│ Tray     ├────►│              ├────►│                  │
│ Harness  │     │              │     └──────────────────┘
│ (5 motors)│    │              │
└──────────┘     │  + Door SW   │
                 │  + Power     │
                 └──────────────┘
```

---

## What We See in Our Unit

Based on the photo analysis of the NTG4000 entree unit:

### Confirmed Observations

| Feature | Observation | Confidence |
|---------|-------------|------------|
| Motor count | 10 motors (5 per tray × 2 trays) | High |
| Motor type | DC gear motors | High |
| Wire count per motor | 3 wires (Red, Black, White/Yellow) | High |
| Home switches | Integrated microswitch on each motor | High |
| Main connectors | 2 large white multi-pin connectors | High |
| Power supply | None in entree unit | High |
| Door switch | Microswitch on door frame | High |
| Tray design | Removable, slide-rail mounted | High |

### Needs Verification (Multimeter Testing)

| Parameter | Expected | Must Verify |
|-----------|----------|-------------|
| Motor voltage | 12V DC | Yes - could be 24V |
| Motor current | 200-800mA | Yes - measure with ammeter |
| Home switch type | NO at home | Yes - test with continuity |
| Wire function (Red) | Motor + | Yes - trace to connector |
| Wire function (Black) | Motor - / GND | Yes - trace to connector |
| Wire function (White) | Switch signal | Yes - trace to connector |
| Connector pinout | See estimates | Yes - must map every pin |
| Motor resistance | 10-50 ohms | Yes - measure with multimeter |

---

## Controlling These Motors with an ESP32

### Hardware Design

```
                                    12V Power Supply (5A+)
                                         │
                                    ┌────┴────┐
                                    │  +12V   │
                                    │         │
                 ┌──────────┐       │    │    │
                 │  ESP32   │       │    │    │
                 │          │       │    │    │
  Motor 1 Power ─┤GPIO 2  ──┼──►Gate│ MOSFET ├──► Motor 1 Red Wire
  Motor 2 Power ─┤GPIO 4  ──┼──►Gate│ MOSFET ├──► Motor 2 Red Wire
  Motor 3 Power ─┤GPIO 5  ──┼──►Gate│ MOSFET ├──► Motor 3 Red Wire
  ...            │...       │       │  ...   │
  Motor 10 Power─┤GPIO 19 ──┼──►Gate│ MOSFET ├──► Motor 10 Red Wire
                 │          │       │        │
  Home SW 1 ────►┤GPIO 21  │       └────────┘
  Home SW 2 ────►┤GPIO 22  │
  Home SW 3 ────►┤GPIO 23  │    All Motor Black Wires
  ...            │...       │         │
  Home SW 10 ───►┤GPIO 34  │         │
                 │          │    ┌────┴────┐
  Door Switch ──►┤GPIO 35  │    │   GND   │
                 │          │    │ (Common)│
                 └──────────┘    └─────────┘
```

### MOSFET Selection

For 12V DC motors drawing up to 800mA:

| Parameter | Requirement | Recommended Part |
|-----------|-------------|-----------------|
| Vds (max) | >12V | IRLZ44N (55V) or IRL540N (100V) |
| Id (max) | >1A per channel | IRLZ44N (47A) |
| Vgs(th) | <3.3V (ESP32 logic) | IRLZ44N (1-2V threshold) |
| Rds(on) | Low | IRLZ44N (0.022Ω) |

**Important**: Use logic-level MOSFETs (the "L" in IRLZ44N) since ESP32 outputs 3.3V. Standard MOSFETs need 5V+ gate drive and won't fully turn on with 3.3V.

### Flyback Protection

Each motor MUST have a flyback diode to protect the MOSFET from voltage spikes when the motor is switched off:

```
         +12V ────────────────┐
                              │
                         ┌────┴────┐
                         │  Motor  │
                         │         │
                         └────┬────┘
                              │
                    ┌─────────┤
                    │         │
               ┌────┴────┐    │
               │ Flyback │    │
               │ Diode   │    │
               │ (1N4007)│    │
               └────┬────┘    │
                    │         │
                    └─────────┤
                              │
                         ┌────┴────┐
                    Gate─┤ MOSFET  │
                         │(IRLZ44N)│
                         └────┬────┘
                              │
                             GND
```

### Home Switch Input Circuit

The home switch signal needs to be read by the ESP32 (3.3V logic):

```
Option A: If switch signal is 0-5V
  ┌──────────────┐
  │ Home Switch  │
  │ Signal Wire  ├───┬──── 10kΩ ──── 3.3V (pull-up)
  │              │   │
  └──────────────┘   └──── ESP32 GPIO (with internal pull-up)

Option B: If switch signal is 0-12V (needs voltage divider)
  ┌──────────────┐
  │ Home Switch  │
  │ Signal Wire  ├──── 10kΩ ──┬──── ESP32 GPIO
  │              │            │
  └──────────────┘       4.7kΩ
                              │
                             GND

  Vout = 12V × 4.7k / (10k + 4.7k) = 3.84V → add zener or adjust values
  Better: 22kΩ + 10kΩ → Vout = 12V × 10k / (22k + 10k) = 3.75V
```

### Software Control Logic (Pseudocode)

```cpp
// Dispense one product from motor N
bool dispense(int motorPin, int switchPin) {
    // Safety checks
    if (digitalRead(DOOR_SWITCH_PIN) == DOOR_OPEN) return false;

    // Record initial switch state
    bool initialState = digitalRead(switchPin);

    // Start motor
    digitalWrite(motorPin, HIGH);

    // Wait for switch to change state (motor left home)
    unsigned long startTime = millis();
    while (digitalRead(switchPin) == initialState) {
        if (millis() - startTime > TIMEOUT_MS) {
            digitalWrite(motorPin, LOW);  // Safety timeout
            return false;  // ERROR: motor didn't start
        }
    }

    // Wait for switch to return to initial state (motor back at home)
    startTime = millis();
    while (digitalRead(switchPin) != initialState) {
        if (millis() - startTime > TIMEOUT_MS) {
            digitalWrite(motorPin, LOW);  // Safety timeout
            return false;  // ERROR: motor didn't complete rotation
        }
    }

    // Motor is at home - stop it
    digitalWrite(motorPin, LOW);

    // Small delay for motor to fully stop
    delay(50);

    return true;  // Vend successful
}
```

---

## Critical Gotchas

### 1. MOSFET vs Relay for Motor Braking

**Problem**: When you turn off a MOSFET, the motor coasts. When you turn off a relay, the relay contacts short the motor terminals, creating dynamic braking that stops the motor precisely.

**Solution**: Use either:
- A relay (simple but slower switching, mechanical wear)
- A half-bridge MOSFET circuit that shorts the motor when off (better)
- Accept slight overshoot and tune with software timing

This was documented in the EDAboard thread where a user found motors wouldn't stop at home with MOSFETs but worked fine with relays.

### 2. Voltage Spikes (Back-EMF)

When a DC motor is switched off, it generates a voltage spike (back-EMF) that can destroy MOSFETs and microcontrollers.

**Solution**: Always use flyback diodes across each motor. A 1N4007 diode is sufficient.

### 3. Switch Bounce

Microswitches bounce when actuated, producing multiple rapid on/off transitions.

**Solution**: Software debouncing (10-50ms delay after state change) or hardware RC filter.

### 4. Motor Stall Current

If a motor stalls (product jammed), current draw increases dramatically (3-5x normal).

**Solution**: 
- Implement timeout protection (stop motor after 8-10 seconds max)
- Consider adding current sensing (ACS712 module) for stall detection
- Use appropriately rated MOSFETs and power supply

### 5. Verify Voltage Before Connecting

The NTG4000 manual says 12 Amp circuit - this is the total AC draw for the whole machine including refrigeration. The motor voltage could be 12V or 24V DC.

**MUST verify with multimeter before connecting any controller.**

### 6. Common Ground Considerations

If all motor grounds are tied together (common ground), you only need to switch the positive side. If grounds are separate, you need to understand why before connecting them.

---

## References

- [Seaga NTG4000 Owner's Manual](https://www.manualslib.com/manual/2979546/Seaga-Naturals2go-Ntg4000.html) - Official service documentation
- [Seaga INF4S Service Manual](https://www.manualslib.com/manual/3865904/Seaga-Inf4s.html) - Related Seaga model with wiring diagrams
- [Vending machine motor working (EE Stack Exchange)](https://electronics.stackexchange.com/questions/714359/vending-machine-motor-working) - Cam/switch operation explained
- [Vending machine motor and switch circuit (EE Stack Exchange)](https://electronics.stackexchange.com/questions/395162/vending-machine-motor-and-switch-circuit) - Park switch / self-latching circuit
- [MOSFET vs Relay for vending motors (EDAboard)](https://www.edaboard.com/threads/how-to-drive-vending-machine-motor-through-mosfet-driver-module.385927/) - Braking issue with MOSFETs
- [Controlling vending motor with MCU (AllAboutCircuits)](https://forum.allaboutcircuits.com/threads/controlling-an-old-2-pin-vending-machine-motor-with-a-mcu.207056/) - ESP32 control challenges
- [Controlling an array of motors (EE Stack Exchange)](https://electronics.stackexchange.com/questions/391412/controlling-an-array-of-motors) - BC-D27/28 motor switch pinout
- [MDBee - ESP32 MDB Interface (GitHub)](https://github.com/IoTrio/MDBee) - ESP32 MDB protocol implementation
- [VendNet Tray Harness 5W](https://vendnetusa.com/products/tray-harness-5w) - Tray harness component reference
