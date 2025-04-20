
# Three-Phase Current Monitoring and Protection System

An ESP32-based Arduino project for real-time monitoring and protection of three-phase current systems using ACS712 current sensors. This system is designed to detect overcurrent and phase imbalance conditions, display real-time data on an I²C LCD, and control the load using an opto-isolated relay interface.

---

## Overview

This system continuously monitors current through three phases (L1, L2, L3), displays real-time readings, and implements protective measures such as:

- Overcurrent detection  
- Phase imbalance detection  
- Auto-restart with retry limits  
- Lockout after trip events  
- Sensor calibration and noise thresholding  

---

## Hardware Components

| Component             | Description                               |
|----------------------|-------------------------------------------|
| ESP32                | Main microcontroller (12-bit ADC used)    |
| ACS712 (30A x2, 20A) | Current sensors for L1, L2, L3             |
| PC817 Opto-isolator  | Load control interface (connected to pin 4) |
| I²C LCD (16x2)       | Display module (Address: 0x27)            |

---

## Features

### Auto-Calibration

- Zero-point calibration for each sensor
- Dynamic noise threshold calculation
- Sensor connection validation

### Real-Time Monitoring

- Current sensing on all three phases
- 10-sample moving average filtering
- Dynamic noise rejection

### Protection Logic

- Overcurrent detection (7.0 A)
- Phase imbalance detection (limit: 1.0 A)
- Auto-restart with max 3 attempts
- Lockout delay and restart timer

### User Interface

- I²C LCD shows live data or trip reason
- Serial monitor for advanced diagnostics
- Load status (ON/OFF) display

### Error Handling

- Detect missing or disconnected sensors
- Detect excessive noise during calibration
- Display persistent trip errors with system halt

---

## System Parameters

### Sensor Configuration

| Parameter                 | Value             |
|--------------------------|-------------------|
| Sensitivity (L1, L2)     | 0.066 V/A (30A)   |
| Sensitivity (L3)         | 0.100 V/A (20A)   |
| Calibration Samples      | 1000 per sensor   |
| Calibration Delay        | 5 ms per sample   |

### Protection Settings

| Condition                | Value             |
|--------------------------|-------------------|
| Overcurrent Trip         | 7.0 Amperes       |
| Restart Threshold        | 2.0 Amperes       |
| Phase Imbalance Limit    | 1.0 Ampere        |
| Min Current for Imbalance| 0.5 Amperes       |
| Lockout Time             | 5000 ms           |
| Trip Reset Time          | 60000 ms          |
| Max Auto-Restarts        | 3                 |

---

## System Logic Flow

### Initialization

1. Set up pins, I²C, serial, and LCD
2. Configure ADC (12-bit, 11dB attenuation)
3. Detect sensor presence
4. Perform zero-point calibration
5. Measure noise and set dynamic threshold
6. Validate calibration success
7. Prepare initial LCD screen

### Main Loop

- Measure current on all 3 phases
- Apply moving average filtering
- Detect faults (overcurrent, imbalance)
- Manage load control (ON/OFF)
- Update LCD and serial logs

### Protection Logic

#### Fault Detection

- Trip if any current > 7.0 A
- Calculate imbalance only if average > 0.5 A
- Trip if max deviation between phases > 1.0 A

#### Load State Management

- Cut power on trip
- Save cause of fault
- Attempt auto-restart (max 3 tries)
- Lockout delay on multiple faults

---

## Calibration Process

### Zero-Point Calibration

- Take 1000 samples, average to get zero-current voltage  
- Skip disconnected sensors  

### Noise Threshold Calibration

- Calculate deviation from zero point  
- Threshold = 150% of noise + 0.2 A margin  
- Fail if noise > 0.5 A  

---

## LCD and Serial Display

- Maintains a virtual buffer to reduce LCD flicker
- Updates display only on value changes
- Automatically toggles between normal and trip display
- Serial output mirrors LCD content

---

## Technical Implementation

### Current Measurement Formula

```cpp
Voltage = analogRead(ACS_PIN) * (3.3 / 4095.0);
Current = abs((Voltage - ZeroVoltage) / Sensitivity);
```

### Filtering

- 10-sample moving average
- Noise rejection with dynamic threshold

### LCD Handling

- Buffer system for optimized refresh
- Clear formatting and trip reason display

---

## Conclusion

This three-phase monitoring and protection system is ideal for:

- Industrial motor control systems
- Overload prevention in automation
- Energy monitoring in commercial setups

The system is self-calibrating, informative, and built for reliability in noisy environments. It ensures robust protection and gives you real-time insight into your power systems.
