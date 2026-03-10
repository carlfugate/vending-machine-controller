# Motor Verification Test Results

**Date**: 2026-03-09  
**Test**: Initial motor power-on and operation verification

---

## Test Setup

- **Power Supply**: 24V DC
- **Current Limit**: 0.1A (100mA)
- **Motor**: ZYT028T-001 (24V DC, 100-140Ω)
- **Configuration**: Direct power to motor terminals

---

## Results

### ✅ Motor Operation - VERIFIED

**Applied Power:**
- Voltage: 24V DC
- Current Draw: ~0.1A (100mA)
- Connection: Red wire (+24V), Black wire (GND)

**Observed Behavior:**
- Motor rotates smoothly
- Cam mechanism actuates microswitch
- Home switch triggers at correct position
- Motor stops when power removed

### Key Findings

1. **Motor Voltage Confirmed**: 24V DC is correct operating voltage
2. **Current Draw**: ~100mA at 24V (well within expected range)
3. **Power Consumption**: ~2.4W per motor (24V × 0.1A)
4. **Home Switch Works**: Cam successfully actuates microswitch
5. **Mechanical Operation**: Smooth rotation, no binding

---

## Implications for Controller Design

### Power Requirements

**Single Motor:**
- Operating: 24V @ 0.1A = 2.4W
- Peak/Stall: Likely 2-3× higher (~0.3A)

**10 Motors (worst case all running):**
- Operating: 24V @ 1A = 24W
- Peak: 24V @ 3A = 72W (conservative estimate)

**Recommended Power Supply:**
- **24V 5A (120W)** - provides 2× safety margin

### Driver Requirements

**Per Motor:**
- Switching: 24V @ 0.3A peak
- Relay: 5A rated relay is massive overkill (good for reliability)
- MOSFET: Any logic-level MOSFET rated >1A would work

**Updated Shopping List:**
- ✅ 24V 5A power supply (confirmed)
- ✅ 16-channel 5V relay module (confirmed, will handle 24V loads easily)
- ✅ LM2596 buck converter 24V→5V for ESP32 (confirmed)

---

## Next Steps

1. ✅ **Motor voltage verified** - 24V DC confirmed
2. ✅ **Motor operation verified** - Rotates and triggers switch
3. ⏳ **Complete pinout mapping** - Trace remaining connector pins (2, 4, 6)
4. ⏳ **Test home switch logic** - Verify NO/NC state at home position
5. ⏳ **Measure stall current** - Determine peak current draw
6. ⏳ **Order parts** - 24V PSU, ESP32, relay module, buck converter
7. ⏳ **Build test circuit** - Single motor + ESP32 + relay
8. ⏳ **Write test firmware** - Basic motor control with home detection

---

## Safety Notes

- ✅ Current limiting to 0.1A prevented damage during initial test
- ✅ Motor operates smoothly at rated voltage
- ⚠️ Always use current-limited supply for initial testing
- ⚠️ Stall current likely 2-3× higher than running current

---

## Photos/Evidence

- Motor operation captured on video (if available)
- Current draw measurement: ~0.1A
- Voltage measurement: 24V DC
- Switch actuation confirmed visually

---

## Status: Phase 2 - Partially Complete

**Completed:**
- ✅ Motor resistance measurement (100-140Ω)
- ✅ Motor voltage verification (24V DC)
- ✅ Motor operation test (rotates smoothly)
- ✅ Home switch mechanical verification (cam actuates switch)
- ✅ Current draw measurement (~0.1A operating)

**Remaining:**
- ⏳ Complete connector pinout (pins 2, 4, 6)
- ⏳ Home switch electrical verification (NO/NC state)
- ⏳ Stall current measurement
- ⏳ All 10 motors tested individually
