# ESP32 Sync Lights

Portable ambient lighting for Windows and an ESP32-controlled WS2812B strip. The app samples a selected screen and sends its edge colors to the strip over USB. It includes brightness control, lighting effects, and configurable dark gaps at folded corners.

Desktop: **1.0.4**. Firmware: **5.25.3-portable.2**.

## Quick start

1. Power the strip from an external 5V supply. Connect the supply negative, strip GND, and ESP32 GND together. Connect GPIO13 / D13 to strip DIN.
2. Connect the ESP32 to the PC using a USB data cable.
3. Open `release/app/ESP32 Sync Lights.exe`. No launcher script, Java installation, or Python installation is needed.
4. In **Settings → Mode**, select the screen and ESP32 COM port, then save.
5. In **Settings → LEDs config**, configure the counts, start point, direction, and corner gaps. Save the layout.
6. Select **Bias light** and turn LEDs on. Output starts off when the app opens.

## Guides

- [App usage](docs/usage.md): wiring, layout, brightness, effects, tests, and troubleshooting.
- [Firmware](docs/firmware.md): supported board, flashing, and recovery.
- [Build and share](docs/build.md): rebuilding and preparing a manual Git upload.

## Compact source upload

This folder is prepared for manual Git upload. Extract `source.zip` into `release/` to get `release/src/`. Download the matching portable app separately and place it at `release/app/` before rebuilding the desktop app. Screenshots, upstream websites, prebuilt firmware, and the downloadable Lombok JAR are omitted. Build scripts retrieve development dependencies when needed.

## Package contents

| Path | Purpose |
| --- | --- |
| `release/app/` | Runnable app with required dependencies |
| `release/lights.zip` | Clean app package; extracts to `app/` |
| `source.zip` | Compact corresponding source; extracts to `src/` |
| `checksums.txt` | SHA256 hash of the compact source ZIP |
| `docs/` | Instructions |

Keep the EXE with its bundled folders. Settings, logs, caches, and temporary files stay inside the app folder. The installed app may contain your personal layout; the transfer ZIP has clean defaults.

## Supported hardware

Windows x64, a classic ESP32 / esp32dev with 4MB flash, and up to 300 physical WS2812B positions including gaps. The supplied firmware is not for ESP32-C3, S2, or S3. This setup uses USB serial.

Protected video can prevent screen capture and make the LEDs dark. Test desktop colors or YouTube first.

## Credits and licensing

Derived from [Firefly Luciferin](https://github.com/sblantipodi/firefly_luciferin) and [Glow Worm Luciferin](https://github.com/sblantipodi/glow_worm_luciferin). Original copyright and licensing notices remain in the source and `release/app/legal/`.

When sharing the app, retain the required notices and provide the corresponding source.
