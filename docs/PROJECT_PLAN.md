# Vending Machine Controller - Project Plan

## Project Information

**Project Name**: Naturals2Go N2G4000 Entree Unit Custom Controller  
**Start Date**: 2026-03-01  
**Target Platform**: Arduino/ESP32 with C++  
**Primary Goal**: Develop custom controller for independent operation

## Problem Statement

The Naturals2Go N2G4000 entree expansion unit was purchased without the main cabinet controller. This project will reverse engineer the control interface and develop a custom controller to enable:

- CTF (Capture the Flag) events at conferences
- Conference and event activations  
- Traditional vending machine operation
- Custom dispensing scenarios

## Requirements

1. **Hardware Documentation First** - Document all components before testing
2. **Safety Priority** - Comprehensive safety approach (voltage ID, isolation, current limiting)
3. **Initial Goal** - Basic motor control (get spirals/dispensers to turn)
4. **Platform** - Arduino/ESP32 with C++
5. **Documentation** - Phase-based organization with research notes in Markdown
6. **Detail Level** - Start high-level, drill down progressively

## Background Research

### Key Findings

- NTG4000 uses DC motors with microswitches for position detection (home switches)
- Motors controlled via VMC (Vending Machine Controller) circuit board
- MDB (Multi-Drop Bus) protocol for payment systems (9-bit serial, master/slave)
- Standard: 110-120V AC input, 12V DC motors
- Safety standards: UL 751 (vending machines), IEC 60335-2-75 (electrical)
- ESP32 MDB implementations exist (MDBee, PCBWay projects)
- Service manual available for NTG4000

### Technical Details from Manual

- Power: 12 Amp circuit, GFCI protected
- Motors: 3-wire harnesses (power, ground, home switch)
- VMC board has MENUS button for service mode
- 2 wire harnesses connect entree unit to main unit
- Temperature sensors and refrigeration controls present

## Implementation Phases

### Phase 1: Hardware Analysis & Documentation
**Goal**: Understand hardware without applying power

**Tasks**:
1. Initial hardware inspection and photography
2. Connector and wiring analysis
3. Motor and switch identification
4. Power supply analysis

**Deliverables**:
- Complete photo documentation
- Component inventory with part numbers
- Wiring diagrams
- Connector pinout documentation
- Motor specifications

### Phase 2: Safety Testing & Validation
**Goal**: Safely validate electrical characteristics

**Tasks**:
5. Safety testing setup (current-limited bench supply)
6. Voltage verification and circuit mapping

**Deliverables**:
- Test bench with safety features
- Verified voltage map
- Power distribution diagram
- Safety procedures document

### Phase 3: Motor Control Development
**Goal**: Achieve basic motor control

**Tasks**:
7. Single motor test circuit
8. Home switch detection and motor homing
9. Controlled rotation and dispensing
10. Multi-motor controller

**Deliverables**:
- Working motor driver circuit
- Homing routine code
- Dispense control code
- Multi-motor control system

### Phase 4: Integration & Testing
**Goal**: Complete functional system

**Tasks**:
11. Simple user interface
12. Safety interlocks and error handling
13. System integration and documentation
14. Field testing and refinement

**Deliverables**:
- User interface (buttons/display)
- Error handling system
- Complete system documentation
- Field test results

## Current Status

**Phase**: 1 - Hardware Analysis & Documentation  
**Task**: 1 - Initial Hardware Inspection and Photography  
**Status**: Ready to begin physical inspection

## Next Actions

1. **Immediate**: Open entree unit and begin photography
2. **Today**: Complete external and internal photography
3. **This Week**: Complete Phase 1 (all 4 tasks)

## Risk Assessment

| Risk | Likelihood | Impact | Mitigation |
|------|------------|--------|------------|
| Component damage during inspection | Low | Medium | No power applied, careful handling |
| Missing documentation | Medium | Medium | Thorough photography, multiple angles |
| Unknown voltages | Medium | High | Current-limited testing in Phase 2 |
| Motor incompatibility | Low | High | Research before purchasing drivers |
| Insufficient power supply | Medium | Medium | Calculate requirements in Phase 1 |

## Resources

### Documentation
- Service manual: Seaga Naturals2GO NTG4000 Owner's Manual
- MDB Protocol specifications
- ESP32 MDB implementations (GitHub)

### Hardware (To Be Determined)
- Arduino or ESP32 development board
- Motor driver modules (L298N or similar)
- Current-limited power supply
- Multimeter and test equipment
- Connectors and wiring

### Software
- Arduino IDE or PlatformIO
- C++ for firmware development
- Git for version control

## Success Criteria

**Phase 1 Complete When**:
- All components photographed and documented
- Component inventory created with part numbers
- Wiring fully mapped
- Power requirements understood

**Phase 2 Complete When**:
- Safe test bench operational
- All voltages verified
- No unexpected electrical characteristics

**Phase 3 Complete When**:
- Single motor controllable
- Homing routine reliable
- All motors individually controllable

**Phase 4 Complete When**:
- User can select and dispense products
- Error handling prevents damage
- System operates reliably in field tests

**Project Complete When**:
- Custom controller fully replaces main cabinet controller
- System operates safely and reliably
- Complete documentation available for reproduction
