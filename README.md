# K4Y Puck OTA

Public release channel for verified K4Y Puck firmware images and the OTA manifest.

## Current state

- Release target: build 244 / 0.4.10.
- Build 244 wakes the display for dispatcher messages and announcements and keeps
  the message visible with the screensaver disabled until the driver confirms it.
  SD benchmark speeds are shown with decimal precision instead of truncating below 1 MiB/s.
- Build 243 reduces a normal dashboard refresh from three TLS sessions to one after the
  matching backend is deployed, enables `-O2`, sends bounded fleet telemetry, benchmarks
  FAT32 SD storage non-destructively and reduces OTA TLS memory with 4 KiB requests.
- Build 242 verifies the exact OTA image SHA-256 on the Puck, repeats update checks
  every six hours, exposes runtime RAM health and safely probes 256GB-class SDXC
  cards in native one-bit mode without ever formatting them automatically.
- Build 241 rebuilds automatic rotation and tap/scroll arbitration, removes the
  LVGL busy-spin, shows real OTA percentages and lowers screensaver brightness to 30%.
- Build 240 adds five validated saved Wi-Fi profiles with automatic failover, migrates
  the existing build 239 connection without clearing NVS, and hardens tap/swipe handling.
- Build 239 stabilizes online/offline presence, background sync and OTA confirmation.
- Build 238 contains the exact K4Y screensaver logo and unified direct-message and
  announcement delivery. Its public binary was restored byte-for-byte from the verified
  CI artifact during the build 239 release.
- Build 237 remains the verified OTA-capable rollback fallback.
- Build 231 is retained only for forensic comparison and must not be installed.

Build 244 uses the existing dual-slot OTA layout and retains the boot self-test for
Czech font glyphs, an LCD flush and LVGL startup. Touch remains visible in diagnostics
but no longer rejects a usable image when it is only transiently unavailable. The SD
probe runs while a new image is still pending verification; a fatal driver regression
therefore remains eligible for ESP-IDF rollback. Missing or exFAT cards are non-fatal.

Every release must pass the repository gate: exact expected size, SHA-256, ESP32
application-image validation, and byte-for-byte public download verification.

This repository contains distributable firmware artifacts only. Application source code,
credentials and pairing tokens are not published here.
