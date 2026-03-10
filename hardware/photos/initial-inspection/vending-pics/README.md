# Vending Machine Internal Photos

Complete photographic documentation of Naturals2Go N2G4000 entree expansion unit internals taken during initial hardware analysis (March 2026).

---

## Motor Assembly & PCB

### IMG_9496.JPG - Motor PCB with Connector
![Motor PCB Assembly](IMG_9496.JPG)
- Complete motor control PCB assembly
- 6-pin white connector with slide lock mechanism
- Green PCB with component side visible
- White plastic motor housing
- Multiple colored wires (red, black, orange, blue, pink, green)

### IMG_9503.JPG - PCB Front (Component Side)
![PCB Front](IMG_9503.JPG)
- **Date Code**: 2010.06.23
- **Labels**: BCMK, CT, RoHS_004, SWITCH_004, SW1
- **Components**:
  - 6-pin white connector (top)
  - Black microswitch (Huizhou 10T85)
  - Yellow capacitor (noise filtering)
  - Silver cylindrical diodes (flyback protection)
  - Multiple solder pads and traces

### IMG_9504.JPG - Microswitch Close-up
![Microswitch Detail](IMG_9504.JPG)
- Huizhou microswitch mounted on PCB
- Visible markings on switch body
- PCB label: "SW1 RoHS-004"
- Red and black wires connected

### IMG_9505.JPG - PCB Back (Trace Side)
![PCB Traces](IMG_9505.JPG)
- **Single-layer PCB** confirmed
- Clear copper trace routing visible
- Ground plane marked "⊥"
- Test point marked "T"
- 6 connector pins at top
- Multiple component pads (diodes, capacitor, switch)

### IMG_9507.JPG - Motor Label
![Motor Model Number](IMG_9507.JPG)
- **Motor Model**: ZYT028T-001
- **Date Code**: S140915 (September 15, 2014)
- **Confirmed Specs**: 24V DC, 100-140Ω winding resistance
- Permanent magnet DC motor with gearbox

### IMG_9508.JPG - Motor Assembly Side View
![Motor Side View](IMG_9508.JPG)
- Blue motor housing
- White plastic spiral/cam mechanism
- PCB mounted on top
- Orange wire visible

### IMG_9509.JPG - Motor with Screen Reference
![Motor Assembly](IMG_9509.JPG)
- Complete motor assembly on blue mounting bracket
- PCB with 6-pin connector
- White plastic gear/cam
- Screen showing similar vending motor reference

### IMG_9891.JPG - Microswitch Specifications
![Microswitch Specs](IMG_9891.JPG)
- **Rating**: 5A 125 250V~
- **Model**: 10T85
- **Type**: SPDT (3 terminals)
- **Manufacturer**: Huizhou
- **Function**: Cam-activated home position detection
- **Terminals**: 1 (left), 2 (middle/common), 3 (right)

---

## Main Harness Connectors

### IMG_9517.JPG - Door Safety Switch
![Door Switch](IMG_9517.JPG)
- Black plastic housing
- Microswitch with pink/red actuator button
- Mounted on door frame
- Safety interlock for door open/close detection

### IMG_9518.JPG - Main Harness Bundle
![Harness Bundle](IMG_9518.JPG)
- Large multi-wire harness
- Multiple colored wires visible (orange, pink, yellow, green, blue, purple, black)
- White connector housings
- Cable management visible

### IMG_9519.JPG - Connector Detail (4-pin)
![4-pin Connector](IMG_9519.JPG)
- White 4-pin connector
- Wire colors: Blue, Pink/Purple, Green
- Slide-lock mechanism visible
- Appears to be power or control connector

### IMG_9521.JPG - Connector Detail (3-pin)
![3-pin Connector](IMG_9521.JPG)
- White 3-pin connector
- Wire colors: Blue, Purple, Orange
- Compact housing design

---

## Motor Installation & Wiring

### IMG_9522.JPG - Main Connector (Large)
![Large Main Connector](IMG_9522.JPG)
- Large white multi-pin connector
- Multiple wire colors: Blue, Purple, Orange, Pink
- Slide-lock housing with 3 sections
- Likely main power/control bus

### IMG_9523.JPG - Motor Installed in Tray
![Motor in Tray](IMG_9523.JPG)
- Motor mounted on black metal tray
- PCB visible with wiring
- White plastic spiral mechanism
- Harness routing visible

### IMG_9524.JPG - Motor Wiring Overview
![Motor Wiring](IMG_9524.JPG)
- Complete motor assembly in context
- Multiple colored wires routed from PCB
- Black tray mounting
- White spiral visible

### IMG_9525.JPG - Wire Routing Detail
![Wire Routing](IMG_9525.JPG)
- Close-up of wire bundle from PCB
- Colors: Orange, Red, Green, Pink, Blue, Purple, Black
- Heat shrink tubing visible on some wires
- Organized routing from connector

---

## Verified Specifications

### Motor
- **Model**: ZYT028T-001 (28mm frame, permanent magnet DC)
- **Voltage**: 24V DC (confirmed via 100-140Ω resistance measurement)
- **Type**: 2-wire motor (Red/Black) + separate home switch
- **Manufacturer**: Chinese OEM (ZYT series)
- **Date**: September 2014

### Home Switch
- **Type**: Mechanical SPDT microswitch (Huizhou 10T85)
- **Rating**: 5A @ 125/250V AC
- **Activation**: Cam on motor shaft
- **Mounting**: PCB-mounted, not motor-integrated
- **Terminals**: 3-pin (Common, NO, NC)

### PCB
- **Type**: Single-layer through-hole
- **Date**: June 23, 2010
- **Function**: Motor interface breakout + switch mounting
- **Protection**: Flyback diodes for back-EMF
- **Connector**: 6-pin white slide-lock (likely 2.54mm pitch)

### Connector Pinout (Partially Verified)
From back of PCB (trace side), pins 1-6 left to right:
- **Pin 1**: Motor Ground (Black wire) - VERIFIED
- **Pin 2**: TBD
- **Pin 3**: Motor Power 24V (Red wire) - VERIFIED  
- **Pin 4**: TBD
- **Pin 5**: Home Switch Signal - VERIFIED
- **Pin 6**: TBD

### System Architecture
- **Motor Control**: Low-side switching (ground switched by VMC)
- **Power**: 24V always present on Pin 3
- **Driver Location**: Main cabinet VMC board (not on motor PCB)
- **Switch Logic**: Separate signal path through Pin 5

---

## Photo Index

| Photo | Subject | Key Details |
|-------|---------|-------------|
| IMG_9496 | Motor PCB assembly | Complete unit with connector |
| IMG_9503 | PCB front | Components, date code, labels |
| IMG_9504 | Microswitch closeup | Switch mounting detail |
| IMG_9505 | PCB back | Traces, single-layer confirmed |
| IMG_9507 | Motor label | Model ZYT028T-001, date S140915 |
| IMG_9508 | Motor side view | Housing and cam mechanism |
| IMG_9509 | Motor assembly | Complete unit with reference |
| IMG_9517 | Door switch | Safety interlock |
| IMG_9518 | Main harness | Multi-wire bundle |
| IMG_9519 | 4-pin connector | Power/control connector |
| IMG_9521 | 3-pin connector | Compact connector |
| IMG_9522 | Large connector | Main bus connector |
| IMG_9523 | Motor in tray | Installation context |
| IMG_9524 | Motor wiring | Wiring overview |
| IMG_9525 | Wire routing | Detailed wire colors |
| IMG_9891 | Microswitch specs | Rating and model number |

---

## Analysis Status

✅ **Complete:**
- Motor voltage identified (24V DC)
- Motor model documented (ZYT028T-001)
- Switch type identified (Huizhou 10T85 SPDT)
- PCB architecture understood (interface breakout, no driver)
- Pin 1, 3, 5 functions verified

⏳ **In Progress:**
- Complete connector pinout (Pins 2, 4, 6)
- Main harness connector identification
- Wire color to function mapping

📋 **Next Steps:**
- Trace remaining pins (2, 4, 6) with continuity testing
- Identify main harness connector type and pinout
- Document complete wiring diagram
- Test motor operation with 24V supply
