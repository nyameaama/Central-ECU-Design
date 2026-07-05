# Central ECU Design

An engine-mounted Rocket Engine ECU (East Zonal ECU) for commanding, monitoring, and protecting the local propulsion hardware of the Converse Engine.

This project is based on [nyameaama/Converse-Engine](https://github.com/nyameaama/Converse-Engine). It develops the engine-side electronics needed to connect that mechanical engine design to the vehicle control system: valve actuation, pressure and temperature acquisition, ignition control, local safety interlocks, power management, and fault-aware CAN communications.

## Engine Design

| Exterior view                                                       | Section view                                                       |
| ------------------------------------------------------------------- | ------------------------------------------------------------------ |
| ![Converse Engine exterior rendering](assets/SCR-20260704-pyvc.png) | ![Converse Engine section rendering](assets/SCR-20260704-pzeb.png) |

## Technical Requirements

### Rocket Engine ECU — Engine Mounted

The ECU will be mounted directly on the engine and will control only local engine-mounted devices. It must acquire engine telemetry, enforce local inhibits, report faults, and exchange commands and status with the tank zone and vehicle controller.

### Power

- Input power from external 48 V bus
- Internal 12 V valve rail
- Internal 5 V sensor rail
- Internal 3.3 V logic rail
- Valve power cutoff / inhibit
- Voltage and current sensing for local ECU rails
- Protected output power to local engine-mounted devices only

### Valves

- MFV: Main Fuel Valve
- MOV: Main Oxidizer Valve
- Fuel Inlet Isolation Valve
- Oxidizer Inlet Isolation Valve
- FPV: Fuel-Side Purge Valve
- OPV: Oxidizer-Side Purge Valve
- CPV: Chamber / Injector Purge Valve
- Oxidizer Bleed / Chilldown Valve
- FBV: Fuel Bleed Valve
- IFV: Igniter Fuel Valve
- IOV: Igniter Oxidizer Valve
- IPV: Igniter Purge Valve
- FDV: Fuel Manifold Dump Valve
- ODV: Oxidizer Manifold Dump Valve

### Valve Driver Inteface

- Solenoid driver circuits
- Per-channel voltage sensing
- Per-channel current sensing
- Open-load detection
- Short-circuit detection
- Flyback / inductive clamp protection
- Global valve power cutoff
- Valve rail overvoltage / undervoltage protection

### Pressure Transducer Inputs

- Main fuel inlet pressure
- Main oxidizer inlet pressure
- Fuel manifold pressure
- Oxidizer manifold pressure
- Chamber pressure
- Igniter fuel pressure
- Igniter oxidizer pressure
- Purge pressure

### Temperature Inputs

- Chamber wall thermistors / thermocouples
- Injector temperature
- Fuel inlet temperature
- Oxidizer inlet temperature
- Valve body temperatures
- ECU board temperature
- Valve driver temperature
- Cooling jacket inlet/outlet temperature

### Ignition

- Igniter power driver -> electrical igniter
- Igniter current sensing
- Igniter voltage sensing
- Igniter enable/inhibit circuit

### Engine Local Safety Inputs

- Engine arm/safe input
- Local abort / inhibit input
- Valve power enable feedback
- Igniter enable feedback
- Service connector interlock

### Zone Communications

- CAN / CAN FD controller
- CAN transceiver
- Tank zone command interface
- Tank zone telemetry interface
- Vehicle controller command interface
- Heartbeat monitoring
- CAN timeout detection
- Fault reporting
