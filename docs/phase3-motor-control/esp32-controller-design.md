# ESP32 Controller Design - Thinking It Through

**Date**: 2026-03-02  
**Status**: Design Phase  
**Goal**: Control 10 vend motors + read 10 home switches + 1 door switch from an ESP32

---

## The Problem

We need to:
- Drive 10 DC motors (one at a time is fine)
- Read 10 home switch signals
- Read 1 door safety switch
- Eventually: serial display, buttons/keypad, maybe WiFi/BLE for CTF scenarios

That's **10 outputs + 11 inputs = 21 GPIO minimum**, plus whatever we add for UI.

---

## Approach 1: Direct GPIO (Simplest)

The ESP32 has enough pins to do this directly. No shift registers, no multiplexers.

### Safe ESP32 Pins

Not all ESP32 GPIOs are created equal. Here's what's actually usable:

```
SAFE OUTPUTS (can drive MOSFET gates):
  GPIO 2, 4, 5, 12, 13, 14, 15, 16, 17, 18, 19, 21, 22, 23, 25, 26, 27, 32, 33

SAFE INPUTS (can read switches):
  All of the above PLUS:
  GPIO 34, 35, 36, 39 (input-only, no internal pull-up)

AVOID:
  GPIO 0  - Boot mode (has pull-up, affects boot)
  GPIO 1  - TX0 (serial output, needed for programming/debug)
  GPIO 3  - RX0 (serial input, needed for programming/debug)
  GPIO 6-11 - Connected to internal flash (DO NOT USE)
```

### Pin Assignment

```
MOTOR OUTPUTS (10 pins) → MOSFET gates
  Motor 1:  GPIO 4
  Motor 2:  GPIO 5
  Motor 3:  GPIO 12
  Motor 4:  GPIO 13
  Motor 5:  GPIO 14
  Motor 6:  GPIO 15
  Motor 7:  GPIO 16
  Motor 8:  GPIO 17
  Motor 9:  GPIO 18
  Motor 10: GPIO 19

HOME SWITCH INPUTS (10 pins)
  Switch 1:  GPIO 21
  Switch 2:  GPIO 22
  Switch 3:  GPIO 23
  Switch 4:  GPIO 25
  Switch 5:  GPIO 26
  Switch 6:  GPIO 27
  Switch 7:  GPIO 32
  Switch 8:  GPIO 33
  Switch 9:  GPIO 34  (input-only, no pull-up → need external pull-up)
  Switch 10: GPIO 35  (input-only, no pull-up → need external pull-up)

DOOR SWITCH (1 pin)
  Door:      GPIO 36  (input-only)

STILL AVAILABLE for UI:
  GPIO 2   - Onboard LED / status
  GPIO 39  - Extra input
  TX0/RX0  - Serial debug (always available)
  I2C: GPIO 21/22 are taken → use software I2C on remaining pins
         OR reassign motors to free up 21/22 for I2C display
```

### Pros/Cons

✅ Simple wiring - one wire per function  
✅ No extra ICs needed  
✅ Easy to debug - each pin maps to one thing  
✅ Fast response - direct GPIO read/write  
❌ Uses most available pins - limited room for expansion  
❌ GPIO 34/35 need external pull-up resistors  

---

## Approach 2: Shift Register for Motor Outputs (More Expandable)

Use a 74HC595 shift register to drive the 10 motor MOSFETs. This frees up GPIO pins for other things.

### How It Works

```
ESP32 (3 pins)  →  74HC595 × 2 (daisy-chained)  →  16 outputs (use 10)
  GPIO 18: DATA   (serial data)
  GPIO 19: CLOCK  (shift clock)
  GPIO 5:  LATCH  (storage register clock)
```

But there's a catch: **shift registers can't do PWM**, and there's a small latency when shifting data out. For motor on/off control this is fine - we don't need PWM for vend motors.

### Pin Assignment (Shift Register Approach)

```
SHIFT REGISTER CONTROL (3 pins)
  DATA:   GPIO 18
  CLOCK:  GPIO 19
  LATCH:  GPIO 5

HOME SWITCH INPUTS (10 pins) - still direct GPIO
  Switch 1:  GPIO 4
  Switch 2:  GPIO 12
  Switch 3:  GPIO 13
  Switch 4:  GPIO 14
  Switch 5:  GPIO 15
  Switch 6:  GPIO 16
  Switch 7:  GPIO 17
  Switch 8:  GPIO 25
  Switch 9:  GPIO 26
  Switch 10: GPIO 27

DOOR SWITCH (1 pin)
  Door:      GPIO 34

AVAILABLE for UI (many pins free!):
  GPIO 2, 21, 22, 23, 32, 33, 35, 36, 39
  That's 9 pins for display, keypad, LEDs, serial, etc.
```

### Pros/Cons

✅ Only 3 pins for all 10 motor outputs  
✅ Lots of free pins for UI, display, keypad  
✅ Easily expandable (chain more 595s)  
✅ 74HC595 costs ~$0.50  
❌ Slightly more complex wiring  
❌ Can't drive MOSFETs directly from 595 (need buffer/driver)  
❌ Small latency (~microseconds, irrelevant for motors)  

### Important: 74HC595 Output Current

The 74HC595 can only source/sink ~35mA per pin. That's enough to drive a logic-level MOSFET gate, but NOT enough to drive a motor directly. The signal chain is:

```
74HC595 output (3.3V/5V, 35mA) → MOSFET gate → MOSFET switches 12V to motor
```

This works fine. The MOSFET gate is essentially a capacitor - it takes very little current to hold it on.

---

## Approach 3: Relay Module (Simplest Hardware, Best Braking)

Use a 16-channel relay module. This solves the motor braking problem automatically.

### Why Relays Solve Braking

When a relay opens, the motor terminals are disconnected. But relay contacts have a property: when they switch from closed to open, the motor's back-EMF creates a brief short through the relay's normally-closed contact, providing dynamic braking.

More importantly, some relay modules have both NO and NC contacts. You can wire it so:
- Relay ON → Motor connected to +12V (runs)
- Relay OFF → Motor terminals shorted together (brakes)

### Pin Assignment (Relay Approach)

Same as Approach 1 (direct GPIO) or Approach 2 (shift register), just replace MOSFETs with relay module inputs.

### Pros/Cons

✅ Built-in motor braking  
✅ Complete electrical isolation  
✅ Simple to wire  
✅ 16-channel relay modules are cheap (~$8-15)  
❌ Mechanical wear (relays have limited cycle life ~100K cycles)  
❌ Slower switching (~5-10ms vs microseconds for MOSFET)  
❌ Audible clicking  
❌ Larger physical size  
❌ Higher power consumption (relay coils draw ~70mA each)  

---

## My Recommendation: Start with Approach 1 + Relays, Evolve to MOSFETs

### Phase 1: Get It Working (Relays)

```
ESP32 DevKit → 16-Channel Relay Module → Motors

Why:
- Fastest path to a working system
- Relay modules are plug-and-play with ESP32
- Built-in motor braking (no overshoot)
- Easy to debug (you can hear/see relays click)
- If a relay fries, swap the module
```

### Phase 2: Optimize (Custom MOSFET Board)

Once you understand the motors' behavior (current draw, braking needs, timing), design a proper MOSFET driver board with:
- Logic-level N-channel MOSFETs (IRLZ44N)
- Flyback diodes
- Optional braking circuit (P-channel high-side + N-channel low-side)
- Current sensing (ACS712 or shunt resistor)

---

## The Motor Driver Circuit (Both Approaches)

### Relay Version (Phase 1)

```
                    +12V Power Supply
                         │
                    ┌────┴────────────────────────────┐
                    │                                  │
               Motor 1                           Motor 10
               Red Wire                          Red Wire
                    │                                  │
              ┌─────┴─────┐                     ┌─────┴─────┐
              │  Relay 1  │                     │ Relay 10  │
              │  (NO)     │                     │  (NO)     │
              └─────┬─────┘                     └─────┬─────┘
                    │                                  │
               Motor 1                           Motor 10
               Black Wire                        Black Wire
                    │                                  │
                    └────┬────────────────────────────┘
                         │
                        GND

Relay Module:
  IN1  ← ESP32 GPIO 4   (Motor 1)
  IN2  ← ESP32 GPIO 5   (Motor 2)
  ...
  IN10 ← ESP32 GPIO 19  (Motor 10)
  VCC  ← 5V (from ESP32 VIN or separate supply)
  GND  ← Common GND
```

### MOSFET Version (Phase 2)

```
For each motor:

                    +12V
                     │
                Motor (Red)
                     │
                Motor (Black)
                     │
                ┌────┴────┐
           ┌───►│ 1N4007  │◄───┐    Flyback diode
           │    └─────────┘    │    (cathode to +12V)
           │                   │
           │    ┌─────────┐    │
           └────┤  Motor  ├────┘
                └────┬────┘
                     │
                     D
                ┌────┤
                │ IRLZ44N
    ESP32 ──── G    │
    GPIO       │    S
    (via 100Ω) └────┤
                     │
                    GND

Gate resistor: 100Ω (limits inrush to gate capacitance)
Gate pull-down: 10kΩ to GND (ensures motor off when ESP32 boots)
```

---

## Home Switch Input Circuit

The switch signal voltage depends on how the switches are wired internally. Two scenarios:

### Scenario A: Switch Pulls to Ground (Most Likely)

The switch connects the signal wire to ground when actuated. Use ESP32 internal pull-up:

```
    +3.3V (internal pull-up)
      │
      ├──── ESP32 GPIO (INPUT_PULLUP)
      │
    Switch
      │
     GND (inside motor assembly)

Reading:
  Switch NOT pressed (home) → GPIO reads HIGH (3.3V)
  Switch PRESSED (rotating) → GPIO reads LOW (0V)
```

### Scenario B: Switch Outputs 12V Signal

If the switch signal is referenced to 12V, need a voltage divider:

```
    Switch Signal (0-12V)
      │
     22kΩ
      │
      ├──── ESP32 GPIO
      │
     10kΩ
      │
     GND

    12V × 10k/(22k+10k) = 3.75V → safe for ESP32 (abs max 3.6V)
    Better: use 33kΩ + 10kΩ → 2.79V (safer margin)
    
    Or just use an optocoupler for full isolation.
```

### Scenario C: Use Optocouplers (Safest)

Complete electrical isolation between motor circuits and ESP32:

```
    Switch Signal ──── 1kΩ ──── Optocoupler LED (+)
                                Optocoupler LED (-) ──── GND (motor side)

    +3.3V ──── 10kΩ ──── Optocoupler Collector ──── ESP32 GPIO
                          Optocoupler Emitter ──────── GND (ESP32 side)
```

**I'd recommend optocouplers** for the first build. They protect the ESP32 from any voltage surprises while you're still figuring out the exact electrical characteristics.

---

## Power Architecture

```
┌─────────────────────────────────────────────────────────┐
│                                                          │
│   Wall Outlet (120V AC)                                 │
│        │                                                 │
│   ┌────┴────┐                                           │
│   │ 12V 10A │    ← Switching power supply               │
│   │  PSU    │      (120W gives plenty of headroom)      │
│   └────┬────┘                                           │
│        │                                                 │
│   +12V Rail ──┬──── To Relay Module / MOSFETs ──► Motors│
│               │                                          │
│               │    ┌──────────┐                          │
│               └────┤ Buck     │                          │
│                    │ Converter│  ← LM2596 module ($2)    │
│                    │ 12V→5V  │                          │
│                    └────┬────┘                           │
│                         │                                │
│                    5V Rail ──┬──── ESP32 (via VIN pin)   │
│                              └──── Relay Module VCC      │
│                                                          │
│   Common GND ──── All grounds tied together              │
│                                                          │
└─────────────────────────────────────────────────────────┘

Current Budget:
  10 motors × 500mA (one at a time)  =  0.5A @ 12V
  Relay coils (if used) × 10 × 70mA  =  0.7A @ 5V
  ESP32                               =  0.2A @ 5V
  Headroom                            =  plenty
  
  12V supply: 10A is overkill for one motor at a time,
              but gives margin for stall current and future expansion.
              A 5A supply would work fine for initial testing.
```

---

## Software Architecture

### Core State Machine

```
┌──────────┐     Button      ┌──────────────┐
│          │    Pressed       │              │
│   IDLE   ├─────────────────►  VALIDATING  │
│          │                  │              │
└────▲─────┘                  └──────┬───────┘
     │                               │
     │                          Door OK?
     │                          Motor OK?
     │                               │
     │         ┌─────────┐     ┌─────▼──────┐
     │    Fail │         │     │            │
     ├─────────┤  ERROR  ◄─────┤  VENDING   │
     │         │         │     │            │
     │         └─────────┘     └─────┬──────┘
     │                               │
     │                          Home switch
     │                          detected
     │                               │
     │                         ┌─────▼──────┐
     │                         │            │
     └─────────────────────────┤  COMPLETE  │
                               │            │
                               └────────────┘
```

### Minimal Firmware Structure

```
firmware/esp32/
├── platformio.ini          # PlatformIO project config
├── src/
│   ├── main.cpp            # Setup and main loop
│   ├── motor.h             # Motor control class
│   ├── motor.cpp
│   ├── config.h            # Pin assignments and constants
│   └── safety.h            # Safety checks (door, timeout)
```

### config.h (Pin Definitions)

```cpp
#pragma once

// Motor output pins (directly to relay module or MOSFET gates)
constexpr int MOTOR_PINS[] = {4, 5, 12, 13, 14, 15, 16, 17, 18, 19};
constexpr int NUM_MOTORS = 10;

// Home switch input pins
constexpr int SWITCH_PINS[] = {21, 22, 23, 25, 26, 27, 32, 33, 34, 35};

// Door safety switch
constexpr int DOOR_PIN = 36;

// Timing
constexpr unsigned long MOTOR_TIMEOUT_MS = 8000;  // Max motor run time
constexpr unsigned long DEBOUNCE_MS = 20;          // Switch debounce
constexpr unsigned long VEND_COOLDOWN_MS = 500;    // Delay between vends
```

### motor.h (Motor Control)

```cpp
#pragma once
#include <Arduino.h>
#include "config.h"

enum class VendResult {
    SUCCESS,
    DOOR_OPEN,
    TIMEOUT,
    INVALID_MOTOR
};

void motors_init();
VendResult vend(int motorIndex);
bool is_door_closed();
bool read_home_switch(int motorIndex);
```

### motor.cpp (Core Logic)

```cpp
#include "motor.h"

void motors_init() {
    for (int i = 0; i < NUM_MOTORS; i++) {
        pinMode(MOTOR_PINS[i], OUTPUT);
        digitalWrite(MOTOR_PINS[i], LOW);
        // Use INPUT_PULLUP for pins that support it
        if (SWITCH_PINS[i] < 34) {
            pinMode(SWITCH_PINS[i], INPUT_PULLUP);
        } else {
            pinMode(SWITCH_PINS[i], INPUT); // External pull-up needed
        }
    }
    pinMode(DOOR_PIN, INPUT);
}

bool is_door_closed() {
    return digitalRead(DOOR_PIN) == LOW; // Adjust based on switch wiring
}

bool read_home_switch(int idx) {
    return digitalRead(SWITCH_PINS[idx]);
}

VendResult vend(int idx) {
    if (idx < 0 || idx >= NUM_MOTORS) return VendResult::INVALID_MOTOR;
    if (!is_door_closed()) return VendResult::DOOR_OPEN;

    bool homeState = read_home_switch(idx);

    // Start motor
    digitalWrite(MOTOR_PINS[idx], HIGH);

    // Wait for switch to change (motor left home)
    unsigned long start = millis();
    while (read_home_switch(idx) == homeState) {
        if (millis() - start > MOTOR_TIMEOUT_MS) {
            digitalWrite(MOTOR_PINS[idx], LOW);
            return VendResult::TIMEOUT;
        }
        if (!is_door_closed()) {
            digitalWrite(MOTOR_PINS[idx], LOW);
            return VendResult::DOOR_OPEN;
        }
    }

    // Wait for switch to return (motor back at home)
    start = millis();
    while (read_home_switch(idx) != homeState) {
        if (millis() - start > MOTOR_TIMEOUT_MS) {
            digitalWrite(MOTOR_PINS[idx], LOW);
            return VendResult::TIMEOUT;
        }
    }

    // Stop motor
    digitalWrite(MOTOR_PINS[idx], LOW);
    delay(VEND_COOLDOWN_MS);
    return VendResult::SUCCESS;
}
```

### main.cpp (Serial Control for Testing)

```cpp
#include <Arduino.h>
#include "motor.h"

void setup() {
    Serial.begin(115200);
    motors_init();
    Serial.println("Vending Controller Ready");
    Serial.println("Send motor number (0-9) to vend");
}

void loop() {
    if (Serial.available()) {
        char c = Serial.read();
        if (c >= '0' && c <= '9') {
            int motor = c - '0';
            Serial.printf("Vending motor %d...\n", motor);

            VendResult result = vend(motor);

            switch (result) {
                case VendResult::SUCCESS:
                    Serial.println("SUCCESS"); break;
                case VendResult::DOOR_OPEN:
                    Serial.println("ERROR: Door open"); break;
                case VendResult::TIMEOUT:
                    Serial.println("ERROR: Motor timeout"); break;
                case VendResult::INVALID_MOTOR:
                    Serial.println("ERROR: Invalid motor"); break;
            }
        }
    }
}
```

---

## Shopping List (Phase 1 - Relay Approach)

| Item | Qty | Est. Cost | Notes |
|------|-----|-----------|-------|
| ESP32 DevKit V1 (30-pin) | 1 | $8 | Any ESP32 dev board works |
| 16-Channel Relay Module (5V) | 1 | $12 | Overkill but gives spares |
| 12V 5A Power Supply | 1 | $12 | Barrel jack, switching type |
| LM2596 Buck Converter (12V→5V) | 1 | $3 | Powers ESP32 + relay module |
| Breadboard + Jumper Wires | 1 | $8 | For prototyping |
| Multimeter | 1 | (have?) | Essential for verification |
| 10kΩ Resistors (pack) | 1 | $3 | Pull-ups for GPIO 34/35 |
| PC817 Optocouplers (pack of 10) | 1 | $5 | If switch signals are 12V |
| 1kΩ Resistors (pack) | 1 | $3 | For optocoupler LEDs |
| **Total** | | **~$54** | |

### Optional but Recommended

| Item | Qty | Est. Cost | Notes |
|------|-----|-----------|-------|
| USB Logic Analyzer | 1 | $10 | Debug switch signals |
| ACS712 Current Sensor | 1 | $4 | Measure motor current |
| OLED Display (SSD1306 I2C) | 1 | $5 | Status display |
| 4x4 Matrix Keypad | 1 | $4 | Product selection |

---

## Testing Plan

### Step 1: Verify Motor Specs (No ESP32 Yet)
- Measure motor winding resistance with multimeter
- Identify switch wiring with continuity test
- Apply 12V from bench supply to ONE motor through a 1A fuse
- Measure actual current draw
- Observe home switch behavior with multimeter

### Step 2: Single Motor Test
- Wire ONE motor through ONE relay
- Connect ONE home switch to ESP32
- Run the serial test firmware
- Verify motor starts, rotates, and stops at home
- Measure timing (how long is one rotation?)

### Step 3: Scale Up
- Add remaining 9 motors one at a time
- Verify each motor works independently
- Test rapid sequential vends
- Test error conditions (blocked motor, open door)

### Step 4: Add UI
- Connect OLED display
- Add keypad or buttons
- Implement product selection workflow

---

## Future Expansion Ideas (CTF/Conference Mode)

Once basic vending works, the ESP32's WiFi/BLE opens up possibilities:

- **Web interface**: Control vending from a phone
- **CTF challenges**: Solve a puzzle → get a vend token via API
- **BLE beacon**: Advertise challenges to nearby phones
- **MQTT integration**: Remote monitoring and control
- **Custom display**: Show CTF scores, challenges, or messages
- **API endpoint**: REST API for programmatic vending
- **NFC/RFID**: Badge-based vending at conferences

The ESP32 is massively overpowered for just running 10 motors - that's the point. The extra capability is what makes this a hackable platform.
