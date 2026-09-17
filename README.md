# K4Y Puck OTA

Public release channel for verified K4Y Puck firmware images and the OTA manifest.

## Current state

- Emergency release target: build 233 / 0.3.4.
- Build 233 disables the audio/I2S path completely to guarantee silence.
- Its primary action button has a larger touch target and fires on the first confirmed press.
- Build 232 remains the known OTA-capable fallback.
- Build 231 is retained only for forensic comparison and must not be installed.

Build 233 uses the existing dual-slot OTA layout and retains the boot self-test for
Czech font glyphs, an LCD flush, LVGL startup and touch. A failed first boot remains
eligible for ESP-IDF rollback to the previously running image.

Every release must pass the repository gate: exact expected size, SHA-256, ESP32
application-image validation, and byte-for-byte public download verification.

This repository contains distributable firmware artifacts only. Application source code,
credentials and pairing tokens are not published here.
