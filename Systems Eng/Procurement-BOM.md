# Parts List

This is the first-build list for the six-valve thruster. My last general stock check was 2026-08-01. I checked the TPS26633, i7A, and Parker G7 again on 2026-08-28.

## Sensors

| Buy | Fitted | Part | Where I am using it |
| ---: | ---: | --- | --- |
| 12 | 11 | TE `AST20PT4A01500P3Y1H000` | Main lines, igniter branches, both purge branches, remote chamber pickup, and cooling jacket |
| 12 | 11 | Phoenix `1681127` | Field-wireable M12 plug for each AST20PT |
| 5 | 5 | Omega `BLMI-XL-K-116U-6-CC` | Four chamber-wall points and one injector point |
| 5 | 5 | Omega `SMPW-K-M` | Type K cable plug |
| 1 pack | 5 | Omega `PCC-SMP-K-5` | Type K PCB sockets |
| 8 | 8 | Analog Devices `MAX31856MUD+T` | Five thermocouple channels and three spares |

The eleven AST20PT points are:

1. Fuel before MFV
2. Fuel after MFV
3. N2O before MOV
4. N2O after MOV
5. Igniter fuel after IFV
6. Igniter oxidizer after IOV
7. Chamber purge after CPV
8. Igniter purge after IPV
9. Remote chamber pressure
10. Cooling jacket inlet
11. Cooling jacket outlet

The selected AST20PT is 0 to 1500 psig with separate 1 to 5 V pressure and temperature outputs, an M12 connector, and a 1/4 inch MNPT process port. I am carrying one spare sensor and one spare M12 plug.

## ECU connectors

| Qty | Part | Use |
| ---: | --- | --- |
| 1 | TE `776231-1` | Black 35-way AMPSEAL PCB header |
| 1 | TE `776231-2` | Natural 35-way AMPSEAL PCB header |
| 1 | TE `776164-1` | Black mating plug |
| 1 | TE `776164-2` | Natural mating plug |
| 70 plus spares | TE `770520-3` | Gold socket contacts, 16 to 20 AWG |
| as needed | TE `770678-1` | Cavity plugs |

The two headers carry 22 AST20PT signal wires, two sensor-power feeds, two returns, and 44 spare cavities. The Type K connections stay separate from the AMPSEAL connectors.

## Valves

| Qty | Part | Assignment | What I am carrying |
| ---: | --- | --- | --- |
| 1 | Buschjost `2/918-69/0824/.272-GO-1E`, 24 VDC | MFV | NC direct coaxial valve, G1/2, DN10, Kv 2.5, 44 W |
| 1 | Buschjost `2/918-24/0824/.272-GO-1E`, 24 VDC | MOV | NC direct coaxial valve, G3/4, DN20, Kv 6.8, 53 W |
| 2 | Parker `20CC04EP7D7B`, 24 VDC | CPV and IPV | NC direct-acting G7 valve, 1/4 NPT, Cv 0.05, 10 W |
| 2 | Parker `ELECE5` | CPV and IPV | Six-foot DIN cord set |
| 2 | Valcor `V19800`, NC, 0.085 inch ESEO, 30 ohm continuous coil, top `MS3102A-10SL-4P` receptacle | IFV and IOV | First-design torch valves |
| 2 | Amphenol `MS3106A10SL-4S` | IFV and IOV | Cable plug for the Valcor receptacle |

The engine-side valve count stops at these six.

The main engine model calls for 0.39879 kg/s propane and 1.75468 kg/s N2O. My first density pass uses 0.50 kg/L propane and 0.75 kg/L N2O. That works out to 12.6 gpm fuel and 37.1 gpm oxidizer.

| | Fuel | Oxidizer |
| --- | ---: | ---: |
| Port and bore | G1/2, DN10 | G3/4, DN20 |
| Kv | 2.5 m3/h | 6.8 m3/h |
| Coil | 44 W | 53 W |
| Current at 24 V | 1.83 A | 2.21 A |
| Catalog switching time at 6 bar gas | 50 ms on, 80 ms off | 110 ms on, 100 ms off |
| First valve-drop estimate | 9.5 psi | 16.7 psi |

Both Buschjost configurations are normally closed, direct-electric 316 Ti stainless valves with PTFE/FKM seals. They are listed for 0 to 100 bar and continuous coil duty. `GO` is the cleaned version and `1E` adds one closed-position inductive sensor. The fuel side uses G1/2 to -8 and the oxidizer side uses G3/4 to -12.

The Buschjost resistance table lists the wetted materials as compatible with LPG and N2O. I still have the 100 bar feed limit, -10 C temperature floor, reverse-pressure limit, and actual `GO` cleaning certificate tied to the purchased configuration. The published switching times are gas tests, so my liquid timing comes from the assembled valve test.

The Parker G7 is only on regulated GN2. It is listed for zero to 2200 psi differential with air or inert gas, has a 3/64 inch orifice, and uses a 10 W coil. CPV feeds the main chamber purge and IPV feeds the torch chamber purge.

The V19800 setup is the 30 ohm continuous coil. Valcor's sheet also states 1.3 A at 30 V and 70 F, which does not agree with the listed 30 ohm resistance. I use the published 1.3 A as the per-channel worst case. The exact 24 V hot and cold current stays tied to the factory dash number and my valve bench data.

## ECU power parts

| Buy | Fitted | Part | Use |
| ---: | ---: | --- | --- |
| 1 | 1 | TDK-Lambda `I7A4W033A033V-003-R` | 28 V nominal bus to regulated 24 V valve rail |
| 8 | 7 | TI `TPS26633PWPR` | Six assigned valve channels and one disabled spare |
| 1 | 1 | TI `TPS6538600QDCARQ1` | AM2634 sequencing, watchdog, and supply supervision |
| 1 | 1 | TI `TPS62903-Q1` | 1.2 V AM2634 core supply |

The Buschjost coils stay on the regulated 24 V rail. I am not running the 24 V coils straight from the 28 V aircraft bus.

Every valve channel uses `SHDN` as the command and has a 47 kOhm pull-down. `IMON`, `FLT`, `PGOOD`, and output-voltage sensing return to the controller. The first current-limit values are:

| Valve channel | `ILIM` resistor | Typical limit |
| --- | ---: | ---: |
| MFV | 7.15 kOhm | 2.52 A |
| MOV | 6.04 kOhm | 2.98 A |
| CPV and IPV | 24.0 kOhm | 0.75 A |
| IFV and IOV | 11.3 kOhm | 1.59 A |

Each output has its own inductive clamp near the harness connector. The clamp voltage and release timing stay tied to the valve bench data because the coil inductance is not published.

## Backup battery

| Qty | Part | Use |
| ---: | --- | --- |
| 1 | EnerSys CYCLON `0819-0020` | 12 V, 2.5 Ah AGM backup battery |
| 1 | Power-Sonic `PSC-12500ACX` | Ground-service AGM charger |

The battery only carries the controller, CAN, and measurement electronics. It does not carry valves or ignition.

The battery is 113.8 x 89.4 x 70.4 mm, weighs 1.04 kg, and uses 0.187 inch Faston terminals. My enclosure has a separate clamp, covered terminals, a positive-terminal fuse, reverse-polarity protection, current measurement, automatic switchover, and low-voltage cutoff.

## Part status

- AST20PT is an active TE family. The selected configuration still goes through a TE or LADD quote.
- The Phoenix M12 plug, Omega connectors, MAX31856, TPS26633, TI power parts, i7A, and CYCLON battery are active parts.
- The two Buschjost main valves are made to order and have public STEP models.
- The Parker G7 is the released GN2 purge valve and has manufacturer CAD.
- The Valcor V19800 family is the released IFV/IOV choice for this pass. The factory dash number and fluid approval come back with the quote.

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
- [Buschjost material table](https://www.buschjostventile.de/api/pdf/resistance-table?locale=en)
- [Buschjost position sensor](https://www.buschjostventile.de/api/attachments/93eacf07-07ba-4c1d-8806-5cbe40241766.pdf)
- [Parker G7 catalog](https://www.parker.com/content/dam/Parker-com/Literature/Fluid-Control-Division/Catalogs/Parker_FCD_Catalog-G7.pdf)
- [Parker G7 CAD](https://parker-embedded.partcommunity.com/3d-cad-models/sso/?info=parkerfcd%2Fg720series20solenoid20valves%2Fg720series20solenoid20valves.prj)
- [Valcor V19800](https://www.valcor.com/valcor-technical-datasheets/2-way-direct-acting-shut-off-solenoid-valve-V19800.pdf)
- [Amphenol valve plug](https://www.digikey.com/en/products/detail/amphenol-industrial-operations/MS3106A10SL-4S/378636)
- [EnerSys CYCLON](https://www.enersys.com/en-gb/products/batteries/cyclon/cyclon/)
- [TDK-Lambda i7A](https://product.tdk.com/en/search/power/switching-power/dc-dc-converter/info?part_no=I7A4W033A033V-003-R)
- [TI TPS2663](https://www.ti.com/product/TPS2663)
- [TI TPS653860-Q1](https://www.ti.com/product/TPS653860-Q1)
- [TI TPS62903-Q1](https://www.ti.com/product/TPS62903-Q1)
