# Parts List

General stock was checked on 2026-08-01. TPS26633, i7A, and Parker G7 status were checked on 2026-08-28. Counts will change before the first build.

## Sensors

| Qty | Part | Use | Status |
| ---: | --- | --- | --- |
| 20 max | TE `AST20PT4A01500P3Y1H000` | 0 to 1500 psig plus fluid temperature, dual 1 to 5 V output, M12, 1/4 inch MNPT | AST20PT is active. This setup needs a TE or LADD quote. |
| 20 max | Phoenix `1681127` | Field-wireable M12 plug for the AST20PT | Active. DigiKey had 2,945. |
| 5 | Omega `BLMI-XL-K-116U-6-CC` | Type K probe for four chamber wall points and one injector point | Orderable from Omega distributors. |
| 5 | Omega `SMPW-K-M` | Type K cable plug | Active. DigiKey had more than 3,000. |
| 1 pack | Omega `PCC-SMP-K-5` | Five Type K PCB sockets | Active. DigiKey had 178 packs. |
| 8 | Analog Devices `MAX31856MUD+T` | Thermocouple converter. Five used and three spare. | Production part. Mouser had more than 7,900. |

The 20 AST20PT count allows two around each retained valve, one remote chamber pickup, two cooling jacket points, and one spare. Shared pipe nodes should bring the fitted count down.

## ECU connectors

| Qty | Part | Use |
| ---: | --- | --- |
| 1 | TE `776231-1` | Black 35-way AMPSEAL PCB header |
| 1 | TE `776231-2` | Natural 35-way AMPSEAL PCB header |
| 1 | TE `776164-1` | Black mating plug |
| 1 | TE `776164-2` | Natural mating plug |
| 70 plus spares | TE `770520-3` | Gold socket contacts, 16 to 20 AWG |
| as needed | TE `770678-1` | Plugs for unused cavities |

The two AMPSEAL headers carry 40 AST20PT signals, two sensor power feeds, two returns, and 26 spare circuits. The thermocouples use their own Type K connectors and do not go through AMPSEAL.

## Valves

| Qty | Part | Use | Status |
| ---: | --- | --- | --- |
| 1 | Buschjost `2/918-69/0824/.272-GO-1E`, 24 VDC | MFV. NC direct coaxial valve, G1/2, DN10, Kv 2.5. | Main fuel selection. Public STEP is in Mechanical Eng. Made to order. |
| 1 | Buschjost `2/918-24/0824/.272-GO-1E`, 24 VDC | MOV. NC direct coaxial valve, G3/4, DN20, Kv 6.8. | Main oxidizer selection. Public STEP is in Mechanical Eng. Made to order. |
| 3 | Parker `20CC04EP7D7B`, 24 VDC | FPV, OPV, and CPV. NC direct-acting G7 valve, 1/4 NPT, Cv 0.05. | Purge baseline. Wilson listed 13 at $273.87 each. Final purge flow is still open. |
| 3 | Parker `ELECE5` | DIN cord set for the G7 valve | Six-foot lead set. |
| 3 | Valcor `V19800`, NC, 0.085 inch ESEO, 30 ohm continuous coil, top `MS3102A-10SL-4P` receptacle | IFV, IOV, and IPV | Igniter-valve baseline. Valcor must assign the orderable dash number and approve each fluid. |
| 3 | Amphenol `MS3106A10SL-4S` | Cable plug for the standard Valcor receptacle | Receptacle stays tied to the final Valcor dash number. |

The engine model calls for 0.39879 kg/s of propane and 1.75468 kg/s of N2O. I used 0.50 kg/L propane and 0.75 kg/L N2O for the first valve check. That gives 12.6 gpm fuel and 37.1 gpm oxidizer.

Estimated drop through one valve:

- fuel Buschjost: 9.5 psi
- oxidizer Buschjost: 16.7 psi

These are complete direct-electric valves. They use no pilot-air circuit. Loss of coil power returns them closed.

| | Fuel | Oxidizer |
| --- | ---: | ---: |
| Port and bore | G1/2, DN10 | G3/4, DN20 |
| Kv | 2.5 m3/h | 6.8 m3/h |
| Coil | 44 W | 53 W |
| Current at 24 V | 1.83 A | 2.21 A |
| Switching time at 6 bar gas | 50 ms on, 80 ms off | 110 ms on, 100 ms off |
| Estimated drop, one valve | 9.5 psi | 16.7 psi |

Both configurations are 0 to 100 bar, 316 Ti stainless, PTFE/FKM, normally closed, IP65 with the Form A plug installed, and rated for continuous coil duty. `GO` is the cleaned version. Buschjost says it may contain oxygen-approved lubricant and does not claim BAM approval. `1E` adds one inductive position sensor and 25 mm to the valve length. The switching times are catalog tests with 6 bar gas, not measurements with this engine's liquids and pressures.

Buschjost's resistance table marks PTFE/FKM and 316 Ti compatible with LPG and N2O. That clears the material screen. It is not a propulsion qualification or an oxidizer-cleaning certificate.

The valve connector is EN 175301-803 Form A and comes with the valve. The build specification calls out the 24 VDC coil because voltage is not encoded in the part string. The position sensor has a separate three-wire, 2 m lead and is ordered for closed-position indication.

The fuel model uses a G1/2 to -8 adapter. The oxidizer model uses a G3/4 to -12 adapter.

The open release items are the 100 bar working-pressure limit, the -10 C fluid and ambient floor, the 16 bar reverse-pressure entry in the family sheet, and the `GO` cleaning certificate for N2O service. The engine chamber is 700 psi, but feed MAWP and the shutdown transient are not frozen yet. The ECU working minimum is -15 C, so the valve installation needs a separate thermal limit or a warmer qualified build.

The STEP files download directly from the exact product pages without an account. They are the complete valve, solenoid, connector, and `1E` switch envelopes. The source filenames cover R270, R271, and R272 because the pressure versions share the same envelope.

The Parker purge valve is rated for zero to 2200 psi differential with air or inert gas. It uses a 3/64 inch orifice and a 10 W coil. The exact assembly weighs 1.25 lb. The `Cv 0.05` flow check stays open until purge supply pressure and required mass flow are released.

The V19800 electrical setup is fixed to the 30 ohm coil. Valcor publishes 1.3 A at 30 V and 70 F, which scales to about 1.04 A at 24 V. Small-valve driver capacity is 1.3 A per channel.

Valcor does not publish an order-code builder for the V19800. The description above is the RFQ configuration. The factory dash number becomes the purchasing identifier.

IFV, IOV, and IPV remain provisional. A custom three-phase igniter actuator would replace these three valve channels with an inverter and new current sensing.

## Power conversion

| Qty | Part | Use | Status |
| ---: | --- | --- | --- |
| 1 | TDK-Lambda `I7A4W033A033V-003-R` | 28 V nominal bus to regulated 24 V valve rail, 500 W module | In production. Input is 18 to 60 V with input above output. |
| 10 buy, 8 fitted | TI `TPS26633PWPR` | One high-side eFuse per valve output, plus two build spares | Active production. 4.5 to 60 V, 0.6 to 6 A adjustable limit, 31 mOhm typical FET. |
| 1 | TI `TPS6538600QDCARQ1` | AM2634 sequencing, watchdog, and supply supervision | TI-recommended AM263x PMIC. |
| 1 | TI `TPS62903-Q1` | 1.2 V AM2634 core supply, 3 A | TI-recommended companion buck. |

The Buschjost coils stay on the regulated 24 V rail. A 28 V direct connection is 16.7% above the nameplate value, and Buschjost does not publish a 28 V operating range for these exact coils.

Each `TPS26633PWPR` uses `SHDN` as its valve command. A 47 kOhm pull-down leaves the channel off while the controller is unpowered or its pin is high impedance. `MODE` stays open for latch-off after an overload. A new command cycle clears the latch. `IMON`, `FLT`, and `PGOOD` return channel current and fault state to the controller.

First-pass current limits are 2.52 A for the fuel main valve with 7.15 kOhm at `ILIM`, 2.98 A for the oxidizer main valve with 6.04 kOhm, 0.75 A for each Parker purge valve with 24.0 kOhm, and 1.59 A for each V19800 channel with 11.3 kOhm. The values include cold-coil margin and remain below the 6 A device limit.

Each valve output also gets its own inductive clamp near the harness connector. Buschjost does not publish coil inductance, so clamp voltage and release time get closed on the valve bench test.

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

The sensor range is 0 to 1,500 psig.

## Links

- [AST20PT](https://www.te.com/en/product-CAT-PTT0038.html)
- [Phoenix 1681127](https://www.digikey.com/en/products/detail/phoenix-contact/1681127/2510450)
- [Omega BLMI probe](https://blackhawksupply.com/products/omega-blmi-xl-k-116u-6-cc)
- [Omega SMPW-K-M](https://www.digikey.com/en/products/detail/omega/SMPW-K-M/25638955)
- [Omega PCC-SMP-K-5](https://www.digikey.com/en/products/detail/omega/PCC-SMP-K-5/25639954)
- [MAX31856](https://www.analog.com/en/products/max31856.html)
- [TE 776231-1](https://www.te.com/en/product-776231-1.html)
- [TE 776164-1](https://www.te.com/en/product-776164-1.html)
- [Buschjost fuel valve](https://www.buschjostventile.de/en/products/2-918-08-R-272/details/2-918-69-0824-R272-GO-1E)
- [Buschjost oxidizer valve](https://www.buschjostventile.de/en/products/2-918-08-R-272/details/2-918-24-0824-R272-GO-1E)
- [Buschjost material resistance table](https://www.buschjostventile.de/api/pdf/resistance-table?locale=en)
- [Buschjost inductive position sensor](https://www.buschjostventile.de/api/attachments/93eacf07-07ba-4c1d-8806-5cbe40241766.pdf)
- [Buschjost US reseller](https://adamsllc.net/brands/buschjost/)
- [Parker 20CC04EP7D7B](https://www.wilson-company.com/product/20cc04ep7d7b/parker-g7-series-solenoid-valves)
- [Parker G7 catalog](https://www.parker.com/content/dam/Parker-com/Literature/Fluid-Control-Division/Catalogs/Parker_FCD_Catalog-G7.pdf)
- [Parker G7 CAD configurator](https://parker-embedded.partcommunity.com/3d-cad-models/sso/?info=parkerfcd%2Fg720series20solenoid20valves%2Fg720series20solenoid20valves.prj)
- [Valcor V19800](https://www.valcor.com/valcor-technical-datasheets/2-way-direct-acting-shut-off-solenoid-valve-V19800.pdf)
- [Amphenol valve plug](https://www.digikey.com/en/products/detail/amphenol-industrial-operations/MS3106A10SL-4S/378636)
- [EnerSys CYCLON](https://www.enersys.com/en-gb/products/batteries/cyclon/cyclon/)
- [EnerSys 0819-0020 stock](https://www.osibatteries.com/enersys-cyclon-0819-0020-assembly-battery-2x3-monobloc)
- [TDK-Lambda I7A4W033A033V-003-R](https://product.tdk.com/en/search/power/switching-power/dc-dc-converter/info?part_no=I7A4W033A033V-003-R)
- [TI TPS2663](https://www.ti.com/product/TPS2663)
- [TI TPS653860-Q1](https://www.ti.com/product/TPS653860-Q1)
- [TI TPS62903-Q1](https://www.ti.com/product/TPS62903-Q1)
