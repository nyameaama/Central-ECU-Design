# Sensor Interfaces

## AST20PT layout

I am fitting eleven AST20PT sensors for the first thruster build.

| ID | Location | Placement |
| --- | --- | --- |
| FP-IN | Main fuel inlet | Before MFV |
| FP-MAN | Fuel manifold | After MFV |
| OP-IN | Main N2O inlet | Before MOV |
| OP-MAN | N2O manifold | After MOV |
| FP-IGN | Igniter fuel | After IFV and before the torch metering point |
| OP-IGN | Igniter oxidizer | After IOV and before the torch metering point |
| PP-CH | Chamber purge | After CPV |
| PP-IGN | Igniter purge | After IPV |
| PC | Main chamber | Remote pressure pickup with a thermal standoff |
| CJ-IN | Cooling jacket inlet | Jacket inlet port |
| CJ-OUT | Cooling jacket outlet | Jacket outlet port |

The igniter branch sensors are downstream of IFV and IOV. They measure the pressure actually delivered toward the torch instead of repeating the main inlet readings.

Each AST20PT has four electrical connections:

| Signal | ECU connection |
| --- | --- |
| Sensor power | `SENS_12V` |
| Sensor return | `SENS_RTN` |
| Pressure | `P_OUT`, 1 to 5 V |
| Temperature | `T_OUT`, 1 to 5 V |

I carry 10 mA per sensor, or 110 mA and 1.32 W for all eleven. Both outputs have input protection, an RC filter, and scaling into the 3.3 V ADC range. The 1 to 5 V live range leaves room for open- and short-circuit detection.

The sensor-side plug is Phoenix `1681127`. It accepts 18 to 24 AWG wire and 4 to 6 mm cable. My harness baseline is four-core shielded PUR cable with the shield bonded at the ECU end.

The chamber sensor uses the same electrical interface. Its remote tube and restrictor trade temperature protection against pressure-response delay, so the installed response becomes part of the chamber-pressure calibration.

## Thermocouples

The hot-structure probe is Omega `BLMI-XL-K-116U-6-CC`. The cable plug is `SMPW-K-M` and the PCB socket is `PCC-SMP-K-5`.

I am using five channels:

- four chamber-wall points
- one injector point

The PCB keeps eight MAX31856 interfaces, leaving three spare channels. The Type K metal runs straight to the Type K PCB sockets and stays out of the copper AMPSEAL contacts.

Each MAX31856 uses 3.3 V, SPI clock and data, one chip select, and the datasheet input filter. `DRDY` and `FAULT` return to the controller where pins are available.

## Main sensor connectors

Two 35-way AMPSEAL headers give 70 cavities.

| Use | Cavities |
| --- | ---: |
| AST20PT pressure outputs | 11 |
| AST20PT temperature outputs | 11 |
| Sensor power feeds | 2 |
| Sensor returns | 2 |
| Spare | 44 |
| Total | 70 |

The two harness branches split sensor power and return. The thermocouples use their own Type K connectors.

## Main-valve position feedback

The Buschjost `1E` option is a three-wire PNP normally open inductive sensor. It runs from 10 to 30 VDC, draws less than 10 mA, and can source 100 mA.

Both position switches run from `SENS_12V`, which keeps closed-valve indication alive on backup power. A protected divider and Schmitt input bring each signal into the AM2634. My selected valve configuration reports the closed position.

## PCB notes

- AST20PT supply range is 10 to 28 V
- AST20PT sample rate is 400 Hz maximum
- sensor ground joins valve return at the power entry point
- the analog filter corner follows the selected sample rate
- ECU board temperature and valve-driver temperature are local PCB measurements
