# ESP32-Voice-Assistant
An ESP32-based AI voice assistant project using an INMP441 digital microphone, MAX98357A audio amplifier, speaker, and OLED display.  The objective is to build a voice-interactive chatbot capable of capturing audio, processing voice input, generating audio responses, and displaying information on an OLED screen.
# 🤖 ESP32 AI Voice Assistant

An ESP32-based AI voice assistant project using an INMP441 digital microphone, MAX98357A audio amplifier, speaker, and OLED display.

The objective is to build a voice-interactive chatbot capable of capturing audio, processing voice input, generating audio responses, and displaying information on an OLED screen.

## 📸 Hardware Wiring Diagram



## 🔧 Hardware Components

| Component       | Description                              |    Quantity |
| --------------- | ---------------------------------------- | ----------: |
| ESP32 DevKit V1 | ESP32-WROOM-32, 30-pin development board |           1 |
| INMP441         | I2S digital microphone                   |           1 |
| MAX98357A       | I2S audio amplifier                      |           1 |
| Speaker         | 4Ω, 3W speaker                           |           1 |
| OLED Display    | 0.96-inch, 128×64 I2C display            |           1 |
| LED             | Status indicator LED                     |           1 |
| Resistor        | 220Ω                                     |           1 |
| Jumper Wires    | Male-to-female / male-to-male            | As required |

## 🔌 GPIO Pin Configuration

The following table represents the proposed connections shown in the wiring diagram.

### 1. INMP441 – Digital Microphone

The INMP441 captures voice input and sends digital audio data to the ESP32 through the I2S interface.

| INMP441 Pin | ESP32 Pin | Connection             |
| ----------- | --------- | ---------------------- |
| VDD         | 3V3       | Power                  |
| GND         | GND       | Ground                 |
| SD          | GPIO 32   | Audio Data             |
| WS          | GPIO 25   | Word Select            |
| SCK         | GPIO 26   | Serial Clock           |
| L/R         | GND       | Left Channel Selection |

**Important:** The microphone must be powered using 3.3V, not 5V.

### 2. MAX98357A – Audio Amplifier

The MAX98357A receives digital audio from the ESP32 and amplifies it to drive the speaker.

| MAX98357A Pin | ESP32 Pin | Connection       |
| ------------- | --------- | ---------------- |
| VIN           | 3V3       | Power            |
| GND           | GND       | Ground           |
| SD            | 3V3       | Amplifier Enable |
| DIN           | GPIO 33   | Audio Data       |
| BCLK          | GPIO 14   | Bit Clock        |
| LRC           | GPIO 27   | Word Select      |

**Speaker connections:**

- Speaker (+) → MAX98357A SPK+
- Speaker (−) → MAX98357A SPK−

Do not connect either speaker terminal directly to ESP32 GND.

### 3. OLED Display – 0.96 Inch

The OLED uses I2C communication to display chatbot status and information.

| OLED Pin | ESP32 Pin | Connection |
| -------- | --------- | ---------- |
| GND      | GND       | Ground     |
| VCC      | 3V3       | Power      |
| SCL      | GPIO 15   | I2C Clock  |
| SDA      | GPIO 4    | I2C Data   |

Display specifications:

- Display size: 0.96 inch
- Resolution: 128 × 64 pixels
- Interface: I2C

### 4. LED – Status Indicator

| LED Connection         | ESP32 Connection              |
| ---------------------- | ----------------------------- |
| Anode (+), long leg    | GPIO 18 through 220Ω resistor |
| Cathode (−), short leg | GND                           |

The resistor limits current and protects the LED.

---

## ⚫ Ground Connections – First Connection Stage

The first stage of hardware assembly is connecting all the black wires.

All modules must share a common ground with the ESP32.

| Component | Ground Pin        | Connect To     |
| --------- | ----------------- | -------------- |
| INMP441   | GND               | ESP32 GND      |
| MAX98357A | GND               | ESP32 GND      |
| OLED      | GND               | ESP32 GND      |
| LED       | Cathode (−)       | ESP32 GND      |
| Speaker   | No GND connection | MAX98357A SPK− |

### Ground Connection Procedure

1. Identify the GND pin on the ESP32.
2. Connect INMP441 GND to ESP32 GND.
3. Connect MAX98357A GND to ESP32 GND.
4. Connect OLED GND to ESP32 GND.
5. Connect the LED cathode to ESP32 GND.
6. Verify all ground connections.

A breadboard's common negative rail can be used to distribute GND to all modules.

**Safety:** Do not power the circuit until the connections have been checked.

## 🔴 Power Connections – Second Connection Stage

After completing and checking the ground connections, connect the power wires.

| Component | Power Pin | ESP32 Connection                      |
| --------- | --------- | ------------------------------------- |
| INMP441   | VDD       | 3V3                                   |
| MAX98357A | VIN       | 3V3 (subject to current requirements) |
| OLED      | VCC       | 3V3                                   |
| MAX98357A | SD        | 3V3                                   |

Check the module specifications and power requirements before powering the complete circuit.

## 🟡 Signal Connections – Third Connection Stage

Connect the communication and control wires according to the GPIO configuration.

| Signal         | GPIO |
| -------------- | ---: |
| INMP441 SD     |   32 |
| INMP441 WS     |   25 |
| INMP441 SCK    |   26 |
| MAX98357A DIN  |   33 |
| MAX98357A BCLK |   14 |
| MAX98357A LRC  |   27 |
| OLED SCL       |   15 |
| OLED SDA       |    4 |
| LED            |   18 |

## 🧪 Hardware Testing Plan

Test each component separately before running the complete chatbot firmware.

- [ ] Verify ESP32 power and USB connection.
- [ ] Verify common GND connections.
- [ ] Test OLED display initialization.
- [ ] Test INMP441 microphone audio capture.
- [ ] Test MAX98357A audio output.
- [ ] Test 4Ω 3W speaker.
- [ ] Test LED status indicator.
- [ ] Verify microphone and speaker operation together.
- [ ] Integrate all components with XIAOZHI firmware.

## ⚠️ Important Hardware Notes

1. Never reverse VCC and GND.
2. Never connect the INMP441 VDD to 5V.
3. Always use a common ground for the modules.
4. Never connect the speaker directly to ESP32 GPIO pins.
5. Never connect the MAX98357A speaker outputs to GND.
6. Check the actual GPIO labels on your ESP32 board before connecting wires.
7. Verify that the firmware GPIO configuration matches the physical connections.
8. Ensure the power supply can handle the amplifier's current requirements.

## 📁 Suggested GitHub Repository Structure

```text
XIAOZHI-ESP32-VOICE-ASSISTANT/
│
├── docs/
│   ├── wiring-diagram.png
│   ├── components.md
│   └── connection-guide.md
│
├── firmware/
│   └── README.md
│
├── images/
│   ├── esp32-board.jpg
│   ├── microphone.jpg
│   ├── amplifier.jpg
│   └── oled-display.jpg
│
├── README.md
└── LICENSE
```

## 🚀 Project Development Roadmap

- [ ] Hardware component selection
- [ ] Initial wiring diagram
- [ ] Ground connections
- [ ] Power connections
- [ ] Signal connections
- [ ] Individual component testing
- [ ] I2S microphone integration
- [ ] Audio amplifier integration
- [ ] OLED interface integration
- [ ] AI chatbot integration
- [ ] Complete voice assistant testing

## 🎯 Project Goal

Build a compact AI voice assistant using ESP32 hardware that can listen to user voice input, communicate with an AI backend, and respond through a speaker while displaying relevant information on an OLED screen.

**Project Name:** BUDDY
**Controller:** ESP32 DevKit V1 (ESP32-WROOM-32)
**Audio Input:** INMP441
**Audio Output:** MAX98357A + 4Ω 3W Speaker
**Display:** 0.96-inch OLED, 128×64
