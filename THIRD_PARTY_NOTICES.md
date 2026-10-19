# Authors, components and provenance

## Student work

The March 2025 report and presentation are authored by **Paul Guillemin and Tristan Lecourtois**, Mines Saint-Étienne. Preparing this repository does not automatically place their joint work or reused technical documents under a new licence.

Paul Guillemin confirmed that both authors worked together on all tasks, sometimes developing two versions before selecting the better solution. The README presents this shared contribution separately from reused components.

## Software

| Component | Attribution in the sources | Preserved notice |
| --- | --- | --- |
| STM32L4 HAL | STMicroelectronics | `firmware/Drivers/STM32L4xx_HAL_Driver/LICENSE.txt` |
| CMSIS | ARM and contributors identified in the files | `firmware/Drivers/CMSIS/LICENSE.txt` |
| STM32L4 device support | STMicroelectronics | `firmware/Drivers/CMSIS/Device/ST/STM32L4xx/LICENSE.txt` |
| Generated initialisation code | STMicroelectronics, headers in `Core/` | Original notices retained |
| MAX7219 matrix driver | Khaled Magdy / DeepBlueMbedded, headers in `DOT_MATRIX.c`, `.h` and `cfg` files | Original attribution retained; no additional licence identified in these files |

This repository does not assign a new global licence to third-party files. Each component's own notices and conditions need consideration before any planned redistribution.

## Images and documents

- Signal captures, the fabrication photo and the carrier-board drawing come from the ECG project archives.
- `docs/media/led-display.jpg` was extracted from Figure 18 of the joint report.
- The report includes an ECG board schematic attributed to François Bernier / EMSE / ID-Fab. It is not claimed as an original design by Paul or Tristan.
- Bibliographic references and third-party illustrations retain their attribution in the original report.

Permission to publish joint work and third-party material must be established before public release. Possession of these files does not by itself authorise relicensing them.
