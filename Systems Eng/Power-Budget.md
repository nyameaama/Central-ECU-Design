# Power Budget

The vehicle feed is a 28 V nominal aircraft bus. The TDK-Lambda `I7A4W033A033V-003-R` makes the regulated 24 V valve rail. I have seven `TPS26633PWPR` valve channels on the PCB, with six assigned and one disabled spare.

The 24 V converter stays because the selected valve coils are 24 V parts. Raw 28 V is 16.7% above the nameplate voltage of the Buschjost coils.

## Valve loads

| Valve | Assignment | Nominal current | Channel budget current | Coil power |
| --- | --- | ---: | ---: | ---: |
| Buschjost fuel main | MFV | 1.83 A | 1.83 A | 44 W |
| Buschjost oxidizer main | MOV | 2.21 A | 2.21 A | 53 W |
| Valcor V19800, 30 ohm coil | IFV and IOV | not released at 24 V | 1.30 A | 31.2 W each at the budget current |
| Parker G7 | CPV and IPV | 0.42 A | 0.42 A | 10 W each |

MFV and MOV together draw 4.04 A and 97 W. The four-valve ignition overlap adds IFV and IOV and brings the valve rail to 6.64 A and 159.4 W. Normal main operation uses MFV, MOV, and IPV for 4.46 A and 107 W. CPV and IPV together use 0.83 A and 20 W during the full post-fire purge.

Valcor's sheet says 1.3 A at 30 V and 70 F for the 30 ohm coil. The stated current and resistance do not agree, so I carry the published 1.3 A as the worst case on the 24 V channel. The exact hot and cold current comes from the purchased dash number and valve test.

## Valve channels

| Channel | Assignment | `ILIM` resistor | Typical limit |
| ---: | --- | ---: | ---: |
| 1 | MFV | 7.15 kOhm | 2.52 A |
| 2 | MOV | 6.04 kOhm | 2.98 A |
| 3 | IFV | 11.3 kOhm | 1.59 A |
| 4 | IOV | 11.3 kOhm | 1.59 A |
| 5 | CPV | 24.0 kOhm | 0.75 A |
| 6 | IPV | 24.0 kOhm | 0.75 A |
| 7 | Spare | not loaded | disabled |

`SHDN` is the valve command. A 47 kOhm pull-down leaves every channel off while the controller is unpowered or resetting. `MODE` is open for latch-off after an overload. `IMON`, `FLT`, `PGOOD`, and output-voltage sensing return to the AM2634.

The `TPS26633PWPR` has a 31 mOhm typical internal FET. The two main channels dissipate about 0.26 W together including the eFuse operating current. The four-channel ignition state is about 0.49 W using the V19800 channel current. Each output has its own inductive clamp.

## Allowed valve states

| State | Energized valves |
| --- | --- |
| Safe | none |
| Torch light | IFV and IOV |
| Main ignition overlap | MFV, MOV, IFV, and IOV |
| Main run | MFV, MOV, and IPV |
| Post-fire purge | CPV and IPV |

CPV stays out of the ignition and main-run states. IPV stays out of the torch-light state and comes on after IFV and IOV close.

## Electronics loads

| Load | Power |
| --- | ---: |
| 11 AST20PT sensors | 1.32 W |
| AM2634 | 1.46 W |
| Analog supplies and rail monitoring | 5.00 W |
| CAN and service interface | 1.25 W |
| Eight MAX31856 parts | 0.053 W |
| Logic and small-load margin | 0.99 W |
| Two Buschjost position switches | 0.24 W max |
| Critical electronics total | 10.3 W |

The AM2634 number is the 1.460 W result from TI's traction-inverter estimate at 150 C junction. The AM263x supply is `TPS6538600QDCARQ1` plus a `TPS62903-Q1` 3 A core buck.

## Main-bus cases

| State | Input power | Current at 28 V | Equivalent current at 24 V | 25% margin at 24 V |
| --- | ---: | ---: | ---: | ---: |
| Safe, valves off | 12.1 W | 0.43 A | 0.51 A | 0.63 A |
| Post-fire purge, CPV and IPV | 33.2 W | 1.19 A | 1.38 A | 1.73 A |
| Main run, MFV, MOV, and IPV | 124.8 W | 4.46 A | 5.20 A | 6.50 A |
| Ignition overlap, four valves | 179.9 W | 6.43 A | 7.50 A | 9.37 A |

I use 95% efficiency for the 24 V valve conversion and 85% from the 12 V rail through the lower electronics rails. TDK's typical curve is around 97 to 98% near this operating point, so 95% is the number I keep in the budget.

The protected vehicle input is 10 A at 28 V, or 280 W before the igniter load. The released valve and electronics peak is about 180 W during ignition overlap. That leaves about 100 W before margin and about 55 W after applying the 25% system margin.

The i7A input range is 18 to 60 V and it requires the input to stay above the commanded output for regulation. Near a 24 V vehicle-bus minimum, the valve rail tracks below 24 V.

## Igniter

The spark igniter is custom. I have not found an off-the-shelf propane and N2O rocket igniter with a released part number, pressure interface, and input-current waveform for this engine.

Its peak current, pulse width, repetition rate, and maximum firing time are not in the total above yet. The first-design valve side is fixed as separate IFV and IOV solenoids.

## Backup

The controller, CAN, and measurement lines stay alive for 30 minutes after loss of the 28 V bus at ambient temperature down to -15 C. Backup does not power valves or ignition.

The critical electronics use 10.3 W. At 85% conversion efficiency, the battery supplies about 12.1 W or 1.01 A at 12 V. A 50% allowance for discharge rate, cold capacity, aging, state of charge, cutoff, and tolerance leaves 12.75 Wh at the electronics. That gives about 74 minutes by calculation, while my released number stays at 30 minutes.

The backup input is a 3 A path with a battery fuse, service disconnect, reverse-polarity protection, automatic switchover, battery current and voltage measurement, and low-voltage cutoff.

## Sources

- [Buschjost 2/918 R272](https://www.buschjostventile.de/en/products/2-918-08-R-272)
- [Buschjost position sensor](https://www.buschjostventile.de/api/attachments/93eacf07-07ba-4c1d-8806-5cbe40241766.pdf)
- [Parker G7](https://www.parker.com/content/dam/Parker-com/Literature/Fluid-Control-Division/Catalogs/Parker_FCD_Catalog-G7.pdf)
- [Valcor V19800](https://www.valcor.com/valcor-technical-datasheets/valcor_aircraft_V19800.pdf)
- [TI TPS2663](https://www.ti.com/product/TPS2663)
- [TI AM2634](https://www.ti.com/lit/ds/symlink/am2634-q1.pdf)
- [TI AM263x power estimator](https://www.ti.com/lit/an/sprad54/sprad54.pdf)
- [TI AM26x hardware guide](https://www.ti.com/lit/an/sprabjl/sprabjl.pdf)
- [TDK-Lambda i7A](https://product.tdk.com/en/system/files?file=dam/doc/product/power/switching-power/dc-dc-converter/specification/i7a_spec.pdf)
- [EnerSys CYCLON](https://www.enersys.com/en-gb/products/batteries/cyclon/cyclon/)
