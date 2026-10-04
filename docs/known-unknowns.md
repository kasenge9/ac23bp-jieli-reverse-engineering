# Known Unknowns

These are questions that have not yet been conclusively answered.

The purpose of this document is to maintain an explicit list of what we do and do not know, preventing false confidence in unverified claims.

---

## Silicon Identification

### Status: **HYPOTHESIS**

- **What exactly does AC23BP mean?**
  - Marking observed: `AC23BP`
  - Interpretation: Possibly Jieli part code or manufacturing marking
  - Confidence: LOW
  - Evidence needed: Jieli datasheet or manufacturer documentation

- **What exactly does 1K966 represent?**
  - Marking observed: `1K966`
  - Interpretation: Unknown (possible lot code, die revision, or manufacturing batch)
  - Confidence: LOW
  - Evidence needed: Manufacturing documentation

- **Does 65E4 definitively identify AC6965E?**
  - Marking observed: `65E4`
  - Interpretation: Possibly encoding the silicon family (AC6965E) and flash size (4 Mbit)
  - Confidence: MEDIUM
  - Evidence needed: Physical pin mapping matching AC6965E datasheet

- **Does the final "4" represent 4 Mbit Flash?**
  - Marking observed: `...65E4`
  - Interpretation: Possibly 4 Mbit = 512 KB Flash
  - Confidence: LOW
  - Evidence needed: Successful firmware extraction and size measurement

### Path to Verification

1. Compare physical pinout with AC6965E datasheet.
2. Locate and extract firmware to verify size.
3. Contact Jieli or authorized distributors for marking documentation.

---

## Hardware Architecture

### Status: **PARTIALLY CONFIRMED**

- **Exact AC23BP pinout?**
  - Current status: UNKNOWN
  - Method to verify: PCB continuity testing + board tracing
  - Risk: Damaging pins if probed incorrectly

- **Exact package type?**
  - Observed: Small IC near antenna, appears to be BGA or QFN
  - Confidence: MEDIUM (visual inspection only)
  - Method to verify: Careful measurement, possible X-ray imaging

- **Internal versus external Flash?**
  - Current observation: No visible external Flash chip on PCB
  - Inference: Flash likely integrated into SoC
  - Confidence: MEDIUM
  - Method to verify: Complete PCB tracing

- **Exact audio architecture?**
  - Current observation: Audio lines visible toward amplifier
  - Unanswered: Is there an external DAC? Audio codec? DSP?
  - Method to verify: Trace audio path from SoC to amplifier

- **Exact amplifier interface?**
  - Current observation: Audio lines connect to amplifier section
  - Unanswered: Single-ended? Differential? I2S? Analog?
  - Method to verify: Oscilloscope measurement

- **Power supply topology?**
  - Current observation: Micro-USB power input visible
  - Unanswered: Is there a voltage regulator? Battery charging circuit? Multiple rails?
  - Method to verify: Voltage measurement + regulator IC identification

- **Crystal connections?**
  - Observation: 24 MHz crystal present
  - Unanswered: Which SoC pins? Load capacitors? Series resistance?
  - Method to verify: Continuity testing

---

## Firmware

### Status: **NO ATTEMPTS YET**

- **Can firmware be extracted?**
  - Current: Unknown
  - Depends on: Identifying a programming interface (UART, USB, JTAG, proprietary)
  - Risk: Bricking the device if wrong interface is probed

- **What format is the firmware?**
  - Current: Unknown
  - Possibilities: Raw binary, ELF, Intel HEX, vendor-specific compressed, encrypted
  - Method to verify: Extract and analyze

- **Where is the Bluetooth name stored?**
  - Current hypothesis: Firmware or configuration data
  - Possibilities:
    - Hardcoded in firmware
    - Configuration block in Flash
    - EEPROM
    - Runtime variable
  - Method to verify: Firmware analysis

- **Where is the Bluetooth MAC address stored?**
  - Current hypothesis: Firmware, configuration, or factory-programmed region
  - Possibilities: Same as device name storage
  - Method to verify: Firmware analysis or direct memory inspection

- **Is there a bootloader?**
  - Current: Unknown
  - Significance: May provide firmware programming interface
  - Method to verify: Firmware extraction and analysis

- **How does firmware update occur?**
  - Current: Unknown
  - Possibilities:
    - USB firmware upload
    - Serial/UART upload
    - Over-the-air via Bluetooth
    - Not upgradeable
  - Method to verify: Attempt update via available interfaces

---

## Programming and Debug Interfaces

### Status: **NOT PROBED**

- **Are programming pads present?**
  - Visual inspection: Some unidentified pads visible on PCB
  - Current: Unknown
  - Safety: Do not probe unknown pads without identification
  - Method to verify: Continuity testing to known points

- **Which protocol do they use?**
  - Current: Unknown
  - Possibilities: UART, JTAG, SWD, Jieli-specific
  - Method to verify: Interface identification + signal measurement

- **Is UART available?**
  - Likelihood: MEDIUM (common on Jieli designs)
  - Current: Not yet identified
  - Method to verify: Look for RX/TX traces or test pads

- **Is USB capable of firmware programming?**
  - Likelihood: MEDIUM
  - Current: Only confirmed for media playback
  - Method to verify: Attempt direct USB communication

- **What Jieli development/programming tools are applicable?**
  - Current: Unknown
  - Significance: May enable safe firmware extraction and modification
  - Method to verify: Research Jieli AC6965E documentation

---

## Bluetooth Subsystem

### Status: **PARTIALLY CONFIRMED**

- **Exact Bluetooth version?**
  - Datasheet expectation (AC6965E): Bluetooth 5.1
  - Observed: Cannot determine without protocol analysis
  - Method to verify: Bluetooth sniffing or firmware analysis

- **Supported profiles?**
  - Current observation: Device pairs and streams audio
  - Unanswered: Which profiles exactly? (A2DP, AVRCP, HFP, etc.)
  - Method to verify: Bluetooth sniffing or host-side interrogation

- **Audio codec?**
  - Possibilities: SBC (default), AAC, aptX, LDAC, etc.
  - Current: Unknown
  - Method to verify: Audio packet analysis or firmware inspection

- **Advertising structure?**
  - Current: Broadcasts as generic audio device
  - Unanswered: Specific advertising format? Manufacturer data? Service UUIDs?
  - Method to verify: Bluetooth sniffer capture

- **Pairing mechanism?**
  - Observation: Device can be paired with Windows
  - Unanswered: PIN required? Just Works? Specific flow?
  - Method to verify: Observe pairing process with sniffer

---

## System Behavior

### Status: **BASIC TESTS ONLY**

- **Power consumption (idle)?**
  - Current: Not measured
  - Method: Ammeter measurement

- **Power consumption (Bluetooth connected)?**
  - Current: Not measured
  - Method: Ammeter measurement

- **Power consumption (playing audio)?**
  - Current: Not measured
  - Method: Ammeter measurement

- **Standby behavior?**
  - Current: Not tested
  - Method: Power measurements and visual inspection

- **Reset behavior?**
  - Current: Not tested
  - Does Bluetooth name persist after power loss?
  - Method: Power cycle testing

---

## Summary Table

| Category | Item | Status | Confidence | Evidence Level |
|----------|------|--------|------------|----------------|
| Silicon | AC23BP meaning | UNKNOWN | LOW | Marking only |
| Silicon | 1K966 meaning | UNKNOWN | LOW | Marking only |
| Silicon | 65E4 = AC6965E | HYPOTHESIS | MEDIUM | Circumstantial |
| Hardware | IC pinout | UNKNOWN | — | Not probed |
| Hardware | Package type | UNKNOWN | MEDIUM | Visual only |
| Hardware | Internal Flash | PROBABLE | MEDIUM | No external chip seen |
| Firmware | Extraction possible | UNKNOWN | — | No interface found |
| Firmware | Device name storage | PROBABLE | MEDIUM | Windows behavior |
| Bluetooth | Version | UNKNOWN | — | Not determined |
| Bluetooth | Profiles | PROBABLE | MEDIUM | Audio observed |
| System | Power consumption | UNKNOWN | — | Not measured |

---

## Next Actions

1. **Immediate (Visual/Non-Invasive):**
   - Complete board photography
   - Detailed component identification
   - Document all visible markings

2. **Short-term (Measurement-Based):**
   - Power supply voltage mapping
   - Crystal connectivity verification
   - USB and TF interface confirmation

3. **Medium-term (Probing Required):**
   - Identify and safely probe test pads
   - Trace audio and RF paths
   - Measure signals

4. **Long-term (Advanced):**
   - Identify programming interface
   - Extract and analyze firmware
   - Modify and test device behavior

---

## Confidence Grading System

- **HIGH**: Directly measured, independently verified, or stated in datasheet
- **MEDIUM**: Multiple supporting observations, reasonable inference from known designs
- **LOW**: Single observation, possible but unconfirmed
- **UNKNOWN**: No data available

---

## Project Philosophy on Unknowns

Unknowns are not failures. They define the scope of the investigation.

Every unknown should be classified as such explicitly and marked with a clear path to verification.

The project succeeds not by claiming to know everything, but by:

1. Documenting what is known
2. Acknowledging what is not
3. Designing experiments to convert unknowns into confirmed knowledge
