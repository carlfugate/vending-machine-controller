# Motor PCB Complete Schematic

Reverse engineered from IMG_9503.JPG (front) and IMG_9505.JPG (back/traces).

## Reference Photos

### PCB Front
![PCB Front](photos/initial-inspection/vending-pics/IMG_9503.JPG)

### PCB Back (Traces)
![PCB Back](photos/initial-inspection/vending-pics/IMG_9505.JPG)

### Microswitch Detail
![Microswitch](photos/initial-inspection/vending-pics/IMG_9891.JPG)

### Motor Label
![Motor Label](photos/initial-inspection/vending-pics/IMG_9507.JPG)

## PCB Overview

**Board**: Single-layer PCB, dated 2010.06.23
**Function**: Motor driver with home position detection
**Labels**: RoHS-004, SWITCH_004, BCMK, CT, BLK, SW1

## Component Identification

### Visible Components (from front - IMG_9503.JPG)
1. **J1**: 6-pin white connector (main interface)
2. **Q1**: Black transistor/MOSFET (motor driver)
3. **C1**: Yellow capacitor (noise filtering)
4. **SW1**: Microswitch - Huizhou 10T85, SPDT, 5A 125/250V, cam-activated
5. **M1**: DC motor connection points (Red/Black wires)

### Trace Analysis (from back - IMG_9505.JPG)

**Connector J1 (6 pins, left to right from back view):**
```
┌─────────────────────────────────┐
│  1    2    3    4    5    6     │  ← Connector pins (back view)
└─────────────────────────────────┘
```

**Component Pads Visible:**
- Top row: 6 pads (connector pins)
- Middle row: 4 pads (likely transistor Q1)
- Bottom row: 5 pads (likely switch SW1 and other components)
- Blue capacitor C1 (left side)
- Ground plane marked "⊥"
- Test point marked "T"

## Pinout Mapping (To Be Verified)

| Pin | Verified | Likely Connection | Test Results |
|-----|----------|-------------------|--------------|
| 1   | ✅ | Motor Ground (Black wire) | Confirmed via continuity |
| 2   | ❓ | TBD | Need trace analysis |
| 3   | ✅ | Motor Power 24V (Red wire) | Confirmed via continuity |
| 4   | ❓ | TBD | Need trace analysis |
| 5   | ✅ | Home Switch Signal | Connects to SW1 terminals 1&3 |
| 6   | ❓ | TBD | Need trace analysis |

## Continuity Test Matrix

**Instructions**: Use multimeter in continuity mode to fill in this matrix.

Test each connector pin against all components and other pins:

| From Pin | To Pin/Component | Continuity? | Notes |
|----------|------------------|-------------|-------|
| Pin 1 | Motor Black | ✅ YES | Confirmed |
| Pin 1 | Pin 2 | ? | Test for ground bus |
| Pin 1 | Pin 4 | ? | Test for ground bus |
| Pin 1 | Pin 6 | ? | Test for ground bus |
| Pin 1 | SW1 terminal 2 | ? | Check if switch common is grounded |
| Pin 2 | Q1 (transistor) | ? | Likely base/gate control signal |
| Pin 3 | Motor Red | ✅ YES | Confirmed |
| Pin 3 | Q1 (transistor) | ? | Likely collector/drain |
| Pin 4 | ? | ? | Unknown |
| Pin 5 | SW1 terminal 1 | ✅ YES | Confirmed |
| Pin 5 | SW1 terminal 3 | ✅ YES (when unpressed) | Confirmed |
| Pin 5 | Pin 1 (GND) | ? | Test when switch unpressed |
| Pin 6 | ? | ? | Unknown |

## Expected Circuit Architecture

Based on typical vending motor driver designs:

```
Pin 1 (GND) ──────┬─── Motor Black (Ground)
                  └─── Q1 Emitter/Source
                  
Pin 2 (CTRL) ─────── Q1 Base/Gate (Motor control signal from VMC)

Pin 3 (24V) ──────┬─── Motor Red (via Q1)
                  └─── Q1 Collector/Drain
                  
Pin 4 (?) ────────── Possibly second ground or unused

Pin 5 (HOME) ─────── SW1 (Home switch signal to VMC)

Pin 6 (?) ────────── Possibly switch common or power
```

## Next Steps

1. Test Pin 2 → Q1 connection (should be control signal)
2. Test Pin 4 → determine if ground or other function
3. Test Pin 6 → determine function
4. Test Pin 5 → Pin 1 continuity when switch is unpressed
5. Identify Q1 part number from front photo
6. Draw complete schematic with all connections verified
