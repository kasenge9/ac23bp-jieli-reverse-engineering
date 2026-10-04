# Bluetooth Analysis

The Bluetooth subsystem is one of the main areas of investigation.

## Device Identity

Current observed Bluetooth name:

```
CHARGE MINI
```

Bluetooth address:

```
93:91:63:79:F6:A9
```

## Questions

- What Bluetooth version is implemented?
- Which Bluetooth profiles are supported?
- What audio codec is used?
- What advertising information is transmitted?
- How does pairing work?
- Where is the device name stored?
- Is the Bluetooth address stored separately?
- What happens when the device is reset?
- Does the name survive power loss?

## Important Observation

Changing the Windows-side cached Bluetooth name did not change the name broadcast by the speaker.

This suggests that the device itself controls the advertised identity.

Further investigation is required to determine whether the name is stored in:

- firmware
- configuration data
- nonvolatile parameters
- a dedicated Bluetooth configuration structure
- another storage mechanism

## Bluetooth Profile Detection

Connecting to the device from different devices (Windows, macOS, iOS, Android) may reveal different perceived capabilities.

Record:

- Device class reported to host
- Supported profiles
- Available services
- Advertising flags

## Bluetooth MAC Address

The MAC address should remain consistent across power cycles, unless the device explicitly resets it.

If the MAC changes, it may indicate:

- firmware corruption
- a random MAC generation feature
- a deliberate randomization for privacy
- or reset behavior
