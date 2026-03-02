# Component Inventory - Naturals2Go N2G4000 Entree Unit

**Date**: 2026-03-02  
**Inspection**: Initial visual analysis from photos  
**Status**: Phase 1 - Hardware Documentation

## Summary

- **Total Motors**: 10
- **Total Wiring**: ~35-40 wires in main harness
- **Main Connectors**: 2 multi-pin connectors
- **Power**: External 12V DC (from main cabinet)
- **Control**: Microcontroller-based (to be designed)

---

## Motors

### Motor Specifications

| Item | Specification | Notes |
|------|---------------|-------|
| **Quantity** | 10 motors | 5 per tray × 2 trays |
| **Type** | DC gear motor | Small form factor with gearbox |
| **Voltage** | 12V DC (estimated) | Standard for vending machines |
| **Mounting** | Bolted to tray | Each drives one spiral dispenser |
| **Home Switch** | Integrated microswitch | Position detection |
| **Wiring** | 3-wire harness | Red/Black/White per motor |
| **Part Number** | Unknown | Not visible in photos |

### Motor Locations

| Motor # | Tray | Position | Photo Reference |
|---------|------|----------|-----------------|
| 1 | Top | Left | IMG_9518, IMG_9519 |
| 2 | Top | Center-Left | IMG_9521, IMG_9522 |
| 3 | Top | Center | IMG_9523 |
| 4 | Top | Center-Right | IMG_9524 |
| 5 | Top | Right | IMG_9525 |
| 6 | Bottom | Left | IMG_9526 |
| 7 | Bottom | Center-Left | IMG_9527 |
| 8 | Bottom | Center | IMG_9528 |
| 9 | Bottom | Center-Right | IMG_9529 |
| 10 | Bottom | Right | IMG_9530 |

### Motor Wiring

Each motor has a 3-wire harness:

| Wire Color | Function | Expected Voltage | Notes |
|------------|----------|------------------|-------|
| **Red** | Motor Power | +12V DC | Switched power to motor |
| **Black** | Ground | 0V (GND) | Common ground |
| **White/Yellow** | Home Switch | 0-5V signal | Digital input to controller |

**Total Motor Wires**: 30 (10 motors × 3 wires)

---

## Connectors

### Main Harness Connectors

| Item | Specification | Notes |
|------|---------------|-------|
| **Quantity** | 2 connectors | Stacked vertically on right side |
| **Type** | Multi-pin rectangular | Appears to be Molex-style |
| **Color** | White housing | Standard vending machine connector |
| **Pin Count** | ~20-30 pins each | Estimated from photos |
| **Total Pins** | ~40-60 pins | Both connectors combined |
| **Purpose** | Connect to main cabinet | Power + all control signals |
| **Photo Reference** | IMG_9496, IMG_9503, IMG_9504 |

### Estimated Pinout

Based on 10 motors + power + safety:

| Pin Group | Quantity | Function | Notes |
|-----------|----------|----------|-------|
| Motor Power (Red) | 10 | +12V to each motor | May be switched or PWM |
| Motor Ground (Black) | 10 | GND for each motor | May share common ground |
| Home Switch Signal | 10 | Digital input | To microcontroller |
| Main Power | 2 | +12V and GND | Power distribution |
| Door Switch | 2 | Safety interlock | Normally open/closed TBD |
| Spare/Unused | 6-26 | Future expansion | Depends on actual pin count |

**Note**: Actual pinout must be traced with multimeter in Task 2.

---

## Switches

### Home Switches (Motor Position Sensors)

| Item | Specification | Notes |
|------|---------------|-------|
| **Quantity** | 10 switches | One per motor |
| **Type** | Microswitch | Integrated with motor assembly |
| **Purpose** | Detect home position | Spiral alignment for dispensing |
| **Wiring** | White/Yellow wire | Connected to motor harness |
| **State** | NO or NC (TBD) | Must test with multimeter |
| **Voltage** | 0-5V signal | Digital input level |

### Door Safety Switch

| Item | Specification | Notes |
|------|---------------|-------|
| **Quantity** | 1 switch | On door frame |
| **Type** | Microswitch | Standard SPDT microswitch |
| **Purpose** | Safety interlock | Prevents operation when door open |
| **Wiring** | 2-wire to main harness | Likely NO or NC configuration |
| **Location** | Door frame | Visible in IMG_9524, IMG_9525 |
| **State** | NO or NC (TBD) | Must test with multimeter |

---

## Mechanical Components

### Spiral Dispensers

| Item | Specification | Notes |
|------|---------------|-------|
| **Quantity** | 10 spirals | One per motor |
| **Type** | Helical coil | Pushes product forward |
| **Material** | Metal wire | Coated/painted |
| **Rotation** | Motor-driven | Half-turn per dispense (typical) |
| **Product Orientation** | Horizontal | Products lay flat in spiral |

### Trays

| Item | Specification | Notes |
|------|---------------|-------|
| **Quantity** | 2 trays | Removable for loading |
| **Capacity** | 5 positions each | 10 total product positions |
| **Mounting** | Slide rails | Easy removal for service |
| **Material** | Metal frame | Powder-coated finish |

---

## Wiring Details

### Wire Specifications

| Parameter | Specification | Notes |
|-----------|---------------|-------|
| **Gauge** | 22-24 AWG | Estimated from photos |
| **Insulation** | PVC | Standard wire insulation |
| **Bundling** | Cable ties | Clean wire management |
| **Routing** | Right side | All wires to main connectors |
| **Total Length** | Varies by motor | ~12-24 inches per harness |

### Wire Color Code

| Color | Function | Voltage | Current |
|-------|----------|---------|---------|
| **Red** | Motor + | +12V DC | ~500mA per motor (est.) |
| **Black** | Ground | 0V | Return path |
| **White/Yellow** | Signal | 0-5V | <10mA (switch) |

---

## Power System

### Power Requirements

| Parameter | Specification | Notes |
|-----------|---------------|-------|
| **Input** | From main cabinet | Via main harness connectors |
| **Motor Voltage** | 12V DC | Standard vending machine |
| **Motor Current** | ~500mA each (est.) | 5A total for all 10 motors |
| **Logic Voltage** | 5V or 12V | For switches and controller |
| **Total Power** | ~60W (est.) | 12V × 5A |

### Power Distribution

- **No onboard power supply** - all power from main cabinet
- **Power enters through main harness connectors**
- **Distributed to all 10 motors**
- **Likely has common ground for all motors**

---

## Photos Reference

### External Views
- `IMG_9496.JPG` - Right side showing main connectors
- `IMG_9503.JPG` - Main harness connectors close-up
- `IMG_9504.JPG` - Connector detail
- `IMG_9505.JPG` - Overall right side view

### Motor Details
- `IMG_9507.JPG` - Motor and spiral assembly
- `IMG_9508.JPG` - Motor mounting
- `IMG_9509.JPG` - Motor wiring detail
- `IMG_9517.JPG` - Top tray overview
- `IMG_9518-9525.JPG` - Individual top tray motors
- `IMG_9526-9530.JPG` - Individual bottom tray motors

### Wiring and Connectors
- `IMG_9531.JPG` - Wire bundling and routing
- `IMG_9532.JPG` - Harness detail
- `IMG_9533.JPG` - Overall wiring view

---

## Next Steps

1. **Task 2**: Trace wiring from motors to connectors with multimeter
2. **Task 3**: Identify motor part numbers and specifications
3. **Task 4**: Measure voltages and currents (with current-limited supply)
4. **Phase 2**: Design motor driver circuit for Arduino/ESP32

---

## Notes

- All observations based on visual inspection only
- No power applied during this phase
- Voltage and current values are estimates based on typical vending machine standards
- Actual measurements required in Phase 2 (Safety Testing)
