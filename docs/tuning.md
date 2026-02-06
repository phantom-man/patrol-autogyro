# Tuning Guide

## Engine Tuning

### RCGF 10cc Carburetor Adjustment

The Walbro-style carburetor has two adjustment needles:

- **H (High)**: Controls fuel mixture at full throttle
- **L (Low)**: Controls fuel mixture at idle/low throttle

#### Initial Settings

1. Turn both needles clockwise until lightly seated
2. Back out H needle 1.5 turns
3. Back out L needle 1.25 turns

#### Fine Tuning

1. Start engine and let warm up (2-3 minutes)
2. Adjust L needle for smooth idle
3. Advance to full throttle, adjust H for max RPM
4. Back off H needle 1/8 turn (rich for safety)
5. Verify smooth transition from idle to full throttle

### Break-in Procedure

1. First 2 tanks: Run slightly rich, avoid sustained full throttle
2. Tanks 3-5: Gradually lean mixture, allow brief full throttle
3. After 5 tanks: Normal operation, final carburetor tuning

## ArduPilot Tuning

### Throttle Calibration

```
# Verify servo range matches carburetor travel
SERVO1_MIN = 1000    # Throttle fully closed
SERVO1_MAX = 2000    # Throttle fully open
SERVO1_TRIM = 1100   # Reliable idle
```

### RPM Sensor Verification

1. Connect to Mission Planner
2. View RPM1 value on HUD
3. Compare to tachometer reading
4. Adjust RPM1_SCALING if needed

### PID Tuning

*Autogyro-specific PID tuning coming soon*
