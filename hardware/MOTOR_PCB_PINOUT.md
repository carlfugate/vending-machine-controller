# Motor PCB Pinout Analysis

Based on IMG_9503.JPG (front) and IMG_9505.JPG (back/traces).

## 6-Pin Connector Pinout

**View: Looking at PCB from BACK (trace side), pins numbered left to right**

```
┌─────────────────────────────────┐
│  1    2    3    4    5    6     │  ← Connector pins (back view)
└─────────────────────────────────┘
```

## Pin Assignments (Fill in what each connects to)

| Pin | Verified Connection | Traces To | Component | Function |
|-----|-------------------|-----------|-----------|----------|
| 1   | **Motor Black (Ground)** | ? | ? | Motor Ground |
| 2   | ? | ? | ? | ? |
| 3   | **Motor Red (24V Power)** | ? | ? | Motor Power |
| 4   | ? | ? | ? | ? |
| 5   | **Switch Terminal 1 & 3** | ? | ? | Home Switch Signal |
| 6   | ? | ? | ? | ? |

## Components on PCB

From IMG_9503.JPG (front):
- **Black component**: Motor driver (transistor/MOSFET)
- **Yellow component**: Capacitor (likely for noise filtering)
- **Microswitch**: SPDT 5A 125/250V (Huizhou 10T85)

## Trace Analysis Needed

For each pin, identify what it connects to:
- Motor terminals (Red/Black wires)
- Microswitch terminals (1, 2, 3)
- Driver transistor pins (Base/Gate, Collector/Drain, Emitter/Source)
- Capacitor
- Other pins (power bus, ground bus)

## Instructions

Using continuity mode on multimeter, test each pin and fill in:
1. **Pin 1**: Confirmed → Motor Black wire
2. **Pin 2**: Routes to → ?
3. **Pin 3**: Confirmed → Motor Red wire
4. **Pin 4**: Routes to → ?
5. **Pin 5**: Confirmed → Switch terminals 1 & 3 (when unpressed)
6. **Pin 6**: Routes to → ?

Also test:
- Pin 2 to Pin 1 (ground bus?)
- Pin 4 to Pin 1 (ground bus?)
- Pin 6 to Pin 1 (ground bus?)
- Pin 5 to Pin 1 (when switch unpressed)
- Pin 5 to Pin 3 (when switch unpressed)
