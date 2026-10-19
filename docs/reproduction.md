# Reproduce the build and prepare a bench test

## Environment

| Item | Identified value |
| --- | --- |
| Board declared in `ECG.ioc` | NUCLEO-L432KC |
| Microcontroller | STM32L432KCUx, Cortex-M4F |
| Configured system clock | 80 MHz |
| CubeMX version recorded in the project | 6.13.0 |
| Recorded STM32Cube package | STM32Cube FW_L4 V1.18.1 |
| IDE used for repository verification | STM32CubeIDE 1.19.0 on Windows |
| Compiler used | GNU Tools for STM32 13.3.rel1 |

The required HAL and CMSIS sources are included in `firmware/Drivers/`. Regenerating the project with CubeMX is not required to build the archive. Regeneration could alter code or configuration; use a separate copy if needed.

## Building in the IDE

1. Create or open a workspace separate from the original archives.
2. Import `firmware/` through **File → Import → General → Existing Projects into Workspace**.
3. Check that the imported project is named `ECG` and select the `Debug` configuration.
4. Run **Project → Build Project**.
5. Check that `firmware/Debug/ECG.elf` was generated and review the diagnostics.

Keep `.project`, `.cproject`, `ECG.ioc`, `Core/`, `Drivers/` and the linker script together inside `firmware/`. Generated `Debug/` outputs are excluded from Git.

## Headless build

STM32CubeIDE provides `headless-build.bat` on Windows. Using the paths to your installation and a dedicated workspace:

```powershell
& 'C:\ST\STM32CubeIDE_1.19.0\STM32CubeIDE\headless-build.bat' `
  -data "$env:TEMP\ecg-build-workspace" `
  -import (Join-Path $PWD 'firmware') `
  -build 'ECG/Debug'
```

Run this from the repository root and adjust the installation path. Check the build summary and generated ELF; the launcher's exit code alone does not establish success.

## Interfaces identified in the firmware

This table lists microcontroller pins, not connector numbers on a carrier board.

| Signal | MCU pin | Usage |
| --- | --- | --- |
| Conditioned analogue ECG input | PB0 / ADC1_IN15 | 12-bit ADC, continuous conversions |
| Detection indicator | PB1 | GPIO output named `LED` |
| USART1 TX | PA9 | Processed signal output to the computer |
| USART1 RX | PA10 | Pin configured for USART1 |
| SPI3 SCK | PB3 | Clock to MAX7219 |
| SPI3 MOSI | PB5 | Data to MAX7219 |
| Matrix chip select | PA15 | Driver configures this pin as a GPIO output |
| SWD | PA13 / PA14 | Programming and debugging |

USART1 is configured for **115200 baud, 8 data bits, no parity, 1 stop bit**, with no flow control. The firmware transmits one integer sample of the processed signal per line, terminated by `\r\n`. It does not transmit a second BPM column. PA2, labelled `VCP_TX`, belongs to USART2: its presence does not mean USART1 is automatically connected to the ST-Link virtual COM port.

The MAX7219 driver reconfigures SPI with a divider of 256 after generated initialisation. IOC settings alone therefore do not describe the complete runtime configuration. The report also describes TXS0108 logic-level translation between the STM32 and display driver.

## Preparing a bench test

No board was connected or flashed during repository preparation.

1. Retrieve the original analogue board wiring and power configuration; the pin table does not replace its electrical schematic.
2. Start with a bench signal source suitable for the ADC input, with no person connected, and check signal levels before connecting it. This repository does not provide a validated procedure for measurements on a person.
3. Program the board using an ST-Link configuration created for the hardware actually available.
4. Connect USART1 to a suitable serial interface, then configure SerialPlot for one ASCII numeric column at 115200 baud.
5. Check sample reception, display output and the detection indicator. Measure the actual acquisition interval and compare the estimated rate against the bench signal.

Do not claim detection accuracy, a 1 kHz sampling frequency or a 300 ms refractory period without measuring them: the archive does not guarantee these properties. Read the [technical notes](technical-notes.md) before testing the detector.
