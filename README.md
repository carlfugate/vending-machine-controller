# Vending Machine Controller

Custom controller for a Naturals2Go N2G4000 Entree Expansion Unit

## Project Overview

This project involves reverse engineering and developing a custom controller for a Naturals2Go N2G4000 entree expansion unit. The unit was purchased without the main cabinet controller, requiring a ground-up controller implementation.

### Hardware
- **Machine**: Naturals2Go N2G4000 Entree Expansion Unit
- **Dimensions**: 16"W x 28.25"D x 73.5"H
- **Weight**: 210 lbs
- **Interface**: MDB (Multi-Drop Bus) compatible

### Use Cases
- Capture the Flag (CTF) events at conferences
- Conference and event activations
- Traditional vending machine operation
- Custom dispensing scenarios

## Project Goals

1. Reverse engineer the control interface
2. Develop custom controller hardware/software
3. Implement MDB protocol for payment systems
4. Create flexible dispensing logic for various use cases
5. Document the entire process

## Project Structure

```
├── docs/                           # Documentation
│   ├── phase1-hardware-analysis/   # Hardware reverse engineering
│   ├── phase2-safety-testing/      # Safety validation
│   ├── phase3-motor-control/       # Motor control development
│   └── phase4-integration/         # System integration
├── firmware/                       # Controller firmware
│   ├── arduino/                    # Arduino implementations
│   └── esp32/                      # ESP32 implementations
└── hardware/                       # Hardware documentation
    └── photos/                     # Component photos
```

## Current Status

**Phase**: Phase 1 - Hardware Analysis & Documentation
**Current Task**: Task 1 - Initial Hardware Inspection and Photography

## Development Approach

This project follows a safety-first, phase-based approach:

1. **Phase 1**: Hardware Analysis & Documentation
2. **Phase 2**: Safety Testing & Validation
3. **Phase 3**: Motor Control Development
4. **Phase 4**: Integration & Testing

See [docs/](docs/) for detailed documentation of each phase.

## Safety Notes

⚠️ **WARNING**: This project involves working with electrical systems and motors. Always:
- Disconnect power before working on hardware
- Use current-limited power supplies during testing
- Verify voltages before connecting components
- Follow proper electrical safety procedures

## License

TBD