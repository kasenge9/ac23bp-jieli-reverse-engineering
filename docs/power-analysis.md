# Power Analysis

Understanding the power system is one of the first priorities in reverse engineering.

## Questions

- What voltage enters the board?
- Where does USB 5 V go?
- Is there battery charging?
- What voltage powers the SoC?
- What voltage powers the amplifier?
- Is there a 3.3 V regulator?
- What happens during standby?
- What happens when Bluetooth connects?

## Measurements

Record:

- USB input voltage
- Battery voltage
- SoC supply voltage
- Regulator output
- Amplifier supply
- Standby current
- Operating current

## Measurement Table

| Condition | Voltage | Current | Notes |
|---|---:|---:|---|
| Powered off | | | |
| Bluetooth idle | | | |
| Bluetooth connected | | | |
| Audio playing | | | |
| USB playback | | | |
| TF playback | | | |

## Safety Notes

Never connect a power source to an unknown test point without first determining what that point is.

Always verify voltage at suspicious pads before connecting anything to them.

When measuring current, use the ammeter in series, not parallel.
