# 3D Printed Mill Driver

Control-system notes for a small 3D-printed hobby CNC milling machine, built from common off-the-shelf 3D printer hardware and open-source software.

## What this is

A setup guide for people who already know 3D printers (Marlin/Klipper) and want to run a CNC mill the same way:

| Layer | Choice |
| --- | --- |
| Motion controller | BTT SKR Pico V1.0 (RP2040, on-board TMC2209 drivers) |
| Firmware | [grblHAL](https://github.com/grblHAL), built with the grblHAL Web Builder |
| Motors | NEMA17 steppers |
| Host | Raspberry Pi (64-bit Raspberry Pi OS) with a touchscreen |
| Sender (UI) | gSender (recommended); bCNC is an alternative with less evidence on this stack |

## Read the guide

**[docs/setup-guide.md](docs/setup-guide.md)** walks through, in order (a printable A4 copy, [docs/setup-guide.pdf](docs/setup-guide.pdf), is a snapshot of 26 Nov 2026; this repository has the latest):

0. Building the Raspberry Pi image, first boot and installing gSender (after a short overview of the CAD, CAM and G-code process)
1. Building grblHAL in the Web Builder and flashing it onto the SKR Pico
2. Connecting and verifying the board
3. Working out motor wiring, then motor current, microsteps, steps/mm and motion settings
4. Limit switches, probe and spindle
5. Homing and soft limits
6. Pi, touchscreen and sender setup
7. Configuring Fusion (post processor) to write G-code for this mill
8. New and emerging open source CAD and CAM options (CamCraft, SOLIDcraft), and why to stay with Fusion for now

It also has a safety checklist and glossary written for teenagers and first-timers, a troubleshooting table, and a full references section.

## Status and honesty notes

This is a research-based guide, not a record of a finished, tested build. Parts that could not be verified are listed in the "Not verified" list in the guide's "References and other supporting documentation" section. The main ones:

- The SKR Pico board map's exact pins (limits, probe, spindle PWM) were not read directly. Use the `$PINS` command and BTT's pinout PDF on your own board.
- The Web Builder shows the Trinamic driver locked to TMC2209 for this board, but the guide has not been tested on a real board. After flashing, `$$` must list `$140`–`$143` and `$150`–`$153`.
- Powering the Pi from the board's 5 V output while USB is also connected is not confirmed safe.
- Conservative speeds, the 0.5 A starting current and the E-stop advice are recommendations, not vendor figures.

Known SKR Pico issues are covered in the guide: start-up current ([grblHAL/RP2040 issue 74](https://github.com/grblHAL/RP2040/issues/74)) and the Hotwire-variant UART address swap ([PR 204](https://github.com/grblHAL/RP2040/pull/204)).

## Safety

This project involves a spinning cutter, motor power supplies and possibly mains-powered equipment. Fit an emergency stop, test with the spindle off first, and have an adult supervise if you are under 18.

## Third-party material and acknowledgements

The board pin diagram is © BIGTREETECH and is not reproduced here. Read it at the source: [BTT SKR Pico V1.0 pinout PDF](https://github.com/bigtreetech/SKR-Pico/blob/3fa005258baeb2ce4d6e629921c4474612d13d29/Hardware/BTT%20SKR%20Pico%20V1.0-PIN.pdf) (from [bigtreetech/SKR-Pico](https://github.com/bigtreetech/SKR-Pico), commit `3fa0052`). That repository has no licence file, so this project links to it rather than redistributing a copy. Thanks to BIGTREETECH, the grblHAL maintainers, Sienci Labs (gSender) and the bCNC authors for their work.

## Contributing

Corrections are welcome, especially verified pin assignments, confirmed Web Builder options, and bCNC-on-Pi install steps. Please cite a source or say how you tested it.

## Licence

The guide and README text are licensed under [Creative Commons Attribution-ShareAlike 4.0 International (CC BY-SA 4.0)](LICENSE). You may share and adapt them, including commercially, if you credit this repository and release your adaptations under the same licence.

This licence covers the documentation only. It does not apply to the upstream projects this guide describes (grblHAL, gSender, bCNC, Raspberry Pi OS and others), which keep their own licences. Any code, configuration or firmware files added to this repository later (for example modified grblHAL build settings) will be licensed separately, and code derived from GPL-licensed projects must keep that licence (check each upstream repository's own licence file).
