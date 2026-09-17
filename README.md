# K4Y Puck OTA

Public release channel for verified K4Y Puck firmware images and the OTA manifest.

## Current state

- Stable device baseline: build 229.
- Candidate awaiting physical display/touch validation: build 232 / 0.3.3.
- Automatic OTA manifest is disabled until that validation is complete.
- Build 231 is retained only for forensic comparison and must not be installed.

Build 232 uses the existing dual-slot OTA layout and delays image confirmation until
its boot self-test verifies Czech font glyphs, an LCD flush, LVGL startup and touch.
A failed first boot remains eligible for ESP-IDF rollback to build 229.

Every release must pass the repository gate: exact expected size, SHA-256, ESP32
application-image validation, and byte-for-byte public download verification.

This repository contains distributable firmware artifacts only. Application source code,
credentials and pairing tokens are not published here.
