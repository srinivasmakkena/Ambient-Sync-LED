# Build and share

## Extract the source

Extract `source.zip` inside `release/`. This creates `release/src/` beside `release/app/`. Use a short path.

The source contains modified desktop and firmware code, build scripts, dependencies, tests, and licenses. App users do not need to extract it.

## Build the Windows app

Provide a JDK 26 directory. From the project root:

```powershell
.\release\src\tools\build-desktop.ps1 -Jdk 'C:\path\to\jdk-26' -BaseRuntime '../app' -Output '../rebuild'
```

The script compiles Java 25-compatible code using the existing app as the dependency/runtime base. Output is `release/rebuild/`. Keep `release/app/` available during the build.

Baselines are Firefly Luciferin v2.28.4 and Glow Worm Luciferin v5.25.3. Build scripts and lock files identify dependencies. ZIP timestamps and compiler metadata may vary between builds.

## Build the firmware

Python 3.12 or newer and internet access are needed for the initial build:

```powershell
.\release\src\tools\build_firmware.ps1 -InstallTools
```

The script creates a source-local Python environment and downloads pinned PlatformIO tools. Later builds can omit `-InstallTools`.

Binaries and their manifest are written to `release/src/firmware/`. Building does not flash. Follow [Firmware](firmware.md), substituting these generated paths.

## Prepare a manual Git upload

1. Use the extracted `src/` contents as the source tree, or retain the subfolder and adjust documentation links.
2. Include the current README and usage, firmware, and build guides.
3. Preserve upstream copyright, license, and attribution files.
4. Exclude personal config, logs, caches, Python environments, temporary files, and generated build output.
5. Publish large binary packages as release assets: the clean app ZIP, matching source ZIP, firmware binaries, and checksums.

Use `lights.zip`, not your configured app folder, for distribution. Keep matching app, firmware, and source versions together. No Git upload is performed automatically.

## Verify packages

From the upload folder:

```powershell
Get-FileHash -LiteralPath source.zip -Algorithm SHA256
```

Compare with `checksums.txt`. Regenerate checksums whenever an archive changes.
