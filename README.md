# ESP32-BLUETOOTH-WIFI-JAMER-SCANNER
A portable, multi-protocol wireless research tool built on the ESP32. nRFBox combines 2.4 GHz ISM band analysis, Bluetooth Low Energy tools, and Wi-Fi utilities into a single handheld device with an OLED display, icon-based menu, and NeoPixel status indicator.

> **For educational and authorized security research use only. Always obtain permission before testing on any network or device you do not own.**

---

## Features

### 2.4 GHz / nRF24 (ISM Band)
| Tool | Description |
|---|---|
| **Scanner** | Scans all 2.4 GHz channels and displays signal activity in real time |
| **Analyzer** | Channel-by-channel power analysis with graphical display |
| **WLAN Jammer** | Floods the 2.4 GHz band using three nRF24L01+ radios simultaneously |
| **Proto Kill** | Targets and disrupts non-WiFi 2.4 GHz protocols (Zigbee, RC, etc.) |

### Bluetooth Low Energy
| Tool | Description |
|---|---|
| **BLE Jammer** | Transmits interference across all three BLE advertising channels |
| **BLE Spoofer** | Spoofs BLE advertisements as Apple, Samsung, or Google devices |
| **Sour Apple** | Sends continuous Apple BLE proximity pairing popups |
| **BLE Scan** | Passive BLE device scanner showing device names and RSSI |

### Wi-Fi
| Tool | Description |
|---|---|
| **WiFi Scan** | Lists nearby access points with SSID, BSSID, channel, and signal strength |
| **Deauther** | Sends deauthentication frames to a selected access point |

### System
| Feature | Description |
|---|---|
| **Settings** | Toggle NeoPixel, adjust OLED brightness (saved to EEPROM) |
| **OTA Firmware Update** | Flash new firmware from SD card (`/firmware.bin`) |
| **NeoPixel Status** | RGB LED gives live feedback for active tool states |

---

## Hardware

| Component | Details |
|---|---|
| Microcontroller | ESP32 (any 4MB flash variant) |
| Display | SSD1306 128×64 OLED (I2C) |
| 2.4 GHz Radios | 3× nRF24L01+ modules |
| Status LED | WS2812B NeoPixel (1 LED, GPIO 14) |
| Storage | MicroSD card slot (SPI, CS GPIO 5) |
| Navigation | 5× tactile push buttons |

### Pin Map

```
── Buttons ──────────────────────
  UP        GPIO 26
  DOWN      GPIO 32
  LEFT      GPIO 25
  RIGHT     GPIO 27
  SELECT    GPIO 33

── nRF24L01+ Radio A ───────────
  CE        GPIO 5
  CSN       GPIO 17

── nRF24L01+ Radio B ───────────
  CE        GPIO 16
  CSN       GPIO 4

── nRF24L01+ Radio C ───────────
  CE        GPIO 15
  CSN       GPIO 2

── SD Card (SPI) ────────────────
  CS        GPIO 5

── NeoPixel ─────────────────────
  Data      GPIO 14

── OLED (I2C) ───────────────────
  SDA/SCL   Default ESP32 I2C pins
```

---

## Getting Started

### Requirements

- Arduino IDE 2.x
- ESP32 board package (`esp32` by Espressif, v3.x or later)

### Libraries (install via Library Manager)

| Library | Purpose |
|---|---|
| U8g2 | OLED display |
| Adafruit NeoPixel | RGB LED |
| RF24 | nRF24L01+ radios |
| ESP32 BLE Arduino | Bluetooth (bundled with ESP32 core) |
| SD (ESP32 built-in) | SD card firmware updates |

### Build & Flash

1. Clone this repository
   ```bash
   git clone https://github.com/YOUR_USERNAME/nRFBox.git
   ```
2. Open `nRFBox.ino` in Arduino IDE
3. Select your ESP32 board under `Tools → Board`
4. **Critical — set the partition scheme:**
   ```
   Tools → Partition Scheme → Huge APP (3MB No OTA / 1MB SPIFFS)
   ```
   The sketch is ~1.8 MB compiled. Without this change it will not fit and you will get a *"text section exceeds available space"* error.
5. Select the correct COM port and click **Upload**

---

## Project Structure

```
nRFBox/
├── nRFBox.ino        Main sketch — menu system and navigation
├── config.h          Pin definitions, library includes, namespace declarations
├── setting.h / .cpp  Global objects, radio helpers, display utilities
├── bluetooth.cpp     BLE Jammer, BLE Spoofer, Sour Apple, BLE Scan
├── wifi.cpp          WiFi Scanner, Deauther
├── ism.cpp           nRF24 Scanner, Analyzer, WLAN Jammer, Proto Kill
├── neopixel.cpp      NeoPixel colour and flash helpers
└── icon.h            XBM bitmap icons for the OLED menu
```

---

## OTA Firmware Update

1. Copy your new firmware binary to the root of a FAT32-formatted microSD card and name it `firmware.bin`
2. Insert the card
3. Navigate to **Setting → Update Firmware** on the device
4. The device will flash the new firmware and restart automatically

---

## License

This project is licensed under the **MIT License** — see [LICENSE](LICENSE) for details.

Original project by [CiferTech](https://github.com/cifertech/nrfbox). This fork includes compilation fixes and flash size optimizations.

---

## Disclaimer

This tool is intended for **educational purposes and authorized security research only**. Jamming radio communications is **illegal in most countries** without explicit authorization. The authors take no responsibility for misuse. Always comply with local laws and regulations.
