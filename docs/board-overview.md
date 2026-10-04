# Board Overview

This document provides a high-level architectural understanding of the AC23BP Bluetooth speaker module.

## Physical Description

The board is a small, compact PCB approximately 45 mm × 35 mm × 12 mm (estimated), designed to fit inside a portable speaker enclosure or robot unit.

### Key Physical Features

- **Single-layer or two-layer PCB** (observation)
- **Small profile**: suitable for embedded applications
- **Surface-mount components**: primarily SMD, no through-hole parts observed
- **Antenna**: PCB-integrated antenna for Bluetooth
- **Connectors**: Micro-USB (power), USB-A (media), microSD slot (media)

## System Architecture

The module is built around a single Jieli SoC (System-on-Chip) that serves as the central processor and Bluetooth radio.

### Block Diagram

```
        ┌─────────────────────────────┐
        │    Jieli AC6965E (SoC)      │
        │                             │
        │  • CPU/DSP                  │
        │  • Bluetooth Radio          │
        │  • Audio Processing         │
        │  • USB Controller           │
        │  • Memory Controller        │
        │  • Regulators               │
        │  • GPIO                     │
        │                             │
        └────────┬────────────────────┘
                 │
    ┌────────────┼────────────┬───────────┐
    │            │            │           │
    ▼            ▼            ▼           ▼
 ┌─────┐    ┌────────┐   ┌────────┐  ┌────────┐
 │ RF  │    │ Audio  │   │ USB    │  │ Power  │
 │ Ant │    │ L/R    │   │ Media  │  │ Input  │
 │     │    │ Amp    │   │ Ctrl   │  │ Regul. │
 └─────┘    └────────┘   └────────┘  └────────┘
    │            │           │           │
    ├────────────┼───────────┼───────────┤
    │            │           │           │
 Bluetooth   Speaker        Media      Power
```

## Functional Subsystems

### 1. Bluetooth Subsystem

- **Purpose**: Wireless audio reception and device discovery
- **Components**: SoC radio, antenna, RF matching network
- **Output**: Bluetooth audio stream to audio processor
- **Interfaces**: RF (wireless), optional UART for configuration
- **Status**: **CONFIRMED** operational

### 2. Audio Input Subsystem

Accepts audio from multiple sources:

- **Bluetooth**: via SoC radio
- **USB**: via USB interface and media controller
- **TF/microSD**: via microSD card reader interface
- **AUX**: optional analog line-in (if implemented)

**Purpose**: Multiplex multiple audio sources to a single audio processor.

### 3. Audio Processing Subsystem

- **Component**: Integrated in SoC or external audio codec
- **Functions**: 
  - Mixing/routing
  - Volume control
  - Equalization
  - Format conversion
  - PCM to analog conversion (DAC)
- **Output**: Analog left/right audio to amplifier

### 4. Amplifier and Speaker Subsystem

- **Purpose**: Amplify audio and drive speaker(s)
- **Input**: Analog left/right from audio processor
- **Output**: Amplified signal to physical speaker
- **Component**: Dedicated amplifier IC or amplifier section of SoC
- **Status**: **CONFIRMED** present, not yet characterized

### 5. Power Subsystem

- **Input**: Micro-USB 5 V power input (and possibly battery)
- **Regulation**: Linear or switching regulators to generate:
  - SoC core voltage (typically 1.8 V or 3.3 V)
  - Analog supply (typically 3.3 V)
  - Amplifier supply (typically 5 V or higher)
- **Status**: **NOT YET CHARACTERIZED**

### 6. Clock and Crystal Subsystem

- **Primary Clock**: 24.000 MHz crystal
- **Purpose**: SoC timing and Bluetooth clock accuracy
- **Status**: **CONFIRMED** 24 MHz crystal present
- **Verification**: Crystal connections to SoC not yet traced

### 7. Storage and Boot Subsystem

- **Program Memory**: Likely internal SoC Flash
- **Data Memory**: Likely SRAM + nonvolatile config storage
- **Purpose**: Hold firmware, configuration (including device name), calibration data
- **Status**: **HYPOTHESIS**, not yet verified
- **Critical for**: Understanding device identity storage

### 8. USB Interface Subsystem

- **Physical Port**: USB-A (host mode)
- **Purpose**: Connect USB flash drives for media playback
- **Current Status**: Functional, media playback confirmed
- **Interface Type**: Likely USB 2.0 Full-Speed or High-Speed

### 9. SD Card Interface Subsystem

- **Physical Port**: microSD/TF slot
- **Purpose**: Connect microSD cards for media playback
- **Current Status**: Likely functional (not yet tested)
- **Interface Type**: Likely SD/SDIO

## Power Flow

```
Micro-USB 5 V Input
        │
        ▼
   ┌─────────┐
   │Regulator│──────► SoC Core (1.8 V or 3.3 V)
   │  LDO    │──────► Analog (3.3 V)
   │         │──────► Amplifier (5 V or battery-derived)
   └─────────┘
        │
    Battery
    connector
    (if present)
```

## Data Flow

```
Bluetooth Input
    │
    ├─────────────┬──────────────┬────────────┐
    │             │              │            │
   USB Input     TF Input      AUX Input      │
    │             │              │            │
    └─────────────┼──────────────┼────────────┘
                  │
              ┌───▼────┐
              │  SoC   │
              │ Audio  │
              │Process│
              └───┬────┘
                  │
                  ▼
              ┌──────────┐
              │   DAC    │──► Analog L/R
              └──────────┘
                  │
                  ▼
              ┌──────────┐
              │Amplifier │
              └────┬─────┘
                   │
                   ▼
              Speaker/Headphones
```

## Expected Signal Voltages

| Rail | Expected Voltage | Purpose |
|------|------------------|---------|
| USB Input | 5 V | Power, media |
| SoC Core | 1.8–3.3 V | Main processor |
| Analog | 3.3–3.6 V | Bluetooth radio, audio codec |
| Amplifier | 5 V or higher | Speaker driver |
| Standby | ~0 V (or low) | Sleep mode |

## Expected Operating Modes

### Power Off
- Minimal current draw
- Device not discoverable

### Standby / Idle
- Bluetooth radio listening for advertisements
- Low power mode

### Pairing
- Bluetooth in discovery mode
- LED (if present) may indicate

### Connected (Bluetooth)
- Audio streamed over wireless
- Power consumption increased

### Playing (USB/TF)
- Media file read from storage
- Audio processed and played
- Power consumption increased

## Suspected Communication Interfaces

Based on common Jieli designs, the following interfaces may exist:

- **UART** (debug/bootloader) — likely on test pads
- **I2C** (for audio codec configuration, if external codec present)
- **GPIO** (for buttons, LEDs, amplifier control)
- **USB** (media playback)
- **SD/SDIO** (media playback)
- **SPI** (for Flash programming or external memory)

None of these have been confirmed yet.

## Next Steps

1. **Board Photography**: Capture detailed images of all sides
2. **Component Identification**: Mark and identify every component
3. **Tracing**: Follow each major signal (power, audio, USB, RF) across the board
4. **Voltage Mapping**: Measure DC levels at key points
5. **Interface Discovery**: Identify and safely probe test pads
6. **Firmware Analysis**: Extract and analyze if possible
