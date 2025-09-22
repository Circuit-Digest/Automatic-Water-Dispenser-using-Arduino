# Arduino Automatic Water Dispenser

![Arduino Automatic Water Dispenser](https://circuitdigest.com/sites/default/files/projectimage_mic/Automatic-Water-Dispenser-using-Arduino.jpg)

## 📋 Table of Contents
- [Overview](#overview)
- [Why Build This Project?](#why-build-this-project)
- [Features](#features)
- [Components Required](#components-required)
- [Circuit Diagram](#circuit-diagram)
- [How It Works](#how-it-works)
- [Pin Configuration](#pin-configuration)
- [Installation & Setup](#installation--setup)
- [Code](#code)
- [Troubleshooting](#troubleshooting)
- [Project Enhancements](#project-enhancements)
- [FAQ](#faq)
- [Contributing](#contributing)
- [License](#license)

## 🌟 Overview

This project demonstrates how to build an **Automatic Water Dispenser** using Arduino and an ultrasonic sensor. The system automatically dispenses water when a glass is detected and stops when the glass is removed, promoting water conservation and hygiene.

**Original Tutorial:** [Arduino Automatic Water Dispenser - Circuit Digest](https://circuitdigest.com/microcontroller-projects/arduino-automatic-water-dispenser)
**Projects:**[Arduino Projects](https://circuitdigest.com/arduino-projects)


## 🌍 Why Build This Project?

With only 2.5% of Earth's water being drinkable and increasing water scarcity concerns, smart water conservation technology is crucial. This automatic dispenser:

- **Saves Water**: Prevents wastage from manual taps left running
- **Promotes Hygiene**: Touch-free operation reduces contamination
- **Smart Technology**: Demonstrates IoT principles in everyday applications
- **Educational Value**: Perfect for learning sensor interfacing and automation

> *A dripping tap wastes 1 gallon of water every 5 hours - enough for a person to survive 2 days!*

## ✨ Features

- **Automatic Detection**: Uses HC-SR04 ultrasonic sensor for object detection
- **Touch-Free Operation**: Completely hands-free water dispensing
- **Water Conservation**: Automatically stops flow when glass is removed
- **Visual Feedback**: LED indicator shows system status
- **Simple Circuit**: Easy to build on breadboard
- **Customizable Range**: Adjustable detection distance (default: 10cm)

## 🛠️ Components Required

| Component | Quantity | Purpose |
|-----------|----------|---------|
| Arduino Uno | 1 | Main microcontroller |
| [**HC-SR04 Ultrasonic Sensor**](https://circuitdigest.com/microcontroller-projects/interfacing-ultrasonic-sensor-hc-sr04-with-pic16f877a) | 1 | Object detection |
| 12V Solenoid Valve | 1 | Water flow control |
| IRF540N MOSFET | 1 | Switching circuit |
| 1kΩ Resistor | 1 | Current limiting |
| 10kΩ Resistor | 1 | Pull-down resistor |
| Breadboard | 1 | Circuit assembly |
| Connecting Wires | Several | Connections |
| 12V DC Adapter | 1 | Power supply |

## 🔌 Circuit Diagram

### Connection Details

| Arduino Pin | Component | Component Pin | Notes |
|-------------|-----------|---------------|--------|
| Digital Pin 8 | HC-SR04 | Echo | Distance measurement input |
| Digital Pin 9 | HC-SR04 | Trigger | Ultrasonic signal output |
| Digital Pin 12 | MOSFET | Gate (via 1kΩ) | Solenoid control |
| Digital Pin 13 | Built-in LED | Onboard LED | Status indicator |
| 5V | HC-SR04 | VCC | Sensor power |
| VIN | Solenoid Valve | Positive | 12V power from adapter |
| GND | All components | Ground | Common ground |

### Circuit Description

The circuit uses an IRF540N MOSFET as a switching element to control the 12V solenoid valve. The HC-SR04 sensor continuously monitors for objects within a 10cm range. When detected, Arduino triggers the MOSFET to open the solenoid valve, allowing water flow.

## ⚙️ How It Works

### Operation Phases

1. **Detection Phase**: HC-SR04 sensor continuously monitors for objects within 10cm range
2. **Activation Phase**: When glass is detected, Arduino sends HIGH signal to MOSFET gate
3. **Flow Phase**: MOSFET turns ON, powering the 12V solenoid valve to allow water flow
4. **Deactivation Phase**: When glass is removed, MOSFET turns OFF, stopping water flow

### Technical Principle

The system uses ultrasonic waves to measure distance. The sensor sends sound waves that bounce off objects and return. By calculating the time taken, the distance is determined using the formula:

```
Distance = (Time × Speed of Sound) / 2
Distance = (Time × 340 m/s) / 20000 (for cm conversion)
```

## 💻 Installation & Setup

### Hardware Setup

1. **Assemble Circuit**: Connect components according to the pin configuration table
2. **Power Connection**: Use 12V DC adapter connected to Arduino's DC jack
3. **Sensor Placement**: Position HC-SR04 sensor below the solenoid valve
4. **Water Setup**: Connect solenoid valve to water inlet and outlet

### Software Setup

1. **Install Arduino IDE**: Download from [arduino.cc](https://arduino.cc)
2. **Connect Arduino**: Use USB cable to connect to computer
3. **Upload Code**: Copy the provided code and upload to Arduino
4. **Serial Monitor**: Open to view distance readings (optional)

## 📝 Code

```cpp
#define trigger 9
#define echo 8
#define LED 13
#define MOSFET 12

float time = 0, distance = 0;

void setup() {
    Serial.begin(9600);
    pinMode(trigger, OUTPUT);
    pinMode(echo, INPUT);
    pinMode(LED, OUTPUT);
    pinMode(MOSFET, OUTPUT);
    delay(2000);
}

void loop() {
    measure_distance();
    
    if(distance < 10) {
        digitalWrite(LED, HIGH);
        digitalWrite(MOSFET, HIGH);
    } else {
        digitalWrite(LED, LOW);
        digitalWrite(MOSFET, LOW);
    }
    
    delay(500);
}

void measure_distance() {
    digitalWrite(trigger, LOW);
    delayMicroseconds(2);
    digitalWrite(trigger, HIGH);
    delayMicroseconds(10);
    digitalWrite(trigger, LOW);
    delayMicroseconds(2);
    
    time = pulseIn(echo, HIGH);
    distance = time * 340 / 20000;
}
```

### Code Explanation

- **Pin Definitions**: Defines pins for trigger, echo, LED, and MOSFET
- **Setup Function**: Initializes pin modes and serial communication
- **Main Loop**: Continuously measures distance and controls solenoid
- **Distance Function**: Implements ultrasonic distance measurement algorithm

## 🔧 Troubleshooting

| Problem | Possible Cause | Solution |
|---------|---------------|----------|
| Solenoid not working | Insufficient power supply | Check 12V adapter, verify VIN voltage |
| Sensor not detecting | Wrong pin connections | Verify trigger (pin 9), echo (pin 8) |
| Continuous water flow | MOSFET stuck ON | Check pull-down resistor, MOSFET connections |
| No response to objects | Code upload issues | Re-upload code, check serial monitor |
| Erratic behavior | Loose connections | Check breadboard connections, use solid wires |

### Important Notes

⚠️ **Warning**: Ensure your solenoid valve operates on 12V and consumes maximum 1.5A

⚠️ **Safety**: Use food-grade solenoid valve for drinking water applications

## 🚀 Project Enhancements

### Possible Improvements

- **Timer Cut-off**: Add maximum dispensing time to prevent overflow
- **Volume Control**: Implement different dispensing volumes based on glass size
- **LCD Display**: Show status messages and water volume dispensed
- **WiFi Connectivity**: Enable remote monitoring and smartphone control
- **Water Level Monitoring**: Alert when reservoir is low
- **Temperature Sensor**: Display water temperature
- **Flow Rate Control**: Adjust water flow speed

### Advanced Features

```cpp
// Example: Timer-based cut-off (5 seconds max)
unsigned long startTime = 0;
const unsigned long maxDispenseTime = 5000; // 5 seconds

if(distance < 10) {
    if(startTime == 0) startTime = millis();
    if(millis() - startTime < maxDispenseTime) {
        digitalWrite(MOSFET, HIGH);
    } else {
        digitalWrite(MOSFET, LOW);
    }
} else {
    startTime = 0;
    digitalWrite(MOSFET, LOW);
}
```

## ❓ FAQ

### General Questions

**Q: Why use a MOSFET instead of direct Arduino control?**
A: Arduino outputs 5V/40mA max, insufficient for 12V solenoid requiring 700mA-1.2A. MOSFET acts as an electronic switch.

**Q: How to adjust detection range?**
A: Modify the threshold value in code: `if(distance < YOUR_VALUE)`. Recommended range: 2-15cm.

**Q: What power supply is needed?**
A: 12V DC adapter with minimum 1.5A capacity. Connects to Arduino's DC jack.

### Technical Questions

**Q: Can I use different solenoid specifications?**
A: Yes, but ensure voltage compatibility. For 24V solenoids, use 24V adapter and check MOSFET ratings.

**Q: How to prevent overflow?**
A: Implement timer cut-off using `millis()` function or add second sensor for "glass full" detection.

**Q: Why isn't my sensor detecting objects?**
A: Check wiring (trigger→pin9, echo→pin8), sensor orientation, and use Serial Monitor for debugging.

## 🤝 Contributing

We welcome contributions! Here's how you can help:

1. **Fork the Repository**
2. **Create Feature Branch**: `git checkout -b feature/amazing-feature`
3. **Commit Changes**: `git commit -m 'Add amazing feature'`
4. **Push to Branch**: `git push origin feature/amazing-feature`
5. **Open Pull Request**

### Contribution Ideas

- Add new sensor types (PIR, IR, etc.)
- Implement wireless connectivity
- Create mobile app interface
- Add water quality monitoring
- Improve power efficiency

## 📜 License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

## 🔗 Related Projects

Explore more HC-SR04 sensor applications:

- [ESP32 Ultrasonic Distance Sensor](https://circuitdigest.com/microcontroller-projects/interfacing-hcsr04-ultrasonic-sensor-module-with-esp32)
- [Raspberry Pi Distance Measurement](https://circuitdigest.com/microcontroller-projects/raspberry-pi-distance-measurement-using-ultrasonic-sensor)
- [Arduino Distance Measurement](https://circuitdigest.com/microcontroller-projects/arduino-ultrasonic-sensor-based-distance-measurement)

## 📞 Support

- **Original Tutorial**: [Circuit Digest - Arduino Automatic Water Dispenser](https://circuitdigest.com/microcontroller-projects/arduino-automatic-water-dispenser)
- **Issues**: Report problems in the Issues section
- **Discussions**: Join community discussions for help and ideas

---

**Built with ❤️ for water conservation and automation learning**

*Remember: Every drop counts! 💧*
