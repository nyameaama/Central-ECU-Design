# Power Budget

The vehicle feed is a 28 V nominal aircraft bus. The TDK-Lambda `I7A4W033A033V-003-R` makes the regulated 24 V valve rail. All eight valve outputs use one `TPS26633PWPR` high-side eFuse each.

The converter stays in the design because the Buschjost coils are rated at 24 V. Raw 28 V would be 16.7% above the nameplate value, and there is no published 28 V range for these exact coils.

## Valve loads

| Valve | Qty energized | Nominal current, each | Budget current, each | Budget power, each |
| --- | ---: | ---: | ---: | ---: |
| Buschjost fuel main | 1 | 1.83 A | 1.83 A | 44 W |
| Buschjost oxidizer main | 1 | 2.21 A | 2.21 A | 53 W |
| Parker G7 purge | up to 3 during purge | 0.42 A | 0.42 A | 10 W |
| Valcor V19800, 30 ohm coil | up to 2 during ignition | 1.04 A estimated | 1.30 A | 31.2 W |

MFV and MOV draw 4.04 A and 97 W. Ignition adds IFV and IOV, bringing the four-valve state to 6.64 A and 159.4 W. All three purge valves together draw 1.25 A and 30 W.

Valcor publishes 1.3 A at 30 V and 70 F for the V19800 30 ohm coil. The 24 V estimate is 1.04 A. Its eFuse channel is set for 1.59 A to cover cold resistance and supply tolerance.

## Valve channels

| Channel | `ILIM` resistor | Typical limit | Nominal coil current |
| --- | ---: | ---: | ---: |
| Fuel main | 7.15 kOhm | 2.52 A | 1.83 A |
| Oxidizer main | 6.04 kOhm | 2.98 A | 2.21 A |
| Parker G7 purge | 24.0 kOhm | 0.75 A | 0.42 A |
| V19800 | 11.3 kOhm | 1.59 A | 1.04 A estimated |

The `TPS26633PWPR` has a 31 mOhm typical internal FET. The two main-valve channels dissipate about 0.26 W including eFuse operating current. The four-channel ignition state is about 0.49 W using the V19800 budget current. The three purge channels are about 0.12 W.

`SHDN` is the valve command. Low removes coil power and the normally closed valve returns closed. A 47 kOhm pull-down gives the unpowered and reset state. `MODE` is open for latch-off after an overload. `IMON`, `FLT`, and `PGOOD` go back to the AM2634. Each output also has voltage sensing and its own inductive clamp.

The command validator allows four energized valves. Ignition uses MFV, MOV, IFV, and IOV. Normal main-engine operation uses MFV and MOV. FPV, OPV, and CPV are a separate purge state.

## Electronics loads

| Load | Power |
| --- | ---: |
| 20 AST20PT sensors | 2.40 W |
| AM2634 | 1.46 W |
| Analog supplies and rail monitoring | 5.00 W |
| CAN and service interface | 1.25 W |
| Eight MAX31856 parts | 0.053 W |
| Logic and small-load margin | 0.99 W |
| Two Buschjost position switches | 0.24 W max |
| Critical electronics total | 11.4 W |

TI's AM2634 traction-inverter estimate reaches 1.460 W at 150 C junction, so that is the value carried here. Rail limits remain 2.5 A for the 1.2 V core and RAM group, 200 mA for 3.3 V digital I/O, and 100 mA for 3.3 V analog.

The AM263x supply is `TPS6538600QDCARQ1` plus a `TPS62903-Q1` 3 A core buck.

## Main-bus cases

| State | Input power | Current at 28 V | Current at 24 V | 25% margin at 24 V |
| --- | ---: | ---: | ---: | ---: |
| Safe, valves off | 13.4 W | 0.48 A | 0.56 A | 0.70 A |
| Purge, three valves | 45.1 W | 1.61 A | 1.88 A | 2.35 A |
| Main engine, two valves | 115.9 W | 4.14 A | 4.83 A | 6.04 A |
| Ignition, four valves | 181.7 W | 6.49 A | 7.57 A | 9.47 A |

The protected vehicle input is 10 A before igniter power. At the 28 V nominal point that is 280 W available. The ignition state is the worst released continuous load at about 182 W before the 25% margin.

The valve calculation uses 95% conversion efficiency. TDK's published typical curve is about 97 to 98% around 28 V input and 24 V output in this load range. The electronics calculation uses 85% from the 12 V bus through the lower rails.

The i7A input range is 18 to 60 V, with input above output for regulation. Near a 24 V bus minimum the valve rail will leave regulation and track below 24 V. The final aircraft bus operating and transient limits set the ECU undervoltage threshold and front-end protection.

## Igniter

I could not find an off-the-shelf propane and nitrous-oxide rocket igniter with a released part number, pressure rating, interface, and input-current waveform for this engine. The igniter is custom.

Its bus voltage, peak current, pulse width, repetition rate, and maximum firing time are not released yet, so igniter power stays outside this total.

IFV, IOV, and IPV are still budgeted as V19800 solenoids. A three-phase replacement needs a new inverter and current profile before this table can include it.

## Backup

The controller, CAN, and measurement lines stay alive for 30 minutes after loss of the 28 V bus at an ambient temperature down to -15 C. Backup does not power valves or ignition.

At 11.4 W and 85% conversion efficiency, the battery supplies about 13.4 W or 1.12 A at 12 V. The CYCLON battery is rated 2.5 Ah at its ten-hour rate. A 50% combined allowance for discharge rate, cold capacity, aging, state of charge, cutoff, and tolerance leaves about 15 Wh at the battery and 12.75 Wh at the electronics. Calculated time is about 67 minutes. The released requirement stays at 30 minutes.

EnerSys rates the CYCLON family down to -40 C. ECU minimum temperature stays at -15 C until the full power tree and harness are qualified there.

The backup input is a 3 A path with a battery fuse, service disconnect, reverse-polarity protection, automatic switchover, battery current and voltage measurement, and low-voltage cutoff.

## Sources

- [Buschjost 2/918 R272](https://www.buschjostventile.de/en/products/2-918-08-R-272)
- [Buschjost inductive position sensor](https://www.buschjostventile.de/api/attachments/93eacf07-07ba-4c1d-8806-5cbe40241766.pdf)
- [Parker G7 series](https://www.parker.com/content/dam/Parker-com/Literature/Fluid-Control-Division/Catalogs/Parker_FCD_Catalog-G7.pdf)
- [Valcor V19800](https://www.valcor.com/valcor-technical-datasheets/valcor_aircraft_V19800.pdf)
- [TI TPS2663](https://www.ti.com/product/TPS2663)
- [TI AM2634 datasheet](https://www.ti.com/lit/ds/symlink/am2634-q1.pdf)
- [TI AM263x power estimator note](https://www.ti.com/lit/an/sprad54/sprad54.pdf)
- [TI AM26x hardware design guide](https://www.ti.com/lit/an/sprabjl/sprabjl.pdf)
- [TDK-Lambda i7A specification](https://product.tdk.com/en/system/files?file=dam/doc/product/power/switching-power/dc-dc-converter/specification/i7a_spec.pdf)
- [EnerSys CYCLON](https://www.enersys.com/en-gb/products/batteries/cyclon/cyclon/)
