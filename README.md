# K4Y Puck OTA

Public release channel for verified K4Y Puck firmware images and the OTA manifest.

## Current state

- Release target: build 239 / 0.4.5.
- Build 239 stabilizes online/offline presence, keeps periodic sync in the background,
  retries transient API failures and prevents a transient touch-controller failure from
  rejecting an otherwise usable OTA image.
- Build 238 contains the exact K4Y screensaver logo and unified direct-message and
  announcement delivery. Its public binary was restored byte-for-byte from the verified
  CI artifact during the build 239 release.
- Build 237 remains the verified OTA-capable rollback fallback.
- Build 231 is retained only for forensic comparison and must not be installed.

Build 239 uses the existing dual-slot OTA layout and retains the boot self-test for
Czech font glyphs, an LCD flush and LVGL startup. Touch remains visible in diagnostics
but no longer rejects a usable image when it is only transiently unavailable. A failed first boot remains
eligible for ESP-IDF rollback to the previously running image.

Every release must pass the repository gate: exact expected size, SHA-256, ESP32
application-image validation, and byte-for-byte public download verification.

This repository contains distributable firmware artifacts only. Application source code,
credentials and pairing tokens are not published here.
