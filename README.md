# 🤖 Arduino Line Follower Robot

A simple two-sensor line follower robot built with Arduino. The robot uses infrared (IR) sensors to detect a black line on a white surface and drives its motors accordingly to stay on track.

---

## 📋 Table of Contents

- [How It Works](#how-it-works)
- [Hardware Requirements](#hardware-requirements)
- [Wiring / Pin Configuration](#wiring--pin-configuration)
- [Getting Started](#getting-started)
- [Logic Overview](#logic-overview)
- [PWM Frequency Note](#pwm-frequency-note)
- [License](#license)

---

## How It Works

The robot reads two IR sensors (left and right). Each sensor outputs `LOW` when it sees a white surface and `HIGH` when it detects a black line. Based on the combination of sensor readings, the robot:

- Drives **straight** when both sensors see white
- Turns **right** when only the right sensor detects the line
- Turns **left** when only the left sensor detects the line
- **Stops** when both sensors detect the line (e.g., at an intersection or end marker)

---

## Hardware Requirements

- Arduino Uno (or compatible board)
- 2× IR sensor modules
- L298N motor driver (or equivalent)
- 2× TT gear motors
- Chassis, wheels, and battery pack

---

## Wiring / Pin Configuration

### IR Sensors

| Sensor       | Arduino Pin |
|--------------|-------------|
| Right IR     | D11         |
| Left IR      | D12         |

### Right Motor

| Function          | Arduino Pin |
|-------------------|-------------|
| Enable (PWM)      | D6          |
| Motor Pin 1       | D7          |
| Motor Pin 2       | D8          |

### Left Motor

| Function          | Arduino Pin |
|-------------------|-------------|
| Enable (PWM)      | D5          |
| Motor Pin 1       | D9          |
| Motor Pin 2       | D10         |

---

## Getting Started

1. **Clone this repository**
   ```bash
   git clone https://github.com/cyril2007-richard/line_follow.git
   ```

2. **Open the sketch** in the [Arduino IDE](https://www.arduino.cc/en/software)

3. **Wire up** your components according to the pin configuration table above

4. **Upload** the sketch to your Arduino board

5. **Place the robot** on a track with a black line on a white surface and power it on

---

## Logic Overview

| Right Sensor | Left Sensor | Action       |
|--------------|-------------|--------------|
| LOW          | LOW         | Go straight  |
| HIGH         | LOW         | Turn right   |
| LOW          | HIGH        | Turn left    |
| HIGH         | HIGH        | Stop         |

Motor speed is set via the `MOTOR_SPEED` constant (default: `180`). Adjust this value to tune the robot's responsiveness.

---

## PWM Frequency Note

TT gear motors typically don't rotate at low PWM values but become hard to control at high ones. To work around this, the sketch modifies the Timer0 prescaler:

```cpp
TCCR0B = TCCR0B & B11111000 | B00000010;
```

This raises the PWM frequency on pins D5 and D6 to **7812.5 Hz**, allowing the motors to run smoothly at the configured speed.

> ⚠️ **Note:** This change also affects `millis()` and `delay()` timing on pins D5 and D6. If your project relies on accurate time functions, account for this accordingly.

---

images
[image](./image.png)
[schematics](./circuit.png)

## License

This project is licensed under the MIT License. See the [LICENSE](LICENSE) file for details.
# line_follow
