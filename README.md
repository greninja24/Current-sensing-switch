# Three-Phase Current Monitoring System for ESP32

![System Diagram](Additional Files/Circuit Diagram.jpg)

## Project Overview

This system provides three-phase current monitoring with protection features including:
- Overcurrent protection
- Phase imbalance detection
- Automatic load disconnection/reconnection
- Visual and serial interface for monitoring

## Hardware Requirements

### Main Components
- ESP32 development board
- 3× ACS712 current sensors (30A/20A variants)
- PC817 optocoupler for load control
- 16×2 I2C LCD display
- Breadboard and jumper wires
- Power supply for ESP32

### Connections
| ESP32 Pin | Connected To         | Notes                     |
|-----------|----------------------|---------------------------|
| 34        | ACS712 L1 output     | Phase 1 current sensing   |
| 35        | ACS712 L2 output     | Phase 2 current sensing   |
| 36        | ACS712 L3 output     | Phase 3 current sensing   |
| 4         | PC817 input          | Load control signal       |
| SDA       | LCD I2C SDA          | I2C data                  |
| SCL       | LCD I2C SCL          | I2C clock                 |
| 3.3V      | ACS712 VCC           | Sensor power              |
| GND       | ACS712 GND, PC817 GND| Common ground             |

## Software Requirements

### Arduino IDE Setup
1. Install [Arduino IDE](https://www.arduino.cc/en/software) (1.8.x or newer)
2. Add ESP32 support:
   - Add this URL to **File > Preferences > Additional Boards Manager URLs**:
     ```
     https://raw.githubusercontent.com/espressif/arduino-esp32/gh-pages/package_esp32_index.json
     ```
   - Install "ESP32" package from **Tools > Board > Boards Manager**
3. Select board: **ESP32 Dev Module**

### Required Libraries
Install these libraries via **Tools > Manage Libraries**:
- `LiquidCrystal_I2C` by Frank de Brabander (for LCD control)
- `Wire` (included with Arduino, for I2C communication)
