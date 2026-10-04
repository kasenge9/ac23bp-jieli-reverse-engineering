# Firmware Analysis

## Goal

Understand how the firmware controls the hardware.

## Questions

- Where is program memory located?
- Is Flash integrated?
- Is there external Flash?
- Is a bootloader present?
- How does firmware update occur?
- How are configuration parameters stored?
- How is Bluetooth initialized?
- How are USB and TF playback handled?

## Firmware Preservation Rule

If firmware is ever successfully extracted:

1. Preserve the original dump.
2. Calculate a hash.
3. Never modify the original.
4. Make a working copy.
5. Record every modification.
6. Keep modified firmware in a separate directory.

Example structure:

```
firmware/
├── original/
│   └── ac23bp_original.bin
├── extracted/
│   └── ac23bp_extracted_2026-10-04.bin
├── modified/
│   └── ac23bp_modified_v1.bin
└── analysis/
    └── firmware-structure.md
```

The original firmware is the reference specimen.

## Firmware Extraction Methods

Possible methods depend on available hardware interfaces:

- UART bootloader
- USB firmware upload
- JTAG or SWD debug port
- Jieli-specific programming tool
- Flash chip direct extraction (if external)

## Firmware Structure Expectations

Embedded firmware typically contains:

- Boot vector
- Initialization code
- Main application loop
- Bluetooth stack
- Audio processing code
- Media library
- Parameter/configuration data
- Possibly padding or unused space

## Analysis Tools

Tools for firmware analysis:

- Hexdump / hex viewer
- Strings extraction
- Binary comparison
- Disassembler (if CPU instruction set is known)
- CRC/checksum calculators
- Pattern search

## Hash Calculation

When preserving firmware:

```bash
sha256sum ac23bp_original.bin > ac23bp_original.sha256
md5sum ac23bp_original.bin > ac23bp_original.md5
```

Always verify integrity on subsequent access.
