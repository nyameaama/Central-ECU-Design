# Power Budget

This is enough to start the power tree and valve drivers. The old all-fourteen-valves case is gone. The command validator limits energized valves to six.

## Valve loads

| Load | Current at 24 V | Power |
| --- | ---: | ---: |
| One Triton main valve | 0.28 A at 28 V | 7.84 W at the published point |
| One main-valve driver allocation | 0.50 A | 12.0 W |
| One V19800, 30 ohm coil | 1.04 A estimated | 25.0 W |
| One V19800 driver allocation | 1.30 A | 31.2 W |
| Four main valves plus two V19800 | 4.60 A allocated | 110.4 W |

The Triton main valves accept 18 to 36 VDC and draw 0.28 A at 28 VDC. A 24 V current figure is not published, so each driver carries a 0.50 A allocation. They are internally piloted from upstream propellant pressure and need no actuation-air circuit.

The Buschjost backup is a different electrical load. Each fuel valve is 44 W, or 1.83 A at 24 V. Each oxidizer valve is 53 W, or 2.21 A. Allocate 2.5 A fuel channels and 3 A oxidizer channels. The existing 0.5 A Triton channels cannot run them.

Valcor publishes 1.3 A at 30 V and 70 F for the V19800 30 ohm coil. Scaling that measurement to 24 V gives 1.04 A. I am still rating every small-valve channel for 1.3 A because coil tolerance, cold copper, wiring, and supply tolerance all push current upward.

The new valve list has fourteen outputs. The allowed ignition state is six valves: both inlet isolation valves, MFV, MOV, IFV, and IOV. Normal main-engine operation is four valves. The old Converse firmware only had six valve IDs and reached five simultaneously during purge. Six is now the software limit. The hardware still gets fourteen independently protected drivers.

The command validator rejects any purge, bleed, dump, or chilldown command that would energize a seventh valve.

## Electronics loads

| Load | Power |
| --- | ---: |
| 32 AST20PT sensors | 3.84 W |
| AM2634 | 1.46 W |
| Analog supplies and rail monitoring | 5.00 W |
| CAN and service interface | 1.25 W |
| Eight MAX31856 parts | 0.053 W |
| Logic and small-load margin | 0.99 W |
| Critical electronics total | 12.6 W |

The Buschjost backup adds four position switches. Powered from the backed-up 12 V sensor rail, they add less than 0.48 W total. The alternate critical load is 13.1 W.

TI's published AM2634 traction-inverter case is 1.042 W at 85 C junction, 1.120 W at 105 C, 1.232 W at 125 C, and 1.460 W at 150 C. I used 1.46 W. The estimator is a power estimate; the rail limits remain 2.5 A for the 1.2 V core and RAM group, 200 mA for 3.3 V digital I/O, and 100 mA for 3.3 V analog.

The AM263x supply is `TPS6538600QDCARQ1` plus a `TPS62903-Q1` 3 A core buck. The old 3.3 V, 1.5 A placeholder is retired.

## 48 V input cases

| State | Load after conversion | Approximate 48 V input | With 25% margin |
| --- | ---: | ---: | ---: |
| Safe, valves off | 12.6 W | 0.31 A | 0.39 A |
| Main engine, four main valves | 60.6 W | 1.36 A | 1.70 A |
| Ignition, four main valves plus two small valves | 123.0 W | 2.73 A | 3.42 A |

Buschjost backup case:

| State | Load after conversion | Approximate 48 V input | With 25% margin |
| --- | ---: | ---: | ---: |
| Main engine, four backup main valves | 207.1 W | 4.57 A | 5.72 A |
| Ignition, four backup main valves plus two small valves | 269.5 W | 5.94 A | 7.43 A |

That alternate case uses the published 44 W and 53 W coil values. A protected 48 V, 8 A input covers it before igniter power. It leaves little room for cold-coil current, tolerance, or an igniter on the same feed.

The valve calculation uses 95% converter efficiency. The electronics calculation uses 85% from the 12 V bus through the downstream rails. The protected 48 V input rating is 7.5 A before the igniter load is added.

## Converters

The 24 V valve converter is TDK-Lambda `I7A4W033A033V-003-R`, set to 24 V. It accepts 18 to 60 V, is in production, and has plenty of transient headroom. TDK gives 97% typical efficiency at 48 V input, 24 V output, and full rated load. I used 95% in the budget because our valve load is well below its 500 W rating and because the published number is typical, not guaranteed.

There is no single actual efficiency before the PCB, airflow, input voltage, and load are fixed. Current budget values:

- 97% is the TDK datasheet point
- 95% is the valve-rail budget number
- 85% is the conservative battery-to-load number through the lower-voltage power tree
- hot and cold measurements close these values on the first assembled ECU

## Igniter

I could not find an off-the-shelf propane and nitrous-oxide rocket igniter with a released part number, pressure rating, interface, and input-current waveform that fits this engine. The igniter needs to be custom.

There is no released igniter current waveform yet. The old 12 V, 10 A entry stays outside the production power budget. The custom igniter release will add bus voltage, peak current, pulse width, repetition rate, and maximum ignition duration.

## Backup

Backup requirement: controller, CAN, and all measurement lines remain alive for 30 minutes after loss of 48 V at an ambient temperature down to -15 C. The backup does not power valves or ignition.

The critical electronics load is 12.6 W. At 85% conversion efficiency the battery supplies about 14.8 W, or roughly 1.23 A at 12 V. The CYCLON battery is rated 2.5 Ah at its ten-hour rate. Applying a 50% combined allowance for discharge rate, cold capacity, aging, state of charge, cutoff, and tolerance leaves about 15 Wh at the battery and 12.75 Wh at the electronics. That gives about 60 minutes calculated. The requirement stays at 30 minutes.

With the four Buschjost position switches powered during backup, the same calculation gives about 58 minutes.

EnerSys rates the CYCLON family down to -40 C. The ECU working minimum stays at -15 C until the rest of the power tree, sensors, connectors, and harness are checked at lower temperature. Qualification includes a fully instrumented 30-minute discharge after a -15 C cold soak.

Backup input still needs a 3 A path, fuse at the battery, service disconnect, reverse-polarity protection, automatic switchover, battery current and voltage measurement, and low-voltage cutoff.

## Sources

- [Valcor V19800 sheet](https://www.valcor.com/valcor-technical-datasheets/valcor_aircraft_V19800.pdf)
- [Triton TS-SP08B2000-0210](https://www.triton-space.com/products/valves/solenoid/ts-sp08b2000-0210)
- [Triton TS-SP16A2000-0200](https://www.triton-space.com/products/valves/solenoid/ts-sp16a2000-0200)
- [Buschjost 2/918 R272](https://www.buschjostventile.de/en/products/2-918-08-R-272)
- [Buschjost inductive position sensor](https://www.buschjostventile.de/api/attachments/93eacf07-07ba-4c1d-8806-5cbe40241766.pdf)
- [TI AM2634 datasheet](https://www.ti.com/lit/ds/symlink/am2634-q1.pdf)
- [TI AM263x power estimator note](https://www.ti.com/lit/an/sprad54/sprad54.pdf)
- [TI AM26x hardware design guide](https://www.ti.com/lit/an/sprabjl/sprabjl.pdf)
- [TDK-Lambda i7A specification](https://product.tdk.com/en/system/files?file=dam/doc/product/power/switching-power/dc-dc-converter/specification/i7a_spec.pdf)
- [EnerSys CYCLON product page](https://www.enersys.com/en-gb/products/batteries/cyclon/cyclon/)
