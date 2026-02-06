# Patrol Autogyro

**Open-source gas-powered patrol autogyro using RCGF 10cc engine with ArduPilot flight controller**

![Status](https://img.shields.io/badge/status-development-yellow)
![License](https://img.shields.io/badge/license-MIT-blue)

## Project Overview

This project aims to develop a long-endurance patrol/surveillance autogyro platform powered by a gasoline engine. Autogyros offer unique advantages for patrol missions:

- **Long endurance**: Gas engines provide 2-4+ hour flight times
- **Low stall speed**: Safe, stable flight characteristics
- **Simple mechanics**: Unpowered rotor means fewer failure modes
- **Quiet operation**: Lower noise signature than multicopters

## Engine: RCGF 10cc RE Rear Exhaust

### Specifications

| Parameter | Value |
|-----------|-------|
| **Model** | RCGF 10cc RE (Rear Exhaust) |
| **Displacement** | 10cc (0.61 cu in) |
| **Type** | Single cylinder, 2-stroke |
| **Power Output** | ~1.2 HP @ 9,000 RPM |
| **Weight** | ~350g (engine only) |
| **Fuel** | 87-93 octane gasoline + 2-stroke oil (25:1 to 32:1) |
| **Ignition** | CDI electronic ignition |
| **Carburetor** | Walbro-style diaphragm |
| **Starting** | Pull start / Electric starter optional |
| **Propeller Range** | 12x6 to 14x7 (pusher configuration) |

### Engine Features

- Rear exhaust design keeps exhaust away from fuselage
- Lightweight aluminum construction
- Reliable CDI ignition (4.8V-6V input)
- Easy carburetor adjustment (H/L needles)

## Flight Controller: ArduPilot

This project uses **ArduPilot** firmware on a Pixhawk-compatible flight controller. ArduPilot's robust autogyro and ICE (Internal Combustion Engine) support makes it ideal for this application.

### Recommended Hardware

- **Flight Controller**: Pixhawk 4, Cube Orange, or Matek H743
- **GPS**: M8N/M9N with compass
- **Telemetry**: 915MHz/433MHz SiK radio or LTE modem
- **RPM Sensor**: Hall effect or optical
- **Kill Switch Relay**: 5V relay module for CDI kill

### ArduPilot Pinout for Gas Engine

| Function | ArduPilot Output | Notes |
|----------|------------------|-------|
| **Kill Switch** | AUX5 (SERVO13) | Relay to CDI ground (kills spark) |
| **RPM Sensor** | AUX6 (SERVO14) | Hall effect on propeller |
| **Throttle Servo** | MAIN1 | Standard servo to carburetor |
| **Choke Servo** | AUX1 (optional) | For cold start automation |
| **Starter Motor** | AUX2 (optional) | Electric starter relay |

### Key ArduPilot Parameters

```
# ICE (Internal Combustion Engine) Configuration
ICE_ENABLE = 1              # Enable ICE support
ICE_START_CHAN = 7          # RC channel for engine start
ICE_OPTIONS = 0             # Options bitmask
ICE_PWM_STRT_ON = 1800      # PWM to start engine
ICE_PWM_IGN_ON = 1800       # PWM for ignition on
ICE_PWM_IGN_OFF = 1100      # PWM for ignition off (kill)

# RPM Sensor Configuration  
RPM1_TYPE = 2               # 2 = Hall effect sensor
RPM1_SCALING = 1.0          # Pulses per revolution
RPM1_PIN = 55               # AUX6 pin number (varies by FC)
RPM1_MIN = 800              # Minimum valid RPM
RPM1_MAX = 12000            # Maximum valid RPM

# Throttle Configuration
SERVO1_FUNCTION = 70        # Throttle function
SERVO1_MIN = 1000           # Throttle closed
SERVO1_MAX = 2000           # Throttle full open
SERVO1_TRIM = 1100          # Idle position

# Kill Switch (Relay on AUX5)
SERVO13_FUNCTION = 0        # Disabled (manual control)
RELAY_PIN = 54              # AUX5 pin number
RELAY_DEFAULT = 0           # Default off (engine can run)

# Autogyro-specific (if using Plane firmware)
Q_ENABLE = 0                # Disable QuadPlane
ARSPD_TYPE = 1              # Airspeed sensor type
THR_MIN = 0                 # Allow zero throttle
THR_MAX = 100               # Maximum throttle %
```

### Wiring Diagram

See [hardware/wiring_diagram.md](hardware/wiring_diagram.md) for detailed pinout.

```
┌─────────────────┐
│   Pixhawk 4     │
├─────────────────┤
│ MAIN1 ──────────┼──► Throttle Servo ──► Carburetor
│ AUX5 (RELAY) ───┼──► Kill Relay ──► CDI Ground
│ AUX6 (RPM) ◄────┼─── Hall Sensor ◄── Propeller Magnet
│ RC IN ◄─────────┼─── Receiver
│ GPS ◄───────────┼─── M9N GPS/Compass
│ TELEM1 ─────────┼──► Telemetry Radio
└─────────────────┘
```

## Safety Considerations

### ⚠️ CRITICAL SAFETY REQUIREMENTS

1. **Kill Switch**: MUST have reliable kill switch that grounds CDI to stop engine
2. **Failsafe**: Configure RTL or engine-kill on RC signal loss
3. **Fuel Safety**: Use appropriate fuel lines, no leaks near ignition
4. **Propeller Guard**: Consider prop guard for ground operations
5. **Fire Extinguisher**: Always have extinguisher on-site
6. **Pre-flight Checks**: Verify kill switch function before every flight

### ArduPilot Failsafe Configuration

```
# Recommended failsafes for gas autogyro
FS_THR_ENABLE = 1           # Throttle failsafe enabled
FS_THR_VALUE = 950          # Failsafe PWM threshold
FS_SHORT_ACTN = 0           # Short failsafe: continue
FS_LONG_ACTN = 1            # Long failsafe: RTL
FS_GCS_ENABLE = 1           # GCS failsafe enabled
```

## FAA Compliance (USA)

### Regulatory Requirements

| Category | Requirement |
|----------|-------------|
| **Part 107** | Commercial operations require Remote Pilot Certificate |
| **Registration** | Required for aircraft >250g (this exceeds) |
| **Marking** | Registration number must be visible |
| **Airspace** | LAANC authorization for controlled airspace |
| **Visual Line of Sight** | Required unless Part 107 waiver obtained |
| **Maximum Altitude** | 400 ft AGL (or higher with waiver) |
| **Operations Over People** | Requires waiver for rotating parts |

### Best Practices

- Always file LAANC authorization via Aloft, Airmap, or DroneZone
- Carry proof of registration during operations
- Log all flights with date, time, location, and duration
- Consider liability insurance for patrol operations

## Project Structure

```
patrol-autogyro/
├── README.md           # This file
├── docs/               # Documentation
│   ├── assembly.md     # Assembly instructions
│   ├── tuning.md       # Engine and flight tuning
│   └── operations.md   # Operational procedures
├── hardware/           # Hardware documentation
│   ├── BOM.md          # Bill of Materials
│   └── wiring_diagram.md
├── firmware/           # ArduPilot configuration
│   ├── params/         # Parameter files
│   └── lua/            # Lua scripts (optional)
└── .gitignore
```

## Resources

### ArduPilot
- [ArduPilot Documentation](https://ardupilot.org/plane/)
- [ICE (Internal Combustion Engine) Support](https://ardupilot.org/plane/docs/common-ice.html)
- [Autogyro Setup](https://ardupilot.org/plane/docs/guide-autogyro.html)
- [RPM Sensor Configuration](https://ardupilot.org/plane/docs/common-rpm.html)

### Engine & RC
- [RCGroups Gas Engines Forum](https://www.rcgroups.com/gas-engines-671/)
- [RCGF Engines](https://www.rcgfusa.com/)
- [Walbro Carburetor Tuning](https://www.rcgroups.com/forums/showthread.php?1345530)

### Regulations
- [FAA Part 107](https://www.faa.gov/uas/commercial_operators/part_107_waivers/)
- [FAA DroneZone Registration](https://faadronezone.faa.gov/)
- [LAANC Authorization](https://www.faa.gov/uas/programs_partnerships/data_exchange)

### Research
- [NASA Technical Reports - Autogyro Aerodynamics](https://ntrs.nasa.gov/search?q=autogyro)
- [Rotorcraft Research](https://ntrs.nasa.gov/search?q=rotorcraft%20UAV)

## Contributing

Contributions welcome! Please:

1. Fork the repository
2. Create a feature branch
3. Submit a pull request with clear description

## License

MIT License - See [LICENSE](LICENSE) for details.

## Disclaimer

This project is for educational and hobbyist purposes. Building and operating gas-powered aircraft involves significant risks. Always:

- Follow all applicable laws and regulations
- Prioritize safety in design and operation
- Test thoroughly in controlled conditions
- Maintain appropriate insurance coverage

**The authors assume no liability for damages or injuries resulting from use of this information.**
