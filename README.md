# ESP32 Fingerprint Door Lock

An ESP32-based access-control prototype with fingerprint authentication, a PIN-operated menu, time-based permissions, and a Firebase monitoring dashboard.

**[Watch the hardware demo](https://youtu.be/4xGycZWLZ9U)** · C++ / Arduino · HTML, CSS, JavaScript · Firebase Realtime Database

## Features

- Unlock by fingerprint or through the PIN menu; drive the lock relay for 3 seconds.
- Enroll and delete staff/guest fingerprints through the keypad and LCD.
- Store access schedules in ESP32 non-volatile memory and check them using a DS3231 RTC.
- Upload fingerprint access events and user records to Firebase; view them in the web dashboard.

| Role | Fingerprint IDs | Access |
| --- | --- | --- |
| Admin | 1–10 | Unrestricted; enrolled separately |
| Staff | 11–60 | Assigned shift: 06:00–14:00 or 14:00–22:00 |
| Guest / Client | 61–127 | Configured check-in/check-out window |

## Hardware and data flow

ESP32 DevKit V1, AS608 fingerprint sensor, 4×4 keypad, 16×2 I²C LCD, DS3231 RTC, relay, LY-03 12 V solenoid lock, and MP1584EN step-down supply.

```text
Fingerprint / Keypad → ESP32 + RTC → Relay → Door lock
                          │
                       Wi-Fi / REST
                          ↓
                 Firebase → Web dashboard
```

The firmware sends a status update every 10 seconds. The dashboard polls Firebase approximately every 5 seconds.

## Repository

- [src/](src/) — firmware, fingerprint enrollment, access rules, LCD menus, RTC and Firebase sync.
- [dashboard/dashboard.html](dashboard/dashboard.html) — monitoring interface.
- [png/](png/) — dashboard image assets.

## Quick start

1. Install the ESP32 board package in Arduino IDE and these libraries: **Adafruit Fingerprint Sensor Library**, **RTClib**, **Keypad**, **LiquidCrystal_I2C**, and **ArduinoJson**.
2. Copy the contents of `src/` into a sketch folder named `doan1_updated`, then open `doan1_updated.ino`.
3. Check the pin definitions before wiring: fingerprint UART uses ESP32 RX **14** / TX **15**; relay **13**; I²C SDA **21** / SCL **22**. Keypad pins are defined in the sketch.
4. Set your own Wi-Fi credentials and Firebase URL in `firebase_sync.h`; use the same database URL in `dashboard.html`. The database paths are `/logs`, `/ids`, and `/status`.
5. Set the RTC time once using the commented `rtc.adjust(...)` line in `control_time.h`, then disable it before subsequent uploads. Upload the firmware and open Serial Monitor at **115200 baud**.
6. Open `dashboard/dashboard.html` in a browser. Use the PIN menu to register staff/guests; admin fingerprints must already be enrolled in IDs 1–10.

## Prototype limitations

Cloud updates require Wi-Fi; offline log buffering is not implemented. The RTC battery value is currently fixed at 3.0 V, and door status reflects software events, not a physical door sensor. PIN changes are not retained after a restart. The current Firebase clients have no authentication flow; authentication and database access rules need to be implemented before deployment.

---
Course Project 1 · April 2026

**Nguyen Duc Phuong & Nguyen Minh Khang**
