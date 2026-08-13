# Sensor Interfaces

## AST20PT

Each sensor has four wires:

| Signal | Connection |
| --- | --- |
| `SENS_12V` | Protected sensor power |
| `SENS_RTN` | Sensor ground back to the ECU |
| `P_OUT` | 1 to 5 V pressure signal |
| `T_OUT` | 1 to 5 V temperature signal |

Budget 10 mA per sensor. Both outputs need input protection, an RC filter, and scaling before a 3.3 V ADC. The valid signal range is 1 to 5 V, which leaves room to detect a broken or shorted wire.

Sensor-side connector is Phoenix `1681127`. It takes 18 to 24 AWG wire and 4 to 6 mm cable. Start with four-core shielded PUR cable. Ground the shield at the ECU end only unless EMC testing gives a reason to change it.

The remote chamber pressure sensor uses the same interface. The sense tube will slow the reading, so test the installed tube and restrictor before using that channel for control.

## Thermocouples

Probe is Omega `BLMI-XL-K-116U-6-CC`. Cable plug is `SMPW-K-M`. PCB socket is `PCC-SMP-K-5`.

Keep Type K metal all the way to the PCB socket. Do not run these signals through the copper AMPSEAL contacts. Put each MAX31856 close to its socket and away from regulators and valve switches.

I am laying out eight channels. Five are used and three are spare.

MAX31856 connections:

- 3.3 V
- SPI clock and data
- one chip select per channel
- `DRDY` and `FAULT` if there are enough MCU pins
- the input filter from the datasheet

## Main sensor connectors

Two 35-way AMPSEAL headers give 70 pins.

| Use | Pins |
| --- | ---: |
| Pressure outputs | 32 |
| AST20PT temperature outputs | 32 |
| Sensor power | 2 |
| Sensor returns | 2 |
| Spare | 2 |
| Total | 70 |

Split power and ground between the two harness branches. Total AST20PT current is only 320 mA, so connector current is not a problem.

## Layout notes

- keep sensor ground away from valve return until the power entry point
- AST20PT needs 10 to 28 V, not the 5 V rail
- do not connect a 1 to 5 V output straight to a 3.3 V ADC
- sensor sample rate is 400 Hz max
- set the analog filter from the real sample rate, not the 400 Hz headline number
