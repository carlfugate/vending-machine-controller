# Task 1: Initial Hardware Inspection and Photography

**Date Started**: 2026-03-01  
**Status**: In Progress  
**Objective**: Document the entree unit's internal layout without powering anything

## Safety Checklist

- [ ] Unit is unplugged from power
- [ ] All capacitors have been discharged (if applicable)
- [ ] Work area is clear and well-lit
- [ ] Camera/phone ready for documentation
- [ ] Notebook ready for notes

## Inspection Procedure

### 1. External Inspection

**Dimensions** (from spec):
- Width: 16"
- Depth: 28.25"
- Height: 73.5"
- Weight: 210 lbs

**External Features**:
- [ ] Door/access panels identified
- [ ] Lock mechanisms documented
- [ ] External connectors noted
- [ ] Labels/model numbers photographed

**Photos to capture**:
- Front view
- Back view
- Both sides
- Top view
- Bottom view (if accessible)
- All labels and markings

### 2. Internal Access

**Access Method**:
- Document how to open the unit
- Note any special tools required
- Photograph locking mechanisms

### 3. Internal Component Identification

**Components to identify and photograph**:

#### Motors
- [ ] Count total number of motors
- [ ] Note motor locations (which spiral/position)
- [ ] Photograph each motor
- [ ] Document any visible markings/part numbers
- [ ] Note wire harness connections

#### Circuit Boards
- [ ] Identify any existing control boards
- [ ] Document board locations
- [ ] Photograph boards from multiple angles
- [ ] Note any labels or part numbers

#### Connectors
- [ ] Main power connector(s)
- [ ] Inter-unit harness connectors (2 expected from manual)
- [ ] Motor connectors
- [ ] Any other connectors
- [ ] Document connector types and pin counts

#### Wiring
- [ ] Main power wiring
- [ ] Motor wiring harnesses
- [ ] Control signal wiring
- [ ] Note wire colors and gauges

#### Power Supply
- [ ] Locate transformer or power supply
- [ ] Document specifications if visible
- [ ] Photograph power supply

#### Switches
- [ ] Home switches on motors
- [ ] Door switches
- [ ] Any other switches
- [ ] Document switch types

#### Other Components
- [ ] Sensors (temperature, position, etc.)
- [ ] Relays or contactors
- [ ] Fuses or circuit breakers
- [ ] Any other notable components

## Component Map

```
RIGHT SIDE (Main Harness Connectors)
├── White Connector 1 (Top) - ~20-30 pins
└── White Connector 2 (Bottom) - ~20-30 pins
    │
    └─── All motor wires route here

TOP TRAY (5 Motors)
├── Motor 1 (Left) ──── 3-wire harness (Red/Black/White)
├── Motor 2 ──────────── 3-wire harness (Red/Black/White)
├── Motor 3 ──────────── 3-wire harness (Red/Black/White)
├── Motor 4 ──────────── 3-wire harness (Red/Black/White)
└── Motor 5 (Right) ──── 3-wire harness (Red/Black/White)

BOTTOM TRAY (5 Motors)
├── Motor 6 (Left) ──── 3-wire harness (Red/Black/White)
├── Motor 7 ──────────── 3-wire harness (Red/Black/White)
├── Motor 8 ──────────── 3-wire harness (Red/Black/White)
├── Motor 9 ──────────── 3-wire harness (Red/Black/White)
└── Motor 10 (Right) ─── 3-wire harness (Red/Black/White)

DOOR FRAME
└── Door Safety Switch (Microswitch)
```

## Initial Observations

### Motors
- **Count**: 10 motors total (5 per tray, 2 trays)
- **Type**: Small DC gear motors with integrated microswitches
- **Visible Markings**: No clear part numbers visible in photos
- **Harness Type**: 3-wire per motor (30 wires total)
- **Wire Colors**: Red (+12V), Black (GND), White/Yellow (home switch signal)
- **Mounting**: Each motor bolted to tray, drives spiral dispenser
- **Home Switch**: Microswitch integrated on each motor assembly

### Connectors
- **Main Harness Connectors**: 2 large white multi-pin connectors on right side
- **Pin Count**: Estimated 20-30 pins each (40-60 total)
- **Connector Type**: Standard rectangular housing, appears to be Molex-style
- **Purpose**: Connect entree unit to main cabinet (power + control signals)
- **Wire Routing**: All motor harnesses bundle and terminate here

### Power Supply
- **Location**: No power supply visible in entree unit
- **Visible Specs**: N/A - power supplied from main cabinet
- **Input Voltage**: Comes through main harness connectors
- **Output Voltage(s)**: Expected 12V DC for motors (standard vending machine)
- **Distribution**: Power distributed through main harness to all motors

### Wiring
- **Wire Gauge**: Appears to be 22-24 AWG for motor wires
- **Color Coding**: 
  - Red = Motor power (+12V)
  - Black = Ground (common)
  - White/Yellow = Home switch signal (digital input)
- **Routing**: Clean bundling with tie-downs, routes from motors to right side connectors
- **Total Wire Count**: 30 motor wires + power rails + door switch = ~35-40 wires

### Door Safety Switch
- **Type**: Microswitch on door frame
- **Purpose**: Safety interlock - prevents operation when door open
- **Wiring**: Likely 2-wire connection to main harness
- **Location**: Visible in door frame area

## Questions/Concerns

- **Motor voltage**: Need to verify 12V DC with multimeter testing
- **Home switch type**: Need to determine normally open (NO) or normally closed (NC)
- **Connector pinout**: Must trace each wire to identify exact pin assignments
- **Current draw**: Need to measure motor current for driver sizing
- **Switch debouncing**: May need software or hardware debouncing for home switches
- **Door interlock**: Need to understand door switch logic (NO/NC)

## Next Steps

1. ✅ Complete photography of all components
2. ✅ Organize photos in `hardware/photos/` directory
3. ✅ Create detailed component inventory
4. **Next**: Move to Task 2: Connector and Wiring Analysis

## Photos

Photos are stored in: `hardware/photos/initial-inspection/`

### Photo Index
- `external-front.jpg` - Front view of unit
- `external-back.jpg` - Back view of unit
- `external-left.jpg` - Left side view
- `external-right.jpg` - Right side view
- `internal-overview.jpg` - Overall internal layout
- `motor-01.jpg` through `motor-XX.jpg` - Individual motor photos
- `connectors-main.jpg` - Main harness connectors
- `power-supply.jpg` - Power supply unit
- `wiring-overview.jpg` - Overall wiring layout

## Notes

[Add any additional observations, concerns, or findings here]
