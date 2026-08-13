# Parts List

Stock was checked on 2026-08-01. Counts will change, so check again before ordering.

## Sensors

| Qty | Part | Use | Status |
| ---: | --- | --- | --- |
| 32 max | TE `AST20PT4A01500P3Y1H000` | 0 to 1500 psig plus fluid temperature, dual 1 to 5 V output, M12, 1/4 inch MNPT | AST20PT is active. This setup needs a TE or LADD quote. |
| 32 max | Phoenix `1681127` | Field-wireable M12 plug for the AST20PT | Active. DigiKey had 2,945. |
| 5 | Omega `BLMI-XL-K-116U-6-CC` | Type K probe for four chamber wall points and one injector point | Orderable from Omega distributors. |
| 5 | Omega `SMPW-K-M` | Type K cable plug | Active. DigiKey had more than 3,000. |
| 1 pack | Omega `PCC-SMP-K-5` | Five Type K PCB sockets | Active. DigiKey had 178 packs. |
| 8 | Analog Devices `MAX31856MUD+T` | Thermocouple converter. Five used and three spare. | Production part. Mouser had more than 7,900. |

The 32 AST20PT count is the worst case. It allows two around every valve, one remote chamber pickup, two cooling jacket points, and one spare. Shared pipe nodes should bring the real count down.

## ECU connectors

| Qty | Part | Use |
| ---: | --- | --- |
| 1 | TE `776231-1` | Black 35-way AMPSEAL PCB header |
| 1 | TE `776231-2` | Natural 35-way AMPSEAL PCB header |
| 1 | TE `776164-1` | Black mating plug |
| 1 | TE `776164-2` | Natural mating plug |
| 70 plus spares | TE `770520-3` | Gold socket contacts, 16 to 20 AWG |
| as needed | TE `770678-1` | Plugs for unused cavities |

The two AMPSEAL headers carry the 64 AST20PT signals, two sensor power feeds, two returns, and two spare circuits. The thermocouples use their own Type K connectors and do not go through AMPSEAL.

## Valves

| Qty | Part | Use | Status |
| ---: | --- | --- | --- |
| 2 | Valcor `V80000`, NC, `-8` ports | Main fuel and fuel isolation. Cv 2.5. | Valcor quote needed. |
| 2 | Valcor `V80000`, NC, `-12` ports | Main oxidizer and oxidizer isolation. Cv 6.1. | Valcor quote and N2O approval needed. |
| 10 max | Valcor `V19800`, NC, 0.085 inch ESEO, 30 ohm continuous coil, top `MS3102A-10SL-4P` receptacle | Igniter, purge, bleed, chilldown, and dump | Valcor must assign the orderable dash number and approve each fluid. |
| 14 max | Amphenol `MS3106A10SL-4S` | Cable plug for the standard Valcor receptacle | DigiKey had 229. Confirm the receptacle with Valcor first. |

The engine model calls for 0.39879 kg/s of propane and 1.75468 kg/s of N2O. I used 0.50 kg/L propane and 0.75 kg/L N2O for the first valve check. That gives 12.6 gpm fuel and 37.1 gpm oxidizer.

Estimated valve drops:

- fuel V80000 `-8`: 12.8 psi
- oxidizer V80000 `-12`: 27.7 psi

The V80000 and V19800 coils use the 24 V valve rail. The V19800 electrical setup is fixed to the 30 ohm coil. Valcor publishes 1.3 A at 30 V and 70 F, which scales to about 1.04 A at 24 V. Keep 1.3 A of driver capacity per channel.

Before buying Valcor valves, get the actual dash numbers and confirm fluid compatibility, N2O cleaning, leakage, flow, opening time, mounting position, connector direction, shock, vibration, and coil current.

Valcor does not publish an order-code builder for the V19800. The description above is the configuration to put on the RFQ. The factory dash number is the final purchasing identifier.

## Power conversion

| Qty | Part | Use | Status |
| ---: | --- | --- | --- |
| 1 | TDK-Lambda `I7A4W033A033V-003-R` | 48 V to 24 V valve rail, 500 W module | In production. 97% typical at the published 48 V to 24 V full-load point. |
| 1 | TI `TPS6538600QDCARQ1` | AM2634 sequencing, watchdog, and supply supervision | TI-recommended AM263x PMIC. |
| 1 | TI `TPS62903-Q1` | 1.2 V AM2634 core supply, 3 A | TI-recommended companion buck. |

## Backup battery

| Qty | Part | Use | Status |
| ---: | --- | --- | --- |
| 1 | EnerSys CYCLON `0819-0020` | 12 V, 2.5 Ah pure-lead AGM backup battery | Active. OSI Batteries had 127 at $78.92. |
| 1 | Power-Sonic `PSC-12500ACX` | 12 V AGM service charger | Use only while the engine is safe. Confirm its charge profile on the first battery. |

The battery only runs the controller, CAN, and sensor measurement electronics. It does not run valves or ignition.

Battery size is 113.8 x 89.4 x 70.4 mm and weight is 1.04 kg. It uses 0.187 inch Faston terminals. Put a fuse next to the positive terminal and cover both terminals. The ECU also needs reverse polarity protection, current measurement, automatic power switchover, and a low voltage cutoff.

AGM float voltage at 25 C is 13.50 to 13.80 V. Do not connect it straight to an unregulated 12 V rail and call that a charger.

## Before ordering AST20PT

Send TE or LADD the full code `AST20PT4A01500P3Y1H000` and confirm:

- the code is valid and currently buildable
- propane and liquid N2O compatibility
- oxidizer cleaning option
- M12 pinout
- shock and vibration ratings
- lead time and minimum order

Do not buy the stocked 150 or 200 psi versions. They are too low for this engine.

## Links

- [AST20PT](https://www.te.com/en/product-CAT-PTT0038.html)
- [Phoenix 1681127](https://www.digikey.com/en/products/detail/phoenix-contact/1681127/2510450)
- [Omega BLMI probe](https://blackhawksupply.com/products/omega-blmi-xl-k-116u-6-cc)
- [Omega SMPW-K-M](https://www.digikey.com/en/products/detail/omega/SMPW-K-M/25638955)
- [Omega PCC-SMP-K-5](https://www.digikey.com/en/products/detail/omega/PCC-SMP-K-5/25639954)
- [MAX31856](https://www.analog.com/en/products/max31856.html)
- [TE 776231-1](https://www.te.com/en/product-776231-1.html)
- [TE 776164-1](https://www.te.com/en/product-776164-1.html)
- [Valcor V80000](https://www.valcor.com/valcors-high-flow-high-pressure-v80000-solenoid-valve/)
- [Valcor V19800](https://www.valcor.com/valcor-technical-datasheets/2-way-direct-acting-shut-off-solenoid-valve-V19800.pdf)
- [Amphenol valve plug](https://www.digikey.com/en/products/detail/amphenol-industrial-operations/MS3106A10SL-4S/378636)
- [EnerSys CYCLON](https://www.enersys.com/en-gb/products/batteries/cyclon/cyclon/)
- [EnerSys 0819-0020 stock](https://www.osibatteries.com/enersys-cyclon-0819-0020-assembly-battery-2x3-monobloc)
- [TDK-Lambda I7A4W033A033V-003-R](https://product.tdk.com/en/search/power/switching-power/dc-dc-converter/info?part_no=I7A4W033A033V-003-R)
- [TI TPS653860-Q1](https://www.ti.com/product/TPS653860-Q1)
- [TI TPS62903-Q1](https://www.ti.com/product/TPS62903-Q1)
