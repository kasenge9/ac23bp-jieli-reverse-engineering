# PCB Tracing Guide

PCB tracing is the process of reconstructing electrical connections from the physical circuit board.

## Basic Technique

1. Photograph the PCB.
2. Identify the IC orientation.
3. Number the IC pins.
4. Identify visible traces.
5. Use continuity mode to confirm connections.
6. Record each connection.
7. Draw the reconstructed circuit.
8. Verify the circuit using measurements.

## Example

If an IC pin has continuity to a USB D+ trace:

```
IC Pin X
   │
   └──────── USB D+
```

This becomes a documented connection.

## Important

A visible trace does not automatically tell us what signal it carries.

A signal must be identified from:

- circuit topology
- datasheet information
- electrical measurements
- observed behavior

## Recommended Trace Record

| Pin | Connected To | Measurement | Status |
|---|---|---|---|
| X | Crystal | Continuity | Confirmed |
| X | GND | Continuity | Confirmed |
| X | Test pad TP1 | Continuity | Confirmed |
| X | Unknown | — | Unknown |

## Safety First

Never test continuity on a powered device.

Always disconnect power before measuring.

Do not connect test probes to an IC unless you know what the pad is.
