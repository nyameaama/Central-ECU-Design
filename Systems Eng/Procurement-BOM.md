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
| 2 | Triton `TS-SP08B2000-0210` | MFV and fuel inlet isolation. NC, AS5202-08 ports, Cv 2.5, 100 to 2,000 psia. | Baseline only. Triton lists RP-1 and N2O, not propane. Propane service and the published `<500 seconds` closing-time entry are open. |
| 2 | Triton `TS-SP16A2000-0200` | MOV and oxidizer inlet isolation. NC, AN-16 ports, Cv 21, 50 to 2,000 psia. | Current oxidizer selection. Triton lists N2O service. Cleaning certification and the 2 second maximum closing time still need to land in the engine requirements. |
| 2 backup | Buschjost `2/918-69/0824/.272-GO-1E`, 24 VDC | Fuel main and inlet isolation backup. NC direct coaxial valve, G1/2, DN10, Kv 2.5. | Active family. Public STEP is in Mechanical Eng. Made to order. |
| 2 backup | Buschjost `2/918-24/0824/.272-GO-1E`, 24 VDC | Oxidizer main and inlet isolation backup. NC direct coaxial valve, G3/4, DN20, Kv 6.8. | Active family. Public STEP is in Mechanical Eng. Made to order. |
| 6 | Amphenol `D38999/26WA98SN` | Harness mate for the Triton valve receptacle. Four used and two spare. | Active part. The Triton pin assignment is not public. |
| 10 max | Valcor `V19800`, NC, 0.085 inch ESEO, 30 ohm continuous coil, top `MS3102A-10SL-4P` receptacle | Igniter, purge, bleed, chilldown, and dump | Valcor must assign the orderable dash number and approve each fluid. |
| 14 max | Amphenol `MS3106A10SL-4S` | Cable plug for the standard Valcor receptacle | DigiKey had 229. Confirm the receptacle with Valcor first. |

The engine model calls for 0.39879 kg/s of propane and 1.75468 kg/s of N2O. I used 0.50 kg/L propane and 0.75 kg/L N2O for the first valve check. That gives 12.6 gpm fuel and 37.1 gpm oxidizer.

Estimated drop through one valve:

- `TS-SP08`: 12.7 psi at the fuel design flow
- `TS-SP16`: 2.3 psi at the oxidizer design flow

There are two valves in series on each propellant line, so the first-pass totals are 25.4 psi fuel and 4.7 psi oxidizer. Both Triton valves use upstream propellant pressure to operate the internal pilot. There is no separate pneumatic supply.

Both valves accept 18 to 36 VDC and draw 0.28 A at 28 VDC. The 24 V driver allocation is 0.50 A per channel. Loss of coil power returns the valve closed. The electrical receptacle is `D38999/25YA98PN`; `D38999/26WA98SN` is the three-socket cable mate.

The oxidizer line moves to AN-16 at the valve. The Converse piping sheet still lists line sizing as unfinished, so this does not conflict with a released pipe size. The fuel valve keeps the earlier -8 interface.

Triton supplies the complete valve envelope CAD through the download form on each product page. The form requires a name, email address, and CAPTCHA, then sends the file by email. No public STEP URL is exposed on either page.

### Buschjost backup

These are complete electric valves. They do not need an actuation-air circuit.

| | Fuel | Oxidizer |
| --- | ---: | ---: |
| Port and bore | G1/2, DN10 | G3/4, DN20 |
| Kv | 2.5 m3/h | 6.8 m3/h |
| Coil | 44 W | 53 W |
| Current at 24 V | 1.83 A | 2.21 A |
| Switching time at 6 bar gas | 50 ms on, 80 ms off | 110 ms on, 100 ms off |
| Estimated drop, one valve | 9.5 psi | 16.7 psi |
| Estimated drop, two valves | 19.1 psi | 33.4 psi |

Both configurations are 0 to 100 bar, 316 Ti stainless, PTFE/FKM, normally closed, IP65 with the Form A plug installed, and rated for continuous coil duty. `GO` is the cleaned version. Buschjost says it may contain oxygen-approved lubricant and does not claim BAM approval. `1E` adds one inductive position sensor and 25 mm to the valve length. The switching times are catalog tests with 6 bar gas, not measurements with this engine's liquids and pressures.

Buschjost's resistance table marks PTFE/FKM and 316 Ti compatible with LPG and N2O. That clears the material screen. It is not a propulsion qualification or an oxidizer-cleaning certificate.

The valve connector is EN 175301-803 Form A and comes with the valve. Specify the 24 VDC coil on the order because voltage is not encoded in the part string. The position sensor has a separate three-wire, 2 m lead. Set it to report closed when ordering.

The fuel model uses a G1/2 to -8 adapter. The oxidizer model uses a G3/4 to -12 adapter.

`R272` stays backup, not a drop-in replacement for Triton. Its 100 bar rating is 1,450 psi and the published temperature floor is -10 C. The engine chamber is 700 psi, but the Converse feed MAWP is not frozen. The ECU working minimum is -15 C. The option page calls it bidirectional, while the family sheet only gives 16 bar reverse pressure. Feed MAWP and the shutdown transient still have to clear those limits. The coil drivers also change from 0.5 A to 2.5 A fuel and 3 A oxidizer.

The STEP files download directly from the exact product pages without an account. They are complete electric-valve envelopes with the solenoid and `1E` switch, not manual-valve models. The source filenames say R270/R271/R272 because Buschjost shares the envelope across those pressure versions. The valves are current but made to order. Buschjost and its US reseller do not show stock for these configurations.

The V19800 electrical setup is fixed to the 30 ohm coil. Valcor publishes 1.3 A at 30 V and 70 F, which scales to about 1.04 A at 24 V. Small-valve driver capacity is 1.3 A per channel.

Valcor does not publish an order-code builder for the V19800. The description above is the RFQ configuration; the factory dash number becomes the purchasing identifier.

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
| 1 | Power-Sonic `PSC-12500ACX` | 12 V AGM service charger | Ground-service charging only. Charge profile gets checked on the first battery. |

The battery only runs the controller, CAN, and sensor measurement electronics. It does not run valves or ignition.

Battery size is 113.8 x 89.4 x 70.4 mm and weight is 1.04 kg. It uses 0.187 inch Faston terminals. The battery lead has a fuse at the positive terminal and covers over both terminals. The ECU includes reverse polarity protection, current measurement, automatic power switchover, and a low voltage cutoff.

AGM float voltage at 25 C is 13.50 to 13.80 V. Charging comes from a regulated AGM profile, not the unregulated 12 V rail.

## AST20PT configuration

Selected code: `AST20PT4A01500P3Y1H000`

The quote still needs these entries:

- the code is valid and currently buildable
- propane and liquid N2O compatibility
- oxidizer cleaning option
- M12 pinout
- shock and vibration ratings
- lead time and minimum order

The required pressure range is 0 to 1,500 psig. The stocked 150 and 200 psig variants do not cover the feed system.

## Links

- [AST20PT](https://www.te.com/en/product-CAT-PTT0038.html)
- [Phoenix 1681127](https://www.digikey.com/en/products/detail/phoenix-contact/1681127/2510450)
- [Omega BLMI probe](https://blackhawksupply.com/products/omega-blmi-xl-k-116u-6-cc)
- [Omega SMPW-K-M](https://www.digikey.com/en/products/detail/omega/SMPW-K-M/25638955)
- [Omega PCC-SMP-K-5](https://www.digikey.com/en/products/detail/omega/PCC-SMP-K-5/25639954)
- [MAX31856](https://www.analog.com/en/products/max31856.html)
- [TE 776231-1](https://www.te.com/en/product-776231-1.html)
- [TE 776164-1](https://www.te.com/en/product-776164-1.html)
- [Triton TS-SP08B2000-0210](https://www.triton-space.com/products/valves/solenoid/ts-sp08b2000-0210)
- [Triton TS-SP16A2000-0200](https://www.triton-space.com/products/valves/solenoid/ts-sp16a2000-0200)
- [Buschjost fuel backup](https://www.buschjostventile.de/en/products/2-918-08-R-272/details/2-918-69-0824-R272-GO-1E)
- [Buschjost oxidizer backup](https://www.buschjostventile.de/en/products/2-918-08-R-272/details/2-918-24-0824-R272-GO-1E)
- [Buschjost material resistance table](https://www.buschjostventile.de/api/pdf/resistance-table?locale=en)
- [Buschjost inductive position sensor](https://www.buschjostventile.de/api/attachments/93eacf07-07ba-4c1d-8806-5cbe40241766.pdf)
- [Buschjost US reseller](https://adamsllc.net/brands/buschjost/)
- [Amphenol D38999/26WA98SN](https://www.mouser.com/ProductDetail/Amphenol-Aerospace/D38999-26WA98SN)
- [Valcor V19800](https://www.valcor.com/valcor-technical-datasheets/2-way-direct-acting-shut-off-solenoid-valve-V19800.pdf)
- [Amphenol valve plug](https://www.digikey.com/en/products/detail/amphenol-industrial-operations/MS3106A10SL-4S/378636)
- [EnerSys CYCLON](https://www.enersys.com/en-gb/products/batteries/cyclon/cyclon/)
- [EnerSys 0819-0020 stock](https://www.osibatteries.com/enersys-cyclon-0819-0020-assembly-battery-2x3-monobloc)
- [TDK-Lambda I7A4W033A033V-003-R](https://product.tdk.com/en/search/power/switching-power/dc-dc-converter/info?part_no=I7A4W033A033V-003-R)
- [TI TPS653860-Q1](https://www.ti.com/product/TPS653860-Q1)
- [TI TPS62903-Q1](https://www.ti.com/product/TPS62903-Q1)
