# STM32 Real-Time Audio DSP Pipeline

## Overview
This repository contains the hardware schematics and compiled firmware binaries for an end-to-end real-time audio processing pipeline. 

The system captures analog audio via an electret microphone, amplifies it using a custom TL974 preamplifier circuit, digitizes and processes the signal using an **STM32 Nucleo-32** microcontroller, and finally outputs the reconstructed analog signal to a commercial audio amplifier module to drive a speaker.

## Hardware Architecture
The physical circuit is divided into four distinct stages:

1. **Microphone Bias Stage:** * Provides power to the electret microphone using the 3V3 rail through a 1kΩ bias resistor.
2. **Preamplifier Stage (TL974):** * AC-coupled input via a 1.69μF capacitor.
   * Utilizes a voltage divider (47kΩ resistors) for biasing, and a gain/filtering network. 
   * *Engineering Note:* While the initial theoretical calculations suggested specific filtering capacitor values (e.g., 220pF for C2), real-world prototype testing revealed that the preamplifier exacerbated high-frequency noise. The physical capacitor values were ultimately adjusted empirically to achieve the cleanest signal-to-noise ratio before feeding into the STM32 ADC.
3. **Digital Signal Processing (DSP) Stage:**
   * **STM32 Nucleo-32** handles the core processing.
   * The amplified analog signal is read by the internal ADC, processed by the ARM core, and outputted via the internal DAC.
4. **Output Stage:**
   * The DAC signal is fed into a commercial audio amplifier module (LM386 based).
   * The final amplified signal passes through a large **470μF DC-blocking capacitor**.

## Firmware Binaries (`.bin` files)
This repository includes two pre-compiled firmware files. You can flash them directly to the STM32 Nucleo board by dragging and dropping them into the `NODE_L432KC` mass storage drive.

* **`simple.bin`**: A lightweight, straightforward implementation.
* **`complex.bin`**: An advanced implementation featuring heavier Digital Signal Processing (DSP) algorithms running on the STM32 core to clean and manipulate the audio before sending it to the DAC.

## Circuit Schematic & Prototype
*Please refer to the repository resources for the complete system schematic diagram and the physical breadboard layout images.*
