# 🤖  A.I ESP32 Voice Assistant

An ESP32-based voice assistant prototype using an INMP441 digital microphone, MAX98357A audio amplifier, speaker, and 0.96-inch OLED display.

## 📸 Hardware Wiring Diagram

<img width="1395" height="873" alt="IM" src="https://github.com/user-attachments/assets/3e7778a9-8c34-4b5a-aaf2-7011e8379625" />


> This is the reference wiring image supplied for the project. Verify every pin against your actual modules and firmware before applying power.

## 🔧 Hardware Components

| Component | Description | Quantity |
|---|---|---:|
| ESP32 DevKit V1 | ESP32-WROOM-32, 30-pin development board | 1 |
| INMP441 | I2S digital microphone | 1 |
| MAX98357A | I2S audio amplifier | 1 |
| Speaker | 4Ω, 3W speaker | 1 |
| OLED display | 0.96-inch, 128×64, I2C | 1 |
| LED | Status indicator | 1 |
| Resistor | 220Ω | 1 |
| Jumper wires | Male-to-female / male-to-male | As required |

## 🔌 GPIO Pin Configuration

The following assignments are transcribed from the supplied wiring diagram. Confirm them against your board, module labels, and firmware before connecting.

### 1. INMP441 – Digital Microphone

| INMP441 pin | ESP32 connection | Function |
|---|---|---|
| VDD | 3V3 | Power |
| GND | GND | Ground |
| SD | GPIO 32 | Microphone data |
| WS | GPIO 25 | Word select |
| SCK | GPIO 26 | Serial clock |
| L/R | GND | Left-channel selection |

**Power note:** Use 3.3V for INMP441 VDD.

### 2. MAX98357A – Audio Amplifier

| MAX98357A pin | ESP32 connection | Function |
|---|---|---|
| VIN | 3V3 (as drawn; verify supply requirements) | Power |
| GND | GND | Ground |
| SD | 3V3 (as drawn) | Shutdown/enable |
| DIN | GPIO 33 | Audio data |
| BCLK | GPIO 14 | Bit clock |
| LRC | GPIO 27 | Word-select clock |

**Speaker connections:**
- Speaker (+) → MAX98357A `SPK+`
- Speaker (−) → MAX98357A `SPK−`

Do **not** connect either speaker output to ESP32 GND. The speaker connects to the amplifier, not directly to ESP32 GPIO pins.

### 3. OLED Display – 0.96-inch, 128×64

| OLED pin | ESP32 connection | Function |
|---|---|---|
| GND | GND | Ground |
| VCC | 3V3 | Power |
| SCL | GPIO 15 | I2C clock |
| SDA | GPIO 4 | I2C data |

### 4. LED – Status Indicator

| LED connection | ESP32 connection |
|---|---|
| Anode (+), long leg | GPIO 18 through a 220Ω resistor |
| Cathode (−), short leg | GND |

The resistor limits current through the LED.

## ⚫ Stage 1 – Ground (Black-Wire) Connections

Start with black wires only. Keep the ESP32 disconnected from USB and external power while wiring.

| Component | Ground connection | Connect to |
|---|---|---|
| INMP441 | GND | ESP32 GND |
| MAX98357A | GND | ESP32 GND |
| OLED | GND | ESP32 GND |
| LED | Cathode (−) | ESP32 GND |
| Speaker | No system-ground connection | Connect only to MAX98357A `SPK+` and `SPK−` |

### Ground wiring steps

1. Locate a GND pin on the ESP32.
2. Connect INMP441 GND to ESP32 GND.
3. Connect MAX98357A GND to ESP32 GND.
4. Connect OLED GND to ESP32 GND.
5. Connect the LED cathode to ESP32 GND.
6. Recheck that no ground wire is connected to a power pin.

A breadboard's common negative rail can distribute GND if that rail is connected to ESP32 GND.

**Important:** The speaker's `SPK−` terminal is an amplifier output, not system GND.

## 🔴 Stage 2 – Power Connections

After checking the ground wiring, add power connections.

| Component | Power pin | Proposed connection in diagram |
|---|---|---|
| INMP441 | VDD | ESP32 3V3 |
| MAX98357A | VIN | ESP32 3V3 (verify current and supply requirements first) |
| MAX98357A | SD | 3V3 |
| OLED | VCC | ESP32 3V3 |

Do not assume the ESP32 3.3V regulator can supply the amplifier's peak current. Use an appropriate regulated supply if required, connect supply grounds together, and check the exact breakout-board documentation.

## 🟡 Stage 3 – Signal Connections

| Signal | ESP32 GPIO |
|---|---:|
| INMP441 SD | GPIO 32 |
| INMP441 WS | GPIO 25 |
| INMP441 SCK | GPIO 26 |
| MAX98357A DIN | GPIO 33 |
| MAX98357A BCLK | GPIO 14 |
| MAX98357A LRC | GPIO 27 |
| OLED SCL | GPIO 15 |
| OLED SDA | GPIO 4 |
| LED control | GPIO 18 |

## 🧪 Hardware Testing Checklist

- [ ] Inspect all wiring with power disconnected.
- [ ] Verify common GND connections.
- [ ] Power and initialize the OLED.
- [ ] Test INMP441 microphone capture.
- [ ] Test MAX98357A audio output.
- [ ] Test the 4Ω, 3W speaker at a safe volume.
- [ ] Test the LED through its 220Ω resistor.
- [ ] Test microphone input and speaker output together.
- [ ] Confirm firmware GPIO settings match the physical wiring.
- [ ] Integrate and test the complete voice assistant.

## ⚠️ Safety and Compatibility Notes

1. Disconnect USB and external power before changing wiring.
2. Never reverse VCC and GND.
3. Power the INMP441 from 3.3V, not 5V.
4. All modules must share a common system ground.
5. Do not connect the speaker directly to ESP32 GPIO pins.
6. Do not connect MAX98357A `SPK+` or `SPK−` to GND.
7. Check the actual GPIO labels on your ESP32 board.
8. Confirm firmware pin configuration matches the wiring.
9. Verify the amplifier board's supply-voltage and current requirements before powering it.

## 📁 Repository Structure

```text
XIAOZHI-ESP32-VOICE-ASSISTANT/
├── docs/
│   └── wiring-diagram.png
├── firmware/
│   └── README.md
├── images/
├── README.md
└── LICENSE
```

## 🚀 Development Roadmap

- [x] Select the main hardware components
- [x] Prepare the initial wiring reference
- [ ] Complete ground connections
- [ ] Complete and verify power connections
- [ ] Complete signal connections
- [ ] Test each module independently
- [ ] Integrate I2S microphone
- [ ] Integrate audio amplifier and speaker
- [ ] Integrate OLED display
- [ ] Integrate AI chatbot backend
- [ ] Test the complete voice assistant

## 🎯 Project Goal

Build a compact ESP32-based AI voice assistant that can listen to a user, send voice input for AI processing, respond through a speaker, and display useful status information on an OLED screen.

**Project name:** BUDDY 
**Controller:** ESP32 DevKit V1 (ESP32-WROOM-32)  
**Audio input:** INMP441  
**Audio output:** MAX98357A + 4Ω, 3W speaker  
**Display:** 0.96-inch OLED, 128×64
