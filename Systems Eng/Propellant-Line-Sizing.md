# Propellant Line Sizi

The copied inputs and reports are in [Engine Data/Nozzle-CC/Propane_NO2_design](<Engine%20Data/Nozzle-CC/Propane_NO2_design/>).

## Design flow

| Stream  |    Mass flow | Density used for this pass |               Volume flow |
| ------- | -----------: | -------------------------: | ------------------------: |
| Propane | 0.39879 kg/s |                  500 kg/m3 | 0.79758 L/s, 12.64 US gpm |
| N2O     | 1.75468 kg/s |                  750 kg/m3 | 2.33957 L/s, 37.08 US gpm |
| Total   | 2.15347 kg/s |                          - |                         - |

O/F is 4.400. These are full-thrust values from `Converse-engine.txt`.

The densities are the same first-pass values used in the parts list. They need to be replaced with properties at the released tank and valve inlet pressure and temperature.

## Bore check

CEA gives mass flow. It does not size the feed lines. For a first pass:

`Q = mass flow / density`

`ID = sqrt(4 Q / (pi velocity))`

| Mean velocity |        Propane ID |            N2O ID |
| ------------: | ----------------: | ----------------: |
|         3 m/s | 18.4 mm, 0.724 in | 31.5 mm, 1.241 in |
|         5 m/s | 14.3 mm, 0.561 in | 24.4 mm, 0.961 in |
|         7 m/s | 12.0 mm, 0.474 in | 20.6 mm, 0.812 in |
|        10 m/s | 10.1 mm, 0.397 in | 17.3 mm, 0.680 in |

The existing Buschjost valves are local restrictions at DN10 fuel and DN20 oxidizer. The current G1/2 to -8 fuel and G3/4 to -12 oxidizer adapters also put the lines toward the high end of the velocity table once the tube wall is included. Their estimated valve drops are already 9.5 psi fuel and 16.7 psi oxidizer.

The final tube sizes need one pressure-drop pass with the actual tube IDs, lengths, bends, fittings, filters, valve curves, inlet fluid state, injector inlet requirement, and shutdown transient. The N2O line also needs margin against flashing as pressure falls through the line and valve.
