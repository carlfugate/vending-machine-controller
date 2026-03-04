# Vending Machine Controller

Custom controller for a Naturals2Go N2G4000 Entree Expansion Unit.

## What Is This?

A Naturals2Go N2G4000 **entree expansion unit** was purchased without the main cabinet that normally controls it. This project reverse engineers the control interface and builds a custom ESP32-based controller from scratch.

The unit will be used for:
- **Capture the Flag (CTF) events** at security conferences
- **Conference and event activations** (custom dispensing scenarios)
- **Traditional vending** when needed

## Machine Specs

| Spec | Value |
|------|-------|
| Model | Naturals2Go N2G4000 Entree Expansion Unit |
| Manufacturer | Seaga |
| Dimensions | 16"W × 28.25"D × 73.5"H |
| Weight | 210 lbs |
| Motors | 10 DC gear motors (5 per tray × 2 trays) |
| Motor Wiring | 3-wire per motor (Red/Black/White) |
| Interface | MDB (Multi-Drop Bus) compatible |
| Power | 12V DC motors (estimated, needs verification) |

## Project Status

| Phase | Status | Summary |
|-------|--------|---------|
| **Phase 1**: Hardware Analysis | ✅ Mostly Complete | Photos taken, motors identified, wiring mapped, deep research done |
| **Phase 2**: Safety Testing | ⏳ Not Started | Need multimeter verification of voltages and pinouts |
| **Phase 3**: Motor Control | 📐 Designed | ESP32 controller design complete, firmware sketched, not built |
| **Phase 4**: Integration | ⏳ Not Started | UI, safety interlocks, field testing |

### What's Done

- 23 photos of internals taken and committed
- 10 motors identified (DC gear motors with integrated home switches)
- 3-wire motor harnesses documented (Red = power, Black = ground, White = home switch signal)
- 2 main harness connectors identified (~40-60 pins total)
- Door safety switch located
- Deep research on how vending spiral motors work (cam + microswitch mechanism)
- ESP32 controller design with 3 approaches analyzed (direct GPIO, shift register, relay module)
- Starter firmware written (motor control class, serial test interface)
- Shopping list created (~$54 for Phase 1 hardware)

### What's Next

**Before buying any parts or writing more code:**
1. Use a multimeter to verify motor voltage (12V or 24V?)
2. Test home switch type (Normally Open or Normally Closed at home?)
3. Trace wires from motors to main connectors (verify pinout)
4. Measure motor winding resistance and current draw

See [PICKUP.md](PICKUP.md) for detailed next steps.

## Project Structure

```
├── PICKUP.md                              # ⭐ START HERE - Current state and next steps
├── README.md                              # This file
├── docs/
│   ├── PROJECT_PLAN.md                    # Full 14-task implementation plan
│   ├── phase1-hardware-analysis/
│   │   ├── README.md                      # Phase 1 overview and progress
│   │   ├── 01-initial-inspection.md       # Inspection checklist and findings
│   │   └── 03-motor-and-control-system.md # ⭐ Deep dive on motors and controls
│   ├── phase2-safety-testing/             # (empty - not started)
│   ├── phase3-motor-control/
│   │   └── esp32-controller-design.md     # ⭐ ESP32 design, circuits, firmware
│   └── phase4-integration/                # (empty - not started)
├── firmware/
│   ├── esp32/                             # (empty - code is in design doc for now)
│   └── arduino/                           # (empty - decided on ESP32)
└── hardware/
    ├── COMPONENT_INVENTORY.md             # Full parts list from photo analysis
    ├── WIRING_DIAGRAM.md                  # System wiring diagrams
    └── photos/initial-inspection/vending-pics/  # 23 JPGs of internals
```

## Key Documents

| Document | What's In It |
|----------|-------------|
| [PICKUP.md](PICKUP.md) | Current state, what's verified vs assumed, exact next steps |
| [Motor Deep Dive](docs/phase1-hardware-analysis/03-motor-and-control-system.md) | How vending motors work, cam/switch mechanism, 2-wire vs 3-wire, VMC architecture |
| [ESP32 Design](docs/phase3-motor-control/esp32-controller-design.md) | Pin assignments, driver circuits, power architecture, firmware code, shopping list |
| [Component Inventory](hardware/COMPONENT_INVENTORY.md) | Every component identified from photos |
| [Wiring Diagram](hardware/WIRING_DIAGRAM.md) | System diagrams and estimated pinouts |
| [Project Plan](docs/PROJECT_PLAN.md) | Full 14-task breakdown with success criteria |

## Safety

⚠️ **WARNING**: This project involves 120V AC input power and DC motors.
- Disconnect power before working on hardware
- Use current-limited power supplies during testing
- Verify voltages with multimeter before connecting anything
- Never trust estimated values - always measure first

## References

- [Seaga NTG4000 Owner's Manual](https://www.manualslib.com/manual/2979546/Seaga-Naturals2go-Ntg4000.html)
- [Seaga INF4S Service Manual](https://www.manualslib.com/manual/3865904/Seaga-Inf4s.html) (related model with wiring diagrams)
- [MDBee - ESP32 MDB Interface](https://github.com/IoTrio/MDBee)
- [Vending machine product page](https://www.vendingconcepts.com/product-page/naturals2go-n2g4000-c/)

## License

TBD
