# K4Y Puck OTA

Public release channel for verified K4Y Puck firmware images and the OTA manifest.

## Current state

- Release target: build 234 / 0.4.0.
- Build 234 adds the mockup-based round UI, persistent text/sound/rotation/screensaver settings, finite one-shot PCM5101 alerts, QMI8658 auto-rotation, dispatcher messages and phone-assisted Wi-Fi recovery.
- Screensaver delay is selectable as 30, 60 or 120 seconds.
- Build 233 / 0.3.4 remains the known silent rollback fallback.
- Build 232 remains the older known OTA-capable fallback.
- Build 231 is retained only for forensic comparison and must not be installed.

Build 234 uses the existing dual-slot OTA layout and retains the boot self-test for
Czech font glyphs, an LCD flush, LVGL startup and touch. A failed first boot remains
eligible for ESP-IDF rollback to the previously running image.

Every release must pass the repository gate: exact expected size, SHA-256, ESP32
application-image validation, and byte-for-byte public download verification.

This repository contains distributable firmware artifacts only. Application source code,
credentials and pairing tokens are not published here.
