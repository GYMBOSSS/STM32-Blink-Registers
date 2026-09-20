# STM32-Blink-Registers

Blink for STM32F103RB (Nucleo-F103RB), written with direct register access — no HAL, no CubeMX code. Built with GCC + CMake, flashed with OpenOCD.

This is my first learning project: I wanted to understand how an MCU works underneath HAL, and get hands-on with the GCC / CMake / OpenOCD toolchain.

## Hardware

- Board: NUCLEO-F103RB
- LED: LD2, connected to PA5

## Requirements

- [ARM GNU Toolchain](https://developer.arm.com/downloads/-/arm-gnu-toolchain-downloads) (`arm-none-eabi-gcc` in PATH)
- CMake ≥ 3.22
- GNU Make (e.g. via MinGW)
- [OpenOCD](https://openocd.org/) with ST-LINK support
- ST-LINK USB driver (Windows)

## Build

```bash
cmake -S . -B build -G "MinGW Makefiles"
cmake --build build
```

Produces `build/blink_registers.hex` and `build/blink_registers.bin`.

## Flash

```bash
openocd -f flash.cfg -c "program build/blink_registers.hex verify reset exit"
```

After reset, LD2 (PA5) should start blinking.

## Project layout

- `src/main.c` — main application, direct register access
- `src/system_stm32f1xx.c` — CMSIS system init
- `startup/startup_stm32f103xb.s` — startup code (GCC syntax)
- `linker/STM32F103RBTx_FLASH.ld` — linker script
- `CMSIS/` — CMSIS core and device headers
- `flash.cfg` — OpenOCD config
