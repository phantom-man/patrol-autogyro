# ArduPilot Parameter Files

## Files

- **base_params.txt** - Base parameters for patrol autogyro

## Usage

### Mission Planner

1. Connect to flight controller
2. Go to **Config** > **Full Parameter List**
3. Click **Load from file**
4. Select the parameter file
5. Click **Write Params**
6. Reboot flight controller

### MAVProxy

```
param load base_params.txt
```

## Customization

These are baseline parameters. You will need to adjust:

- PID gains for your specific airframe
- Servo directions and endpoints
- RPM sensor scaling if using multi-magnet
- Failsafe actions based on your requirements

## Important Notes

- Always test kill switch function before flight
- Verify servo directions on the ground
- Calibrate radio before first flight
- Check RPM reading against tachometer
