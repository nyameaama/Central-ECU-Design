# Propellant Line Sizing

The source reports are in [Engine Data/Nozzle-CC/Propane_NO2_design](<Engine%20Data/Nozzle-CC/Propane_NO2_design/>). The [first design P&ID](Engine%20Data/Nozzle-CC/Propane_NO2_design/converse.jpg) is the layout I am carrying into the thruster.

The tank side hands the thruster propane, N2O, and regulated GN2. Tank isolation, pressurization, venting, draining, relief, and bulk filtration stay upstream of the thruster boundary. The engine side only carries the two main paths, the two torch tapoffs, CPV, and IPV.

## Main flow

| Stream | Mass flow | Density in this pass | Volume flow |
| --- | ---: | ---: | ---: |
| Propane | 0.39879 kg/s | 500 kg/m3 | 0.79758 L/s, 12.64 US gpm |
| N2O | 1.75468 kg/s | 750 kg/m3 | 2.33957 L/s, 37.08 US gpm |
| Total | 2.15347 kg/s | - | - |

The full-thrust O/F is 4.400. These mass flows come from `Converse-engine.txt`. The density values are first-pass numbers and stay tied to the actual tank outlet pressure and temperature.

## Bore check

I used:

`Q = mass flow / density`

`ID = sqrt(4 Q / (pi velocity))`

| Mean velocity | Propane ID | N2O ID |
| ---: | ---: | ---: |
| 3 m/s | 18.4 mm, 0.724 in | 31.5 mm, 1.241 in |
| 5 m/s | 14.3 mm, 0.561 in | 24.4 mm, 0.961 in |
| 7 m/s | 12.0 mm, 0.474 in | 20.6 mm, 0.812 in |
| 10 m/s | 10.1 mm, 0.397 in | 17.3 mm, 0.680 in |

The first mechanical interfaces are G1/2 to -8 on fuel and G3/4 to -12 on N2O. The Buschjost valves are local restrictions at DN10 fuel and DN20 oxidizer.

My first valve-drop estimates are:

- MFV: 9.5 psi
- MOV: 16.7 psi

The complete pressure-drop model includes the actual tube IDs, bends, adapters, valve curves, injector requirement, and the cooling-jacket loss if the fuel path uses the jacket. The N2O side also carries margin against flashing through the line and MOV.

## Torch branches

IFV and IOV tap off upstream of MFV and MOV. Their PT/TT points are downstream of the torch valves and upstream of the torch metering points.

The torch flow is a separate low-flow calculation from the main CEA mass flow. The first hardware layout keeps both torch valves independent and uses the downstream pressure and temperature readings with the final metering geometry.

## Purge branches

Regulated GN2 enters at one thruster interface and splits to CPV and IPV.

- CPV feeds the main chamber purge
- IPV feeds the torch chamber purge
- both purge PT/TT points are downstream of their valves
- IPV is on during the main burn after IFV and IOV close
- CPV is a post-fire function

Purge flow is set by the tank-side regulated pressure and the engine-side metering geometry. There are no engine-side propellant vent or manifold dump paths in this design.
