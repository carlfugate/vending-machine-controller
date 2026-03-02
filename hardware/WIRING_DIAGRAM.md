# Wiring Diagram - Naturals2Go N2G4000 Entree Unit

**Date**: 2026-03-02  
**Status**: Preliminary - Based on Visual Inspection  
**Note**: Actual pinout must be verified with multimeter testing

---

## System Overview

```
┌─────────────────────────────────────────────────────────────────┐
│                    ENTREE UNIT (Standalone)                      │
│                                                                   │
│  ┌──────────┐  ┌──────────┐  ┌──────────┐  ┌──────────┐  ┌────┐│
│  │ Motor 1  │  │ Motor 2  │  │ Motor 3  │  │ Motor 4  │  │ M5 ││
│  │  + SW1   │  │  + SW2   │  │  + SW3   │  │  + SW4   │  │+SW5││
│  └─┬─┬─┬────┘  └─┬─┬─┬────┘  └─┬─┬─┬────┘  └─┬─┬─┬────┘  └┬┬┬─┘│
│    │ │ │         │ │ │         │ │ │         │ │ │         │││  │
│    R B W         R B W         R B W         R B W         RBW  │
│    │ │ │         │ │ │         │ │ │         │ │ │         │││  │
│  ┌──────────┐  ┌──────────┐  ┌──────────┐  ┌──────────┐  ┌────┐│
│  │ Motor 6  │  │ Motor 7  │  │ Motor 8  │  │ Motor 9  │  │M10 ││
│  │  + SW6   │  │  + SW7   │  │  + SW8   │  │  + SW9   │  │+S10││
│  └─┬─┬─┬────┘  └─┬─┬─┬────┘  └─┬─┬─┬────┘  └─┬─┬─┬────┘  └┬┬┬─┘│
│    │ │ │         │ │ │         │ │ │         │ │ │         │││  │
│    R B W         R B W         R B W         R B W         RBW  │
│    │ │ │         │ │ │         │ │ │         │ │ │         │││  │
│    └─┴─┴─────────┴─┴─┴─────────┴─┴─┴─────────┴─┴─┴─────────┴┴┴──┤
│                                                                   │
│                          ┌──────────────┐                         │
│                          │ Door Switch  │                         │
│                          └──────┬───────┘                         │
│                                 │                                 │
│    ┌────────────────────────────┴─────────────────────────────┐  │
│    │                                                           │  │
│    │  ┌─────────────────────────────────────────────────┐    │  │
│    │  │         MAIN HARNESS CONNECTORS                 │    │  │
│    │  │  ┌──────────────────────────────────────────┐   │    │  │
│    │  │  │  Connector 1 (Top) - ~20-30 pins        │   │    │  │
│    │  │  └──────────────────────────────────────────┘   │    │  │
│    │  │  ┌──────────────────────────────────────────┐   │    │  │
│    │  │  │  Connector 2 (Bottom) - ~20-30 pins     │   │    │  │
│    │  │  └──────────────────────────────────────────┘   │    │  │
│    │  └─────────────────────────────────────────────────┘    │  │
│    │                                                           │  │
│    │  (All wires terminate here - connects to main cabinet)   │  │
│    └───────────────────────────────────────────────────────────┘  │
│                                                                   │
└─────────────────────────────────────────────────────────────────┘

Legend:
  R = Red wire (Motor +12V)
  B = Black wire (Ground)
  W = White/Yellow wire (Home switch signal)
  SW = Home switch (microswitch on motor)
```

---

## Motor Wiring Detail

### Individual Motor Connection

```
                    Motor Assembly
         ┌──────────────────────────────┐
         │                              │
         │   ┌──────────────┐           │
         │   │   DC Motor   │           │
         │   │   (12V DC)   │           │
         │   └───┬──────┬───┘           │
         │       │      │               │
         │      (+)    (-)              │
         │       │      │               │
         │   ┌───┴──────┴───┐           │
         │   │  Gearbox     │           │
         │   └──────┬───────┘           │
         │          │                   │
         │     ┌────┴────┐              │
         │     │ Spiral  │              │
         │     │Dispenser│              │
         │     └─────────┘              │
         │                              │
         │   ┌──────────────┐           │
         │   │ Microswitch  │           │
         │   │ (Home Sensor)│           │
         │   └──┬────────┬──┘           │
         │      │        │              │
         │     COM       NO/NC          │
         │      │        │              │
         └──────┼────────┼──────────────┘
                │        │
                │        │
         ┌──────┴────────┴──────┐
         │   3-Wire Harness     │
         │                      │
         │  RED    ────────────────> Motor + (12V)
         │  BLACK  ────────────────> Motor - (GND)
         │  WHITE  ────────────────> Switch Signal (0-5V)
         │                      │
         └──────────────────────┘
```

---

## Estimated Main Harness Pinout

**Note**: This is a preliminary estimate. Actual pinout MUST be verified with multimeter.

### Connector 1 (Top) - Estimated 20-30 pins

| Pin # | Function | Wire Color | Voltage | Notes |
|-------|----------|------------|---------|-------|
| 1 | Motor 1 Power | Red | +12V | Switched |
| 2 | Motor 1 Ground | Black | GND | Common |
| 3 | Motor 1 Home Switch | White | 0-5V | Digital input |
| 4 | Motor 2 Power | Red | +12V | Switched |
| 5 | Motor 2 Ground | Black | GND | Common |
| 6 | Motor 2 Home Switch | White | 0-5V | Digital input |
| 7 | Motor 3 Power | Red | +12V | Switched |
| 8 | Motor 3 Ground | Black | GND | Common |
| 9 | Motor 3 Home Switch | White | 0-5V | Digital input |
| 10 | Motor 4 Power | Red | +12V | Switched |
| 11 | Motor 4 Ground | Black | GND | Common |
| 12 | Motor 4 Home Switch | White | 0-5V | Digital input |
| 13 | Motor 5 Power | Red | +12V | Switched |
| 14 | Motor 5 Ground | Black | GND | Common |
| 15 | Motor 5 Home Switch | White | 0-5V | Digital input |
| 16-20 | Main Power/Ground | Red/Black | +12V/GND | Distribution |
| 21-30 | Spare/Unused | - | - | Future expansion |

### Connector 2 (Bottom) - Estimated 20-30 pins

| Pin # | Function | Wire Color | Voltage | Notes |
|-------|----------|------------|---------|-------|
| 1 | Motor 6 Power | Red | +12V | Switched |
| 2 | Motor 6 Ground | Black | GND | Common |
| 3 | Motor 6 Home Switch | White | 0-5V | Digital input |
| 4 | Motor 7 Power | Red | +12V | Switched |
| 5 | Motor 7 Ground | Black | GND | Common |
| 6 | Motor 7 Home Switch | White | 0-5V | Digital input |
| 7 | Motor 8 Power | Red | +12V | Switched |
| 8 | Motor 8 Ground | Black | GND | Common |
| 9 | Motor 8 Home Switch | White | 0-5V | Digital input |
| 10 | Motor 9 Power | Red | +12V | Switched |
| 11 | Motor 9 Ground | Black | GND | Common |
| 12 | Motor 9 Home Switch | White | 0-5V | Digital input |
| 13 | Motor 10 Power | Red | +12V | Switched |
| 14 | Motor 10 Ground | Black | GND | Common |
| 15 | Motor 10 Home Switch | White | 0-5V | Digital input |
| 16-17 | Door Switch | - | 0-5V | Safety interlock |
| 18-20 | Main Power/Ground | Red/Black | +12V/GND | Distribution |
| 21-30 | Spare/Unused | - | - | Future expansion |

---

## Alternative Pinout (Common Ground)

It's possible the design uses a common ground bus to reduce wiring:

```
Main Power Rails:
  +12V ────────┬─────┬─────┬─────┬─────┬─────┬─────┬─────┬─────┬─────┬─────┐
               │     │     │     │     │     │     │     │     │     │     │
            Motor1 Motor2 Motor3 Motor4 Motor5 Motor6 Motor7 Motor8 Motor9 Motor10
               │     │     │     │     │     │     │     │     │     │     │
  GND  ────────┴─────┴─────┴─────┴─────┴─────┴─────┴─────┴─────┴─────┴─────┘

In this case:
  - 2 pins for power distribution (+12V, GND)
  - 10 pins for motor power switching (one per motor)
  - 10 pins for home switch signals (one per motor)
  - 2 pins for door switch
  - Total: 24 pins minimum
```

---

## Custom Controller Interface

### Required Connections for Arduino/ESP32 Controller

```
┌─────────────────────────────────────────────────────────────┐
│                  Arduino/ESP32 Controller                    │
│                                                              │
│  Digital Outputs (10):                                      │
│    GPIO 2-11  ──> Motor Driver Board ──> Motors 1-10       │
│                                                              │
│  Digital Inputs (10):                                       │
│    GPIO 22-31 <── Home Switches 1-10                        │
│                                                              │
│  Digital Input (1):                                         │
│    GPIO 32    <── Door Safety Switch                        │
│                                                              │
│  Power:                                                     │
│    5V/3.3V    <── Voltage Regulator <── 12V Supply         │
│    GND        <── Common Ground                             │
│                                                              │
└─────────────────────────────────────────────────────────────┘
         │                                    │
         │                                    │
         ▼                                    ▼
┌──────────────────┐              ┌──────────────────┐
│  Motor Driver    │              │   12V Power      │
│  Board (L298N    │              │   Supply         │
│  or similar)     │              │   (5A minimum)   │
│                  │              │                  │
│  10 channels     │              │  +12V ──────────┐│
│  for 10 motors   │              │  GND  ──────────┘│
└──────────────────┘              └──────────────────┘
```

---

## Power Distribution

### Current Requirements

| Component | Quantity | Current Each | Total Current |
|-----------|----------|--------------|---------------|
| DC Motors | 10 | ~500mA | 5A |
| Controller | 1 | ~200mA | 0.2A |
| **Total** | - | - | **~5.2A @ 12V** |

### Power Supply Specifications

- **Voltage**: 12V DC regulated
- **Current**: Minimum 5A, recommended 8A for safety margin
- **Power**: ~60W minimum, 100W recommended
- **Type**: Switching power supply with overcurrent protection

---

## Safety Considerations

### Door Interlock

```
Door Switch Logic (TBD - must verify):

Option 1: Normally Closed (NC)
  Door Closed  → Switch Closed  → Signal = GND (0V) → Motors Enabled
  Door Open    → Switch Open    → Signal = +5V      → Motors Disabled

Option 2: Normally Open (NO)
  Door Closed  → Switch Closed  → Signal = +5V      → Motors Enabled
  Door Open    → Switch Open    → Signal = GND (0V) → Motors Disabled
```

### Emergency Stop

Controller should implement:
- Door switch monitoring (disable motors if door opens)
- Motor timeout (stop if motor runs too long)
- Overcurrent detection (stop if motor draws too much current)
- Home switch validation (error if home not detected)

---

## Next Steps

1. **Task 2**: Use multimeter to trace and verify all connections
2. **Task 2**: Identify actual connector pinout
3. **Task 3**: Measure motor specifications (voltage, current, resistance)
4. **Task 4**: Test home switch operation (NO vs NC, voltage levels)
5. **Phase 2**: Design and build motor driver circuit
6. **Phase 3**: Implement controller firmware

---

## Notes

- This diagram is based on visual inspection only
- Actual pinout may differ significantly
- DO NOT apply power until pinout is verified with multimeter
- All voltage and current values are estimates
- Safety testing required before connecting controller
