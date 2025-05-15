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

### Final Output

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


## Project Images

![Description for Image 1](IMG_4234.HEIC)
![Description for Image 2](IMG_4232.HEIC)


#### Code Used

/*
 *  This sketch demonstrates how to scan WiFi networks.
 *  The API is based on the Arduino WiFi Shield library, but has significant changes as newer WiFi functions are supported.
 *  E.g. the return value of `encryptionType()` different because more modern encryption is supported.
 */
#include "WiFi.h"

void setup() {
  Serial.begin(115200);

  // Set WiFi to station mode and disconnect from an AP if it was previously connected.
  WiFi.mode(WIFI_STA);
  WiFi.disconnect();
  delay(100);

  Serial.println("Setup done");
}

void loop() {
  Serial.println("Scan start");

  // WiFi.scanNetworks will return the number of networks found.
  int n = WiFi.scanNetworks();
  Serial.println("Scan done");
  if (n == 0) {
    Serial.println("no networks found");
  } else {
    Serial.print(n);
    Serial.println(" networks found");
    Serial.println("Nr | SSID                             | RSSI | CH | Encryption");
    for (int i = 0; i < n; ++i) {
      // Print SSID and RSSI for each network found
      Serial.printf("%2d", i + 1);
      Serial.print(" | ");
      Serial.printf("%-32.32s", WiFi.SSID(i).c_str());
      Serial.print(" | ");
      Serial.printf("%4ld", WiFi.RSSI(i));
      Serial.print(" | ");
      Serial.printf("%2ld", WiFi.channel(i));
      Serial.print(" | ");
      switch (WiFi.encryptionType(i)) {
        case WIFI_AUTH_OPEN:            Serial.print("open"); break;
        case WIFI_AUTH_WEP:             Serial.print("WEP"); break;
        case WIFI_AUTH_WPA_PSK:         Serial.print("WPA"); break;
        case WIFI_AUTH_WPA2_PSK:        Serial.print("WPA2"); break;
        case WIFI_AUTH_WPA_WPA2_PSK:    Serial.print("WPA+WPA2"); break;
        case WIFI_AUTH_WPA2_ENTERPRISE: Serial.print("WPA2-EAP"); break;
        case WIFI_AUTH_WPA3_PSK:        Serial.print("WPA3"); break;
        case WIFI_AUTH_WPA2_WPA3_PSK:   Serial.print("WPA2+WPA3"); break;
        case WIFI_AUTH_WAPI_PSK:        Serial.print("WAPI"); break;
        default:                        Serial.print("unknown");
      }
      Serial.println();
      delay(10);
    }
  }
  Serial.println("");

  // Delete the scan result to free memory for code below.
  WiFi.scanDelete();

  // Wait a bit before scanning again.
  delay(5000);
}
