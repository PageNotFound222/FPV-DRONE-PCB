# STM32 Drone Flight Controller PCB

A two-board flight-computer design for an FPV/GPS drone, designed entirely in **KiCad**. It is built around the **STM32F401CCU6** and combines an IMU, barometer, GNSS receiver and LoRa radio on the flight board, plus a separate STM32 board that handles USB-C, 3.3 V regulation and programming/breakout headers.

> **Inspiration & credit:** This design is adapted from the open-source
> [FPV-Drone-STM32F411/DroneController](https://github.com/FPV-Drone-STM32F411/DroneController)
> by **Ammar Mahmood ([@ammarjmahmood](https://github.com/ammarjmahmood))** and **esb8 ([@esb8](https://github.com/esb8))**.
> Huge thanks to them for publishing their work openly. Their schematics and layouts were the starting point for this project.

## Overview

The original project uses an STM32F411 flight computer plus a separate power/ESC board. This repository is my modified version. I kept the overall architecture (STM32 + SPI sensors + GPS + LoRa) and changed the MCU, IMU, GPS and LoRa implementation, and split the USB/power circuitry onto its own board.

**Status:** Schematics and PCB layouts complete for both boards, with DRC passing.

## Boards

### Board 1: STM32 MCU Board

| Block | Part |
|---|---|
| Microcontroller | STM32F401CCU6 |
| Crystal | 12 MHz (8 pF load caps) |
| USB | USB-C receptacle (USB 2.0) with 5.1k CC resistors |
| Regulator | AMS1117-3.3 LDO (10 uF in, 22 uF out) |
| Programming / debug | 6-pin SWD header (SWDIO, SWCLK, GND, NRST, SWO) |
| Headers | UART (RX/TX), IMU SPI breakout (CS, SCK, MISO, MOSI), 3-pin SPI1 header |
| Controls | BOOT switch, RESET switch, status LED |

**Layout stats (KiCad):** 181 pads, 20 vias, 279 track segments, 49 nets.

### Board 2: Flight Board

| Block | Part |
|---|---|
| Microcontroller | STM32F401CCU6 |
| Crystal | 12 MHz (8 pF load caps) |
| IMU (gyro + accel) | TDK InvenSense **ICM-42688-P** |
| Barometer | BMP280 |
| GNSS | u-blox **SAM-M8Q** (UART) |
| LoRa transceiver | Semtech **SX1276** (discrete) + 32 MHz crystal |
| Antenna | Edge connector `J4` (`J_LORA_ANT`) |
| Programming / debug | 4-pin SWD header (3V3, SWDIO, SWCLK, GND), BOOT and RESET switches |
| Power | External 3.3 V |

**Layout stats (KiCad):** 293 pads, 44 vias, 765 track segments, 75 nets.

**Signals:** the schematic carries chip-select lines for the IMU (`IMU_CS`), barometer (`BARO_CS`) and LoRa radio (`LORA_CS`), plus `LORA_RESET` and `LORA_DIO0` for the SX1276 and a UART link to the GPS.

## What's Different From the Original

| | Original | This Design |
|---|---|---|
| MCU | STM32F411CEU6 | **STM32F401CCU6** |
| IMU | ICM-42605 | **ICM-42688-P**: lower noise and better performance for flight stabilization |
| GPS | Quectel L86-M33 | **u-blox SAM-M8Q**: compact module with integrated patch antenna |
| LoRa | NiceRF SX1276 module | **Discrete SX1276 IC** with its own 32 MHz crystal and an on-board antenna connector, so the RF section is designed into the PCB |
| USB / power | USB-C and LDO on the flight computer | Moved to a **separate STM32 board** (USB-C + AMS1117-3.3); the flight board takes external 3.3 V |
| Backup battery | Coin-cell port | No coin-cell holder on the flight board |
| Debug | UART, SWD, SPI test pads | Dedicated SWD headers, BOOT/RESET switches and breakout headers on both boards |
| Scope | Flight computer + power/ESC board | MCU/USB board + sensor flight board (no ESC board) |

What stayed the same: the STM32 architecture, SPI-connected sensors, onboard barometer, 12 MHz crystal, and the GPS + LoRa combination.

## Repository Structure

```
KiCad/
  MCU-Board/       schematic and PCB for Board 1
  Flight-Board/    schematic and PCB for Board 2
Docs/              screenshots and notes
```

## Tools Used

KiCad 10

## Media

`[Add: PCB layouts, schematics, 3D renders and photos of both boards]`

## Roadmap

- [x] Schematics and PCB layouts for both boards
- [x] DRC clean
- [ ] Fabricate and assemble both boards
- [ ] Sensor bring-up (IMU, barometer, GPS)
- [ ] LoRa link test
- [ ] Flight firmware

## License

Released under the **MIT License**. This work is derived from
[FPV-Drone-STM32F411/DroneController](https://github.com/FPV-Drone-STM32F411/DroneController)
(MIT), and attribution to the original authors is retained as required.

## Acknowledgements

- [FPV-Drone-STM32F411/DroneController](https://github.com/FPV-Drone-STM32F411/DroneController): original design
- [Ammar Mahmood](https://github.com/ammarjmahmood) and [esb8](https://github.com/esb8)
- [STM32F401 documentation (ST)](https://www.st.com/en/microcontrollers-microprocessors/stm32f401cc.html)

## Author

**Prathamesh Galphade**: [github.com/PageNotFound222](https://github.com/PageNotFound222)
