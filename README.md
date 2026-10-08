# STM32 Drone Flight Controller PCB

A custom flight-computer PCB for an FPV/GPS drone, designed in **KiCad 9**. It is built around an STM32 microcontroller with an IMU, barometer, GNSS receiver and a LoRa radio on one board.

> **Inspiration & credit:** This design is adapted from the open-source
> [FPV-Drone-STM32F411/DroneController](https://github.com/FPV-Drone-STM32F411/DroneController)
> by **Ammar Mahmood ([@ammarjmahmood](https://github.com/ammarjmahmood))** and **esb8 ([@esb8](https://github.com/esb8))**.
> Huge thanks to them for publishing their work openly. Their schematics and layouts were the starting point for this board.

## Overview

The original project uses a 5x5 STM32F411 flight computer and a separate power/ESC board. This repository is my modified version of the flight computer. I kept the overall architecture (STM32 + SPI sensors + GPS + LoRa) and changed several key parts. I also redid the RF section of the LoRa link.

**Status:** PCB design complete, firmware bring-up `[UPDATE: in progress / tested]`

## Hardware Details

| Block | Part |
|---|---|
| Microcontroller | STM32 `[CONFIRM exact part, e.g. STM32F411CEU6]` |
| IMU (gyro + accel) | TDK InvenSense **ICM-42688-P** |
| Barometer | BMP280 `[CONFIRM]` |
| GNSS | u-blox **SAM-M8Q** |
| LoRa transceiver | Semtech **SX1276** (discrete) + 32 MHz crystal |
| Antenna | Edge connector `J4` (`J_LORA_ANT`) for an external LoRa antenna |
| Main crystal | `[CONFIRM frequency]` |
| Programming / debug | SWD header (`J2`), switches `SW1`/`SW2`, header `J6` |
| Power | 3.3 V rail |

**Board stats (KiCad):** 293 pads, 44 vias, 765 track segments, 75 nets.

## What's Different From the Original

| | Original | This Board |
|---|---|---|
| IMU | ICM-42605 | **ICM-42688-P**: lower noise and better performance for flight stabilization |
| GPS | Quectel L86-M33 | **u-blox SAM-M8Q**: compact module with integrated patch antenna |
| LoRa | NiceRF SX1276 module | **Discrete SX1276 IC** with dedicated 32 MHz crystal and on-board antenna connector, so the RF section is designed into the PCB |
| Debug | UART, SWD, SPI test pads | SWD header, switches and a breakout header |
| Scope | Flight computer + power/ESC board | Flight computer only `[UPDATE if you add more boards]` |

What stayed the same: the STM32-based architecture, SPI-connected sensors, onboard barometer, and the GPS + LoRa combination.

## Repository Structure

```
KiCad/      schematic and PCB files
Docs/       screenshots and notes
Firmware/   [coming soon]
```

## Tools Used

KiCad 9, STM32CubeIDE `[CONFIRM]`, VS Code

## Media

`[Add: PCB layout screenshot, 3D render, schematic, photos of the assembled board]`

## Roadmap

- [ ] Fix remaining unrouted net / pass DRC
- [ ] Order and assemble the board
- [ ] Sensor bring-up (IMU, barometer, GPS)
- [ ] LoRa link test
- [ ] Flight firmware and tuning

## License

Released under the **MIT License**. This work is derived from
[FPV-Drone-STM32F411/DroneController](https://github.com/FPV-Drone-STM32F411/DroneController)
(MIT), and attribution to the original authors is retained as required.

## Acknowledgements

- [FPV-Drone-STM32F411/DroneController](https://github.com/FPV-Drone-STM32F411/DroneController): original design
- [Ammar Mahmood](https://github.com/ammarjmahmood) and [esb8](https://github.com/esb8)
- [STM32F4 Reference Manual (RM0383)](https://www.st.com/resource/en/reference_manual/rm0383-stm32f411xce-advanced-armbased-32bit-mcus-stmicroelectronics.pdf)

## Author

**Prathamesh Galphade**: [github.com/PageNotFound222](https://github.com/PageNotFound222)
