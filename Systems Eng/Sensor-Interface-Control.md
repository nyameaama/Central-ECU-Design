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

The Type K metal runs to the PCB socket. The thermocouples bypass the copper AMPSEAL contacts. Each MAX31856 sits beside its socket and away from regulators and valve switches.

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
| Pressure outputs | 20 |
| AST20PT temperature outputs | 20 |
| Sensor power | 2 |
| Sensor returns | 2 |
| Spare | 26 |
| Total | 70 |

Split power and ground between the two harness branches. Total AST20PT current is 200 mA.

## Main-valve position feedback

The Buschjost `1E` switch is a three-wire PNP normally-open inductive sensor. It runs from 10 to 30 VDC, draws less than 10 mA at 24 V, and can source 100 mA. Run the two switches from `SENS_12V` so closed-valve feedback stays alive on backup power. A protected divider and Schmitt input bring each 12 V PNP signal into the AM2634. It does not connect straight to a 3.3 V pin.

The valve order sets the sensed end position. Use closed indication for MFV and MOV. The supplied switch lead is 2 m, three-pole, with an LED.

## Layout notes

- sensor ground meets valve return at the power entry point
- AST20PT supply is 10 to 28 V
- the 1 to 5 V outputs are scaled before the 3.3 V ADC
- sensor sample rate is 400 Hz max
- analog filter corner follows the selected sample rate
