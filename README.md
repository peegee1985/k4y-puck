# K4Y Puck OTA

Public release channel for verified K4Y Puck firmware images and the OTA manifest.

## Current state

- Release target: build 254 / 0.4.20.
- Build 254 retries missing/failed touch and IMU inputs, preserving the SPI display and
  the accepted 251 rotation mapping. I2C transactions and EXIO changes share a mutex.
- Degree/ellipsis glyphs are present in every Czech font size; pairing and Wi-Fi setup
  explicitly use Czech fonts. Fixed rotation modes are labelled PEVNĚ 0° / PEVNĚ 90°.
- Hold PWR for two seconds, then release: UI stops, current settings and SD data are flushed,
  SD is unmounted, and the battery latch is released. With USB attached, the MCU deep sleeps
  until PWR; the charging circuit remains powered. Active OTA excludes shutdown.
- Actual LVGL text/pixel/touch tests and ESP-IDF 5.5.2 build pass. Recovery and shutdown were
  fault-tested on the host; physical testing on the owner's board remains pending.
  See releases/254.md.
- Build 253 adds the reviewed Czech spoken notifications “Nová zakázka” and “Nová zpráva”,
  each with two tones before and after. Audio streams from embedded flash in 512-byte blocks;
  existing volume, mute and driver-login gates still apply. Downloadable WAV previews are in audio/.
- Device status displays live filtered battery voltage next to its percentage.
  A 97% reading alone does not diagnose degraded capacity: the ETA6098 CV specification is
  4.16–4.24 V and this firmware's nominal voltage curve maps 4.160 V to 97%.
  Charging limits and calibration have not been blindly changed; see releases/253.md.
- Build 252 follows the driver's phone login: logout locks work/messages and
  job actions; login restores access without pairing. Server checks apply to every action.
  Screensaver ONLINE/OFFLINE is driver login; unverified connectivity shows STAV NEZNÁMÝ.
- SD now stores an authenticated read-only job copy and a bounded diagnostic journal.
  Wi-Fi profiles stay in NVS (about 500 bytes for five) so startup works without SD.
- Build 251 reverses incorrect 90/270 automatic rotation compensation from build 250.
  Field testing rejected 250's rotation; the owner reports improved rotation after 251.
- Build 250 fixes OTA text overlap and adds
  independent 10–100% screensaver brightness under the Puck's Settings menu.
  Battery percentage mapping/display were audited; capacity remains voltage-estimated.
- Build 249 places screensaver battery percentage close to the icon with 6px gaps,
  removes the empty hidden-bolt slot and automatically centres time plus battery.
- Build 248 adds battery level and a conservative charging-trend bolt below Settings
  and beside the centred screensaver clock. Percentage is voltage-derived; charging
  is estimated because the board's ETA6098 STAT signal only drives its physical LED.
  Invalid/stale readings become unknown rather than a fabricated full battery.
- Build 247 supplies the physically accepted new-device battery power path used
  as the base for build 248. OTA keeps pairing, Wi-Fi and settings.
- Build 246 replaces unbounded header notices with a compact green/amber/red status dot,
  bounds OTA and pending-action labels, and skips full LVGL dashboard reconstruction when
  a periodic synchronization returns unchanged data.
- Build 245 adds safe one-shot remote restart, forced OTA checks and remote display,
  sound, orientation, text-size and screensaver settings from the web dispatcher.
  Smartwatch pairings are no longer misreported as offline Pucks.
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

Build 253 uses the existing dual-slot OTA layout and retains the boot self-test for
Czech font glyphs, an LCD flush and LVGL startup. Touch remains visible in diagnostics
but no longer rejects a usable image when it is only transiently unavailable. The SD
probe runs while a new image is still pending verification; a fatal driver regression
therefore remains eligible for ESP-IDF rollback. Missing or exFAT cards are non-fatal.

Every release must pass the repository gate: exact expected size, SHA-256, ESP32
application-image validation, and byte-for-byte public download verification.

This repository contains distributable firmware artifacts only. Application source code,
credentials and pairing tokens are not published here.
