# Audio Analysis

The module supports multiple possible audio sources.

Conceptually:

```
Bluetooth ──┐
USB ────────┤
TF ─────────┤──► Audio Processing ──► DAC ──► L/R ──► Amplifier
AUX ────────┘
```

## Investigation

Identify:

- Bluetooth audio input
- USB audio/media path
- TF media path
- AUX path
- DAC output
- Amplifier input
- Speaker output

## Measurements

Where safe, investigate:

- DC level
- waveform
- frequency
- amplitude
- left/right separation

Oscilloscope measurements should be performed carefully around amplifier outputs because some amplifiers use bridge-tied-load outputs.

## Audio Path Tracing

Start with the known connectors:

1. Trace the audio output lines (L/R) from the SoC.
2. Identify any intermediate components (op-amps, filters, impedance matching).
3. Follow the signal to the amplifier input.
4. Trace the amplifier output to the speaker.

## Expected Audio Standards

For a Bluetooth speaker module, expect:

- I2S or PCM interface between SoC and DAC (if external)
- I2C control for audio codec configuration
- GPIO for audio routing selection
- analog left/right outputs
- possibly a speaker amplifier with bridge-tied-load topology

## Stereo Separation

Record whether the module maintains L/R separation or converts to mono.

This affects routing and debugging efforts.
