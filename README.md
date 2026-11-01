# qT-Embed V1

[![Discord](https://img.shields.io/badge/Discord-Join%20the%20server-5865F2?logo=discord&logoColor=white)](https://discord.gg/2vWpzvJRkX)
[![Platform](https://img.shields.io/badge/platform-Windows%2010%2F11-0078D6?logo=windows&logoColor=white)](../../releases)
[![License](https://img.shields.io/badge/license-GPL--3.0-blue)](LICENSE)
[![Release](https://img.shields.io/github/v/release/<your-user>/qT-Embed?include_prereleases)](../../releases)

Desktop companion app for the **LilyGo T-Embed (CC1101 / CC1101 Plus)** running the
[Flipper-Zero-ESP32-Port](https://github.com/Sor3nt/Flipper-Zero-ESP32-Port) firmware.
Windows 10/11 (x64).

qT-Embed is a customized fork of [qFlipper](https://github.com/flipperdevices/qFlipper) by TBX, with a purple UI,
a virtual T-Embed demo mode and a Windows installer that needs no extra setup.

## Features

- **Live screen streaming**: mirror the T-Embed display and control it with the on-screen D-pad or the keyboard.
- **File manager**: browse the device storage and upload or download files.
- **Virtual demo mode** (no device needed): click **DEMO** on the start screen, choose a board
  (T-Embed CC1101 or CC1101 Plus) and a firmware, and try the menus on a simulated 320x170 display.
  - Firmware: **Bruce** or **Flipper Zero ESP32 Port**.
  - **8 selectable Bruce UI themes**: Bebra First Creation, Mad Shark, Dark Mode, Cyber Hacker, Bruce Theme,
    BLWS, Shark Blue and Spider-Man, including their boot animations.
  - If a Bruce `.bin` is found in your Downloads folder (or picked manually), its chip, flash size and
    partitions are read and shown.
- **Custom accent colors** through the THEME button.

> The demo is a simulation of the menus. It does not emulate or run real firmware.

## Download and install

1. Download `T-Embed-Setup.exe` from the [Releases](../../releases) page.
2. Run it and follow the wizard. Everything the app needs is included.
3. Optional during setup: the **STM32 DFU USB driver** (about 5 MB) is downloaded and checked with SHA-256.
   It is only needed for DFU recovery of an original Flipper Zero, not for the T-Embed.

The installer is not code-signed, so Windows SmartScreen may warn you. Choose **More info > Run anyway**.

## Connecting a real device

The app detects devices that report the Flipper USB IDs `0483:5740` and answer the Flipper RPC protocol.
That is what the **Flipper-Zero-ESP32-Port** firmware provides.

- Other firmwares, such as **Bruce**, use different USB IDs and are **not** detected. They are available in the demo only.
- If nothing is detected, open **LOGS** in the app. A line such as `Detected new device: VID_0x483:PID_0x5740`
  means the device was found, while `Unexpected device VID and PID` means the firmware reports other IDs.

## Build from source

The Windows build is cross-compiled on Linux inside a container (MinGW-w64 and Qt 6.4.2). You need
Docker or Podman and an internet connection for the first run.

```sh
git clone --recursive https://github.com/<your-user>/qT-Embed.git
cd qT-Embed
./build_windows_cross.sh        # incremental build
CLEAN=1 ./build_windows_cross.sh # full rebuild
```

The result is `output/T-Embed-Setup.exe`.

The script builds a builder image from `docker/Dockerfile.win` (Qt for Windows, MinGW, NSIS, Wine for tests),
compiles the app, collects DLLs, plugins and QML modules (`docker/win-deploy.py`) and packs everything with
NSIS (`installer_embed.nsi`).

A native Windows build with Visual Studio is also possible: adjust and run `build_windows.bat`
(MSVC 2019+, Qt 6.4+, NSIS).

## Project structure

- `application`: the graphical app, mostly QML (`components/Demo*.qml` and `VirtualScreen.qml` are the demo).
- `backend`: C++ backend library.
- `cli`: command line interface.
- `dfu`: low-level USB and DFU access.
- `plugins`: protobuf communication plugin.
- `3rdparty`: third-party libraries (nanopb).
- `docker`, `build_windows_cross.sh`, `installer_embed.nsi`: Windows cross-build and installer.
- `installer-assets`: installer artwork and icons.

## Community and support

Questions, feedback or bug reports? Join the Discord server: **https://discord.gg/2vWpzvJRkX**

## Credits and license

- Based on [qFlipper](https://github.com/flipperdevices/qFlipper) by Flipper Devices Inc.
- Firmware port: [Sor3nt/Flipper-Zero-ESP32-Port](https://github.com/Sor3nt/Flipper-Zero-ESP32-Port).
- [Bruce](https://github.com/pr3y/Bruce) is a separate project. Its firmware is **not** included in this repository.
- Bundled Bruce UI themes are community creations by their respective authors, among them CreeperRick (Cyber Hacker),
  Matt / icy.1 (Bruce Theme), Fantailed (BLWS) and koua29 (Spider-Man). Please open an issue if you are an author
  and want a theme credited differently or removed.

Licensed under the **GNU GPL v3**, see [LICENSE](LICENSE).
