# ESP32-CAM WiFi Network Scanner

A lightweight firmware example for the ESP32-CAM that scans and lists nearby WiFi networks, including SSID, signal strength (RSSI), channel, and encryption type.

---

## Supported Targets

This example supports the following ESP32 variants:

| **Target**      | Supported |
|------------------|-----------|
| ESP32            | ✔️       |
| ESP32-S2         | ✔️       |
| ESP32-C3         | ✔️       |
| ESP32-S3         | ✔️       |
| ESP32-C6         | ✔️       |

---

## Prerequisites

1. **Arduino IDE** ([Installation Guide](https://github.com/espressif/arduino-esp32/tree/master/docs/arduino-ide))
2. **ESP32 Board Package**:  
   - Add `https://dl.espressif.com/dl/package_esp32_index.json` to *File > Preferences > Additional Boards Manager URLs*.
   - Install via *Tools > Board > Boards Manager > ESP32*.

---

## Hardware Setup

1. Connect the ESP32-CAM to your computer using a USB-to-UART adapter:
   - **Wiring**:  
     - `5V` → `5V`  
     - `GND` → `GND`  
     - `U0R` (ESP32-CAM TX) → RX (Adapter)  
     - `U0T` (ESP32-CAM RX) → TX (Adapter)  
   - Hold the `BOOT` button while powering on to enter programming mode.

---

## Upload Instructions

### Using Arduino IDE
1. **Select Board**:  
   - *Tools > Board > ESP32 Arduino > AI Thinker ESP32-CAM*.
2. **Configure Port**:  
   - *Tools > Port* → Select your COM port (e.g., `COM3` or `/dev/ttyUSB0`).
3. **Upload Firmware**:  
   - Open the example: *File > Examples > WiFi > WiFiScan*.
   - Click **Upload** (➡️). Release the `BOOT` button once uploading starts.

### Using PlatformIO
1. **Configure `platformio.ini`**:
   ```ini
   [env:esp32cam]
   platform = espressif32
   board = esp32cam
   framework = arduino
   upload_port = COM5  ; Replace with your COM port
   monitor_speed = 115200

### Final OutPut

   Scan start  
Scan done  
11 networks found  
Nr | SSID                     | RSSI  | CH  | Encryption  
-------------------------------------------------  
1  | Verizon_ZL6MYQ           | -49   | 6   | WPA2  
2  | Verizon_M3WWHR           | -62   | 1   | WPA2  
3  | Verizon_M3WWIR-Guest     | -62   | 1   | WPA2  
4  | MyOptimum_0c8b71         | -62   | 11  | WPA2  
5  | iPhone                   | -68   | 6   | WPA2  
6  | MyOptimum_df32c1         | -69   | 1   | WPA2  
7  | MyAltice_714037          | -80   | 1   | WPA2  
8  | MyAltice_fec359_guest    | -89   | 11  | WPA2  
9  | SmartLife-BF98           | -90   | 6   | Open  
10 | MyAltice_fec359          | -92   | 11  | WPA2  
11 | At_401_WIN_056905_WW_1988| -94   | 11  | WPA2  
