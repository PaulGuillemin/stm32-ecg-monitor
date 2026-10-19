# Repository provenance and verification

## Timeline and sources

- Academic project: 2025, with report and presentation dated 27 March 2025.
- Portfolio preparation: 19 October 2026.
- Report authors: Paul Guillemin and Tristan Lecourtois.

Two firmware copies were compared: `ECG_backup` in the academic folder and `ECG` in the former STM32CubeIDE 1.17.0 workspace. The latter was selected because it includes signal squaring and BPM text updates, whereas the academic copy contains a fixed `BPM 120` message and a commented-out update call.

This selection does not prove that the chosen copy is exactly the one used for the project presentation. It is the most representative working snapshot found during this preparation. Differences between the report and code are recorded in the [technical notes](technical-notes.md).

## File selection

The `Core/` and `Drivers/` sources, `.project` and `.cproject` project files, `ECG.ioc` configuration and linker script were retained. Tracked sources and configuration were compared with the originals using SHA-256 after compilation to check for automatic IDE changes.

Old binaries, object files, personal launch configurations, local settings, caches and unrelated course materials are excluded from the history. Selected original project documents are copied without rewriting. Preserved figures are neither generated images nor new acquisitions.

Selected photos and captures come from the ECG folder. `led-display.jpg` was extracted from Figure 18 on page 16 of the report. README images illustrate the 2025 project; acquisition windows and conditions are not sufficiently established to calculate a quantitative signal-to-noise comparison.

The original French reports, slides, captures and source comments remain historical archives. Portfolio documentation and diagram labels written for this repository are in English.

## Build performed

| Check | Result |
| --- | --- |
| Import into a separate CubeIDE workspace | Project `ECG` recognised |
| Configuration | Debug |
| IDE | STM32CubeIDE 1.19.0 |
| Compiler | GNU Tools for STM32 13.3.rel1 |
| Target | STM32L432KCUx, Cortex-M4F, hardware floating-point ABI |
| Compilation and linking | Successful, 0 errors and 0 warnings |
| Generated binary | `ECG.elf` |
| `arm-none-eabi-size` summary | text: 32348, data: 576, bss: 2720 bytes |

These figures come from the Debug build on 19 October 2026. `text`, `data` and `bss` are the tool's output categories, not measurements of power consumption or maximum runtime stack usage. Build outputs are excluded from the repository.

## Documentation checks

Project files were read, local README and documentation links checked, selected images inspected and third-party notices retained. A targeted search for credentials, keys and personal paths was performed on the selected files. It does not guarantee that all sensitive information has been detected.

## Checks not performed

- No flashing or testing on a physical board.
- No new ECG acquisition.
- No new measurement of sampling rate, detection accuracy or power consumption.
- No certification or clinical validation.

The successful build verifies that the archive can be compiled and linked, not its hardware or numerical behaviour. This repository presents past work and its known limitations.
