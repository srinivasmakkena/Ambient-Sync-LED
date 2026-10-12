# ESP32 firmware guide

## Board and behavior

Firmware **5.25.3-portable.2** is built for a classic **ESP32 / esp32dev with 4MB flash**. Do not use these binaries on ESP32-C3, S2, or S3. Flashing replaces MicroPython.

The board receives colors over USB serial at **500000 baud** and drives the strip on **GPIO13 / D13**. A saved physical mask keeps gaps and skipped LEDs dark. A missing, invalid, or count-mismatched mask blocks output. This guide covers USB operation.

## Wiring and preparation

- GPIO13 → strip DIN.
- External regulated 5V → strip +5V.
- Supply negative → strip GND and ESP32 GND.
- PC USB → ESP32 USB.

Do not power the strip through ESP32 VIN. Before flashing, switch off strip power, close the lighting app and other serial tools, and leave the ESP32 connected by USB.

## Choose a binary

Files are in `release/app/firmware/`.

| File ending | Flash address | Purpose |
| --- | --- | --- |
| `-factory.bin` | `0x0` | First installation, replacing MicroPython, or recovery |
| `-app.bin` | `0x10000` | Update an existing installation with matching partitions |

App-only flashing preserves storage outside the application segment. A full-chip erase clears settings and masks. A factory image does not necessarily clear every previous settings sector; save the layout in the app after replacing unrelated firmware.

Check `manifest.json` for board details, offsets, and hashes. `SHA256SUMS.txt` lists binary hashes.

## Install a project-local flashing tool

Python 3.12 or newer is required for flashing, not for running the app. In PowerShell, from the project root:

```powershell
$env:TEMP = Join-Path (Get-Location) '.flash-tmp'
$env:TMP = $env:TEMP
New-Item -ItemType Directory -Force $env:TEMP | Out-Null
python -m venv .flash
.\.flash\Scripts\python.exe -m pip install --cache-dir .flash-cache esptool
```

These commands install the tool without flashing. Replace COM9 below with the port shown in Device Manager.

## First installation or recovery

With strip power off and the app closed:

```powershell
.\.flash\Scripts\python.exe -m esptool --chip esp32 --port COM9 --baud 115200 write-flash 0x0 .\release\app\firmware\ESP32SyncLights-5.25.3-portable.2-factory.bin
```

## App-only update

Use this only for an existing matching partition layout:

```powershell
.\.flash\Scripts\python.exe -m esptool --chip esp32 --port COM9 --baud 115200 write-flash 0x10000 .\release\app\firmware\ESP32SyncLights-5.25.3-portable.2-app.bin
```

Wait for successful verification. If automatic download mode fails, hold **BOOT** while the tool connects and release it when writing starts. Some boards require an **EN/reset** press while BOOT is held.

Command syntax follows [Espressif's esptool documentation](https://docs.espressif.com/projects/esptool/en/latest/esp32/esptool/basic-commands.html).

## After flashing

1. Reset the board or reconnect USB.
2. Open the app, choose the current COM port, and save.
3. Save the strip layout.
4. Restore strip power after checking wiring.
5. Run the single-LED test, then the screen color test.

Output starts off. A dark strip is expected until the layout is valid and output is enabled.

## Rebuilding

See [Build and share](build.md). The source includes the pinned PlatformIO configuration and mask implementation. Building does not flash the board. Use the generated binary paths when flashing a rebuilt version.
