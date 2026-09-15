# Central ECU

This is the engine-side ECU for the [Converse Engine](https://github.com/nyameaama/Converse-Engine). The engine is packaged as a thruster, so I am keeping everything on this side as small as I can.

Tank isolation, pressurization, regulation, relief, venting, filling, draining, bulk filtration, and service plumbing are all on the tank side. The thruster gets propane, N2O, regulated GN2, 28 V power, and CAN. The Central ECU runs the local firing sequence, drives the six engine valves and spark igniter, reads the engine sensors, and reports back to the vehicle over CAN.

## First design P&ID

![First design P&ID](Systems%20Eng/Engine%20Data/Nozzle-CC/Propane_NO2_design/converse.jpg)

This is the first design I am locking down. MFV and MOV are the main engine shutoff valves. IFV and IOV tap off upstream of them so the torch can light before the main valves open. The igniter fuel and oxidizer PT/TT pickups are downstream of IFV and IOV.

The GN2 arriving at the thruster is already regulated. CPV handles the main chamber purge and IPV handles the torch chamber purge. All six fluid valves are normally closed.

## Engine

| Engine | Section view |
| --- | --- |
| ![Converse Engine exterior](assets/SCR-20260704-pyvc.png) | ![Converse Engine section](assets/SCR-20260704-pzeb.png) |

## Current enclosure

![First ECU enclosure](assets/SCR-20260705-bmch.png)

This is the first enclosure pass. I am using it to get a real PCB outline and connector layout.

- 18 mm main body height
- 23 mm max height
- R2 top edge
- R39 PCB keepout
- R6 outer corners with the smaller R9, R7, and R3 transitions from the CAD
- two 9.5 mm engine mounting holes
- 5.5 mm PCB mounting holes
- separate battery clamp with covered terminals and service access
- sealed lid and connector openings

## Power

- 28 V nominal aircraft bus
- regulated 24 V valve rail
- 12 V ignition and sensor rails
- 5 V analog and communications rail
- 3.3 V logic rail
- EnerSys CYCLON `0819-0020`, 12 V 2.5 Ah backup battery

The backup battery only keeps the controller, CAN, and sensor measurements alive. It does not run the valves or igniter. Loss of valve power closes every valve.

## Valves

| Tag | Function | First design part |
| --- | --- | --- |
| MFV | Main propane | Buschjost `2/918-69/0824/.272-GO-1E` |
| MOV | Main N2O | Buschjost `2/918-24/0824/.272-GO-1E` |
| IFV | Torch fuel | Valcor V19800 |
| IOV | Torch oxidizer | Valcor V19800 |
| CPV | Main chamber purge | Parker `20CC04EP7D7B` |
| IPV | Torch chamber purge | Parker `20CC04EP7D7B` |

I have seven identical `TPS26633PWPR` valve channels on the PCB. Six are assigned above and the seventh stays disabled as a spare. `SHDN` is the command for each normally closed valve. Every channel also returns current, fault status, power-good status, and output voltage.

The igniter uses two separate solenoid valves in this build.

## Measurements

I have eleven AST20PT mounting points:

- fuel before and after MFV
- N2O before and after MOV
- igniter fuel after IFV
- igniter oxidizer after IOV
- chamber purge after CPV
- igniter purge after IPV
- remote chamber pressure
- cooling jacket inlet and outlet

The line sensors give pressure and fluid temperature from the same port. Four chamber-wall thermocouples and one injector thermocouple handle the hot structure. ECU and valve-driver temperatures stay on the PCB.

## Firing sequence

The first sequence is:

1. Spark igniter on.
2. IFV and IOV open.
3. Torch pressure comes up.
4. MFV and MOV open.
5. Main chamber pressure comes up.
6. IFV and IOV close.
7. IPV opens and protects the torch during the main burn.
8. MFV and MOV close at shutdown.
9. CPV and IPV handle the post-fire purge.

The largest valve state is the four-valve ignition overlap with MFV, MOV, IFV, and IOV energized together.

## Safety and communications

- arm and safe
- abort and hardware inhibit
- valve power feedback
- igniter enable feedback
- service connector interlock
- CAN FD to the tank and vehicle controllers
- heartbeat and timeout handling
- fault reporting

The tank controller provides the upstream emergency isolation path if an engine valve fails open.

Parts, interfaces, and the power numbers are in [Systems Eng](Systems%20Eng/README.md).
