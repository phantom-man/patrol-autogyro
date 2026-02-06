# Wiring Diagram

## System Overview

```
                                    ┌──────────────────┐
                                    │   RCGF 10cc      │
                                    │   Engine         │
                                    ├──────────────────┤
┌─────────────┐                     │ CDI ◄────────────┼─── Kill Relay
│ 2S LiPo     │                     │ Throttle ◄───────┼─── Throttle Servo
│ (Ignition)  ├────► CDI Unit ─────►│ Spark Plug       │
└─────────────┘                     │ Propeller ───────┼──► RPM Sensor
                                    └──────────────────┘

┌─────────────────────────────────────────────────────────────────────┐
│                         PIXHAWK 4                                   │
├─────────────────────────────────────────────────────────────────────┤
│                                                                     │
│  POWER ◄───────────────── Power Module ◄──── 4S LiPo (Avionics)     │
│                                │                                    │
│  MAIN OUT 1 ──────────────────►│ Throttle Servo ──► Carburetor      │
│  MAIN OUT 2 ──────────────────►│ Rudder Servo                       │
│  MAIN OUT 3 ──────────────────►│ Elevator Servo                     │
│  MAIN OUT 4 ──────────────────►│ (Reserved)                         │
│                                │                                    │
│  AUX OUT 1 ───────────────────►│ Choke Servo (optional)             │
│  AUX OUT 2 ───────────────────►│ Starter Relay (optional)           │
│  AUX OUT 5 ───────────────────►│ Kill Switch Relay ──► CDI Ground   │
│  AUX OUT 6 ◄───────────────────│ RPM Sensor ◄── Propeller Magnet    │
│                                │                                    │
│  RC IN ◄───────────────────────│ RC Receiver ◄── RC Transmitter     │
│  GPS ◄─────────────────────────│ M9N GPS Module                     │
│  I2C ◄─────────────────────────│ Compass (in GPS)                   │
│  TELEM1 ───────────────────────│ Telemetry Radio ──► Ground Station │
│  TELEM2 ───────────────────────│ (Reserved for companion computer)  │
│                                                                     │
└─────────────────────────────────────────────────────────────────────┘
```

## Kill Switch Circuit

```
Pixhawk AUX5 ────► 5V Relay Coil ─┐
                                  │
Ground ──────────────────────────►│ Relay
                                  │
CDI Ground Wire ◄── Relay NO ◄───┘

When AUX5 HIGH (>1500µs): Relay energized, CDI grounded, engine STOPS
When AUX5 LOW  (<1500µs): Relay open, CDI normal, engine RUNS
```

## RPM Sensor Circuit

```
5V (from Pixhawk) ────► Hall Sensor VCC
                              │
Pixhawk AUX6 ◄──────── Hall Sensor OUT
                              │
Ground ───────────────► Hall Sensor GND

                        ┌───┐
Propeller ──────────────│ N │ Magnet (1 per revolution)
                        │ S │
                        └───┘
                          │
                     Hall Sensor
```

## Throttle Servo

```
Pixhawk MAIN1 (+) ────► Servo Signal (Orange/White)
Pixhawk MAIN1 (-) ────► Servo Ground (Brown/Black)
Power Rail ───────────► Servo Power (Red) [5-6V]

Servo Arm ──► Push Rod ──► Carburetor Throttle Barrel
```

## Connector Pinouts

### Pixhawk 4 MAIN OUT

| Pin | Function |
|-----|----------|
| 1 | Throttle |
| 2 | Rudder |
| 3 | Elevator |
| 4 | Reserved |
| 5 | Reserved |
| 6 | Reserved |
| 7 | Reserved |
| 8 | Reserved |

### Pixhawk 4 AUX OUT

| Pin | Function |
|-----|----------|
| 1 (SERVO9) | Choke (optional) |
| 2 (SERVO10) | Starter (optional) |
| 3 (SERVO11) | Reserved |
| 4 (SERVO12) | Reserved |
| 5 (SERVO13) | Kill Switch Relay |
| 6 (SERVO14) | RPM Sensor Input |
