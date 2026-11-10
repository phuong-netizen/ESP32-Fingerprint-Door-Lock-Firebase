# Setup guide

[Back to project overview](../README.md)

1. Install the ESP32 board package in Arduino IDE and these libraries: **Adafruit Fingerprint Sensor Library**, **RTClib**, **Keypad**, **LiquidCrystal_I2C**, and **ArduinoJson**.
2. Copy the contents of `src/` into a sketch folder named `doan1_updated`, then open `doan1_updated.ino`.
3. Check the pin definitions before wiring: fingerprint UART uses ESP32 RX **14** / TX **15**; relay **13**; I²C SDA **21** / SCL **22**. Keypad pins are defined in the sketch.
4. Set your own Wi-Fi credentials and Firebase URL in `firebase_sync.h`; use the same database URL in `dashboard.html`. The database paths are `/logs`, `/ids`, and `/status`.
5. Set the RTC time once using the commented `rtc.adjust(...)` line in `control_time.h`, then disable it before subsequent uploads. Upload the firmware and open Serial Monitor at **115200 baud**.
6. Open `dashboard/dashboard.html` in a browser. Use the PIN menu to register staff/guests; admin fingerprints must already be enrolled in IDs 1–10.
