
# ESP32 Advanced Current Sensor Calibration System

## Overview
This documentation covers an ESP32-based advanced calibration system for ACS712 current sensors. The system implements a sophisticated two-stage calibration process, noise analysis, and continuous zero-drift monitoring to ensure accurate current measurements in three-phase electrical systems.

## Hardware Components

- **ESP32 Microcontroller**: Main processing unit  
- **ACS712 Current Sensors**: Three sensors connected to pins 34, 35, and 36  
  - Two 30A sensors (L1, L2)  
  - One 20A sensor (L3)  

## Features

### Two-Stage Calibration Process
- Initial quick calibration for approximate zero-point  
- Secondary precision calibration for accurate zero-point determination  
- Sensor warm-up period to ensure stable readings  

### Advanced Noise Analysis
- Automatic noise level detection for each sensor  
- Dynamic threshold calculation with safety margins  
- Individual noise thresholds for each phase  

### Zero-Drift Monitoring
- Continuous monitoring of zero-point stability  
- Real-time drift detection and warnings  
- Periodic reporting of drift measurements  

### Optimized ESP32 ADC Configuration
- 12-bit resolution for increased precision  
- 11dB attenuation setting for improved signal-to-noise ratio  

### Diagnostic Output
- Detailed calibration progress reporting  
- Noise threshold analysis results  
- Real-time drift monitoring with warnings  

## System Parameters

### Hardware Configuration
- **ADC Pins**: 34, 35, 36 for L1, L2, L3 respectively  
- **Sensor Types**: 30A for L1/L2, 20A for L3  
- **Reference Voltage**: 3.3V  
- **ADC Resolution**: 12-bit (4095 steps)  

### Calibration Parameters
- **Initial Calibration**: 500 samples with 5ms delay  
- **Precision Calibration**: 2000 samples with 20ms delay  
- **Warm-up Period**: 100 discarded readings  
- **Noise Safety Margin**: 50% above maximum detected noise  

## System Logic Flow

### Initialization Sequence
1. Configure serial communication at 115,200 baud rate  
2. Set ESP32 ADC parameters for optimal performance  
3. Execute two-stage calibration process for each sensor  
4. Perform noise analysis and set dynamic thresholds  
5. Begin continuous zero-drift monitoring  

### Calibration Process

#### Stage 1 - Quick Calibration
- Collect 500 samples with 5ms intervals  
- Calculate initial zero-voltage reference point  

#### Stage 2 - Precision Calibration
- Collect 2000 samples with 20ms intervals  
- Calculate final zero-voltage reference point with higher accuracy  

#### Sample Collection Method
- Discard first 100 readings to allow sensor warm-up  
- Collect specified number of samples with configured delay  
- Convert ADC readings to voltage using reference voltage and resolution  
- Report progress during lengthy calibration processes  

### Noise Analysis Process
- Take 100 current measurements per phase  
- Track maximum deviation from zero  
- Set noise threshold at 150% of maximum detected noise  
- Store individual thresholds for each phase  

### Zero-Drift Monitoring
- Continuously measure current at zero-load conditions  
- Compare readings against established noise thresholds  
- Generate warnings when drift exceeds acceptable limits  
- Report measurements at 1-second intervals  

## Technical Implementation Details

### ADC Configuration
```cpp
analogReadResolution(12);
analogSetAttenuation(ADC_11db);
```

### Current Calculation
```cpp
float voltage = (analogRead(ACS712_PINS[phase]) * (VREF/ADC_RESOLUTION)) - zeroVoltages[phase];
return voltage / (SENSOR_TYPES[phase] == 20 ? 0.100 : 0.066);
```

### Sensor Sensitivity
- **30A Sensor**: 0.066 V/A  
- **20A Sensor**: 0.100 V/A  

## Applications
- **Industrial Power Monitoring**: Where accurate current measurement is critical  
- **Research and Development**: When precise calibration of sensors is needed  
- **Quality Control**: For validating sensor performance before deployment  
- **Long-term Monitoring Systems**: Where drift detection is important for maintenance  

## Extending the System
- Add temperature compensation for improved accuracy across varying conditions  
- Use EEPROM to store calibration values for persistence  
- Enable Wi-Fi reporting for calibration data and warnings  
- Integrate with alerting systems for critical drift detection  

## Conclusion
This advanced calibration system significantly improves the accuracy and reliability of ACS712-based current measurements on ESP32 platforms. By implementing a two-stage calibration process, noise analysis, and continuous monitoring, the system ensures optimal performance in applications requiring precise current measurement.

---

## Repository Structure

```
/main
│
├── sensor_calibration_code
│   └── [View Sensor Calibration Code](./main/sensor_calibration_code)
```

This folder contains the calibration system code used to enhance the performance of ACS712 current sensors.
