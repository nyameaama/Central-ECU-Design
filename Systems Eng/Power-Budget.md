# Power Budget

This is enough to start the power tree and valve drivers. I removed the old all-fourteen-valves case. The new command validator must make that state impossible.

## Valve loads

| Load | Current at 24 V | Power |
| --- | ---: | ---: |
| One V80000 | 1.10 A | 26.4 W |
| One V19800, 30 ohm coil | 1.04 A estimated | 25.0 W |
| One V19800 driver allocation | 1.30 A | 31.2 W |
| Four V80000 plus two V19800 | 7.00 A allocated | 168.0 W |

Valcor publishes 1.3 A at 30 V and 70 F for the V19800 30 ohm coil. Scaling that measurement to 24 V gives 1.04 A. I am still rating every small-valve channel for 1.3 A because coil tolerance, cold copper, wiring, and supply tolerance all push current upward.

The new valve list has fourteen outputs. The allowed ignition state is six valves: both inlet isolation valves, MFV, MOV, IFV, and IOV. Normal main-engine operation is four valves. The old Converse firmware only had six valve IDs and reached five simultaneously during purge. Six is now the software limit. The hardware still gets fourteen independently protected drivers.

This limit needs to live in the command validator. A purge, bleed, dump, or chilldown command must not be allowed to create a seventh energized valve.

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

TI's published AM2634 traction-inverter case is 1.042 W at 85 C junction, 1.120 W at 105 C, 1.232 W at 125 C, and 1.460 W at 150 C. I used 1.46 W. The TI estimator note says its result is not for regulator sizing. Size the AM2634 rails from the datasheet limits instead: 2.5 A for the 1.2 V core and RAM group, 200 mA for 3.3 V digital I/O, and 100 mA for 3.3 V analog.

The first board should follow TI's AM263x supply recommendation: `TPS6538600QDCARQ1` plus a `TPS62903-Q1` 3 A core buck. Do not put the whole MCU behind the old 3.3 V, 1.5 A placeholder.

## 48 V input cases

| State | Load after conversion | Approximate 48 V input | With 25% margin |
| --- | ---: | ---: | ---: |
| Safe, valves off | 12.6 W | 0.31 A | 0.39 A |
| Main engine, four large valves | 118.2 W | 2.63 A | 3.29 A |
| Ignition, four large plus two small valves | 180.6 W | 3.99 A | 4.99 A |

The valve calculation uses 95% converter efficiency. The electronics calculation uses 85% from the 12 V bus through the downstream rails. Use at least a 7.5 A protected 48 V input for the ECU before the igniter is added.

## Converters

The 24 V valve converter is TDK-Lambda `I7A4W033A033V-003-R`, set to 24 V. It accepts 18 to 60 V, is in production, and has plenty of transient headroom. TDK gives 97% typical efficiency at 48 V input, 24 V output, and full rated load. I used 95% in the budget because our 168 W valve state is well below its 500 W rating and because the published number is typical, not guaranteed.

There is no honest single "actual efficiency" before the PCB, airflow, input voltage, and load are fixed. For now use these numbers:

- 97% is the TDK datasheet point
- 95% is the valve-rail budget number
- 85% is the conservative battery-to-load number through the lower-voltage power tree
- measure all three hot and cold on the first assembled ECU

## Igniter

I could not find an off-the-shelf propane and nitrous-oxide rocket igniter with a released part number, pressure rating, interface, and input-current waveform that fits this engine. The igniter needs to be custom.

That means there is no defensible igniter current waveform to put in the power budget yet. Do not size a production switch around the old 12 V, 10 A placeholder. The custom igniter design needs to release its bus voltage, peak current, dwell or pulse width, repetition rate, and maximum ignition duration before that channel is frozen.

## Backup

Final working requirement: keep the controller, CAN, and all measurement lines alive for 30 minutes after loss of 48 V at an ambient temperature down to -15 C. The backup does not power valves or ignition.

The critical electronics load is 12.6 W. At 85% conversion efficiency the battery supplies about 14.8 W, or roughly 1.23 A at 12 V. The CYCLON battery is rated 2.5 Ah at its ten-hour rate. Applying a 50% combined allowance for discharge rate, cold capacity, aging, state of charge, cutoff, and tolerance leaves about 15 Wh at the battery and 12.75 Wh at the electronics. That gives about 60 minutes calculated. The requirement stays at 30 minutes.

EnerSys rates the CYCLON family down to -40 C. The ECU working minimum stays at -15 C until the rest of the power tree, sensors, connectors, and harness are checked at lower temperature. The 30-minute result must be checked with a fully instrumented -15 C cold-soak discharge test before qualification.

Backup input still needs a 3 A path, fuse at the battery, service disconnect, reverse-polarity protection, automatic switchover, battery current and voltage measurement, and low-voltage cutoff.

## Sources

- [Valcor V19800 sheet](https://www.valcor.com/valcor-technical-datasheets/valcor_aircraft_V19800.pdf)
- [TI AM2634 datasheet](https://www.ti.com/lit/ds/symlink/am2634-q1.pdf)
- [TI AM263x power estimator note](https://www.ti.com/lit/an/sprad54/sprad54.pdf)
- [TI AM26x hardware design guide](https://www.ti.com/lit/an/sprabjl/sprabjl.pdf)
- [TDK-Lambda i7A specification](https://product.tdk.com/en/system/files?file=dam/doc/product/power/switching-power/dc-dc-converter/specification/i7a_spec.pdf)
- [EnerSys CYCLON product page](https://www.enersys.com/en-gb/products/batteries/cyclon/cyclon/)
