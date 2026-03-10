# Vending Machine Photos

Photos taken during initial inspection and hardware analysis.

## Motor Assembly

### IMG_9507.JPG - Motor Label
- **Motor Model**: ZYT028T-001
- **Date Code**: S140915 (September 15, 2014)
- **Confirmed Specs**: 24V DC, 100-140Ω winding resistance

## PCB and Control Board

### IMG_9503.JPG - PCB Front
- 6-pin white connector
- Date code: 2010.06.23
- Motor driver transistor/MOSFET (black component)
- Yellow capacitor
- Microswitch for home position detection
- Labels: BCMK, CT, RoHS_004, SWITCH_004

### IMG_9505.JPG - PCB Back (Trace Side)
- Single-layer PCB confirmed
- Clear trace routing visible
- Ground and test point markings

### IMG_9891.JPG - Microswitch Detail
- **Rating**: 5A 125/250V AC
- **Type**: SPDT (3 terminals)
- **Manufacturer**: Huizhou
- **Model**: 10T85
- **Function**: Home position detection - **CAM ACTIVATED** (confirmed)
- **Mechanism**: Motor shaft has cam that presses switch actuator at home position

## Connector Pinout (Verified)

From back of PCB (trace side), left to right:
- **Pin 1**: Black (Motor Ground) - VERIFIED
- **Pin 2**: TBD (likely home switch)
- **Pin 3**: Red (Motor Power 24V) - VERIFIED
- **Pin 4**: TBD (likely home switch)
- **Pin 5**: TBD
- **Pin 6**: TBD

## Key Findings

1. **Motor Voltage**: 24V DC (confirmed via resistance measurement)
2. **Motor Type**: 2-wire power + separate home switch on PCB
3. **Home Switch**: Mechanical SPDT microswitch, not integrated in motor
4. **PCB Design**: Single-layer, simple motor driver circuit
