# K4Y Puck OTA

Public release channel for verified K4Y Puck firmware images and the OTA manifest.

Current verified bootstrap release: build 231 / 0.3.2.

Build 229 requires one final cable update of `K4Y-Puck-231.bin` at `0x10000`
with erase disabled. Build 231 and later read this public OTA channel automatically.

Every published firmware image must pass the repository release gate: exact expected
size, SHA-256, ESP32 application-image validation, and a byte-for-byte public download check.

This repository contains distributable firmware artifacts only. Application source code,
credentials and pairing tokens are not published here.
