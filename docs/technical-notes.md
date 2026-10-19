# Technical state of the preserved firmware

The firmware was copied without functional changes from an original CubeIDE project snapshot. The observations below describe this archive; they do not reconstruct a past development process or establish medical validity.

## Actual processing chain

In `Core/Src/main.c`, `HAL_ADC_ConvCpltCallback()` calls:

```text
ADC → remove_spikes → remove_outliers → low_pass_filter
    → moving_average_filter → square_signal
    → conversion to uint16_t → detect_r_peak → UART
```

- ADC: 12-bit resolution, channel 15, software start and continuous mode, no DMA.
- Low-pass filter: integer recurrence `y = (15 × previous_y + x) / 16`.
- Moving average: 25-sample window.
- Detection: input above 45000, a latch and refractory counter; the derivative sign change participates in rearming.
- Interval: TIM7 uses a prescaler of 8000 from an 80 MHz clock, giving 10000 ticks/s. The code calculates `bpm = 600000 / counter`.
- Display: BPM text updated every three detections. The MAX7219 driver uses timer 16 for scrolling.
- Serial output: one processed signal value per line, transmitted synchronously from the ADC interrupt.

## Differences from the report

| Report description | Preserved implementation |
| --- | --- |
| Second-order Butterworth at 10 Hz with 1 kHz sampling | First-order recursive low-pass filter and moving average; acquisition is not triggered by a periodic timer |
| Peak detected through a threshold and a positive-to-negative derivative transition | Threshold-triggered detection; derivative used for rearming |
| Transmission of the signal and heart rate | Serial callback sends only the processed signal |
| OLED display mentioned in some headings | Final demonstrator documented with a commercial MAX7219 LED matrix |

The MATLAB file `docs/design/filtre.fda` is preserved as a design document. Its presence does not prove that the corresponding coefficients are implemented in this firmware.

## Limitations to address before robust reuse

1. **Squaring and numeric width.** `square_signal()` returns a 32-bit integer that is then assigned to `uint16_t`. Values above 65535 are truncated, which can alter the signal shape and detection.
2. **Spike rejection.** `remove_spikes()` assigns `previous_value = input` before returning that same value when the variation threshold is exceeded. This path therefore does not reject the spike as its name suggests.
3. **Shared state.** `previous_value` is used both inside `remove_spikes()` and to calculate the processed signal derivative. It does not simply represent the previous filtered sample.
4. **Outlier bounds.** Floating-point bounds are converted to unsigned integers without explicit saturation; a negative lower bound needs handling before conversion.
5. **Timing and refractory period.** The refractory counter is decremented per ADC callback, not per millisecond. Its duration depends on the effective throughput, which is affected by computation and blocking UART transmission.
6. **Heart-rate calculation.** The TIM7 counter may be zero or wrap; division is unprotected and the interval is not averaged. BPM limit macros do not filter this result in the inspected execution path.
7. **Unused configuration.** TIM15 is initialised for capture but is not used in the main processing path. The driver overwrites some SPI and TIM16 settings.

These observations were documented during portfolio preparation. No algorithmic correction was applied: any improved version should use commits dated when the changes are actually made and undergo separate testing.
