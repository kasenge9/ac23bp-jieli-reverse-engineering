# Reverse Engineering Methodology

## 1. Observe

Before touching the circuit, document what can already be seen.

Record:

- Component markings
- Connector types
- PCB markings
- Crystal frequency
- IC package
- Wire colors
- Test pads
- Antenna structure

Take photographs before modification.

---

## 2. Identify

Determine what each component is.

Do not identify a component based only on physical appearance.

Use:

- markings
- datasheets
- circuit topology
- package information
- measurements
- comparison with known designs

---

## 3. Hypothesize

Develop a possible explanation.

Example:

"The unconnected test pads may provide UART access."

This is a hypothesis, not a fact.

---

## 4. Test

Design a measurement that can distinguish between possibilities.

For example:

- continuity test
- resistance measurement
- voltage measurement
- oscilloscope
- logic analyzer

---

## 5. Record

Every measurement should include:

- Date
- Instrument
- Probe configuration
- Test point
- Power state
- Measured value
- Expected value
- Interpretation

---

## 6. Verify

A conclusion should ideally be confirmed using another method.

Example:

PCB trace → continuity measurement → schematic → signal measurement.

---

## 7. Document

Update the repository immediately after an important discovery.

Never rely on memory.
