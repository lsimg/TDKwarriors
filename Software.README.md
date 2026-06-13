# Software Architecture and Control Logic

Back to navigation: [README](./README.md)

##  Software Stack
| Component | Technology | Rationale |
|---|---|---|
| Language | Python (Pybricks) | Native hardware support, fast performance |
| Control Loop | 100 Hz | Optimal balance between sensor precision and CPU load |
| Localization | Odometry + Distance Sensors | Robustness against drift on long tracks |

## Control Strategy
* **Driving Model:** Pure pursuit path following with feed-forward steering correction.
* **Localization:** Wall-following logic using `DISTANCE_SENSOR_MODE` to compensate for wheel slippage and odometry errors.
* **Safety:** Emergency stop routines triggered by IMU tilt detection and distance sensor timeouts.

## PID Tuning Parameters
KP = 0.5
KI = 0.0
KD = 0.2/0.1

##  Design Decisions
- **Non-blocking Execution:** The use of `wait()` and generator-based actions ensures sensor polling and odometry calculation are never delayed.
- **Fail-safe states:** If sensor data is invalid (out of range), the robot defaults to a pre-defined "safe-track" velocity.
- **Config-driven:** All constants are stored in `fe_config.py` for rapid tuning without touching core algorithms.

##  Logic Flow Summary##  Iteration Log
| Version | Change | Why | Result |
|---|---|---|---|
| v1 | Basic PID drive | Simple straight line testing | High oscillation at high speed |
| v2 | Added D-term to Steering | Reduce overshoot | Smoother lane keeping |
| v3 | Implemented Sensor Localization | Correct odometry drift | Consistent laps without crashing |

##  Implementation Notes
- **Calibration:** `STEER_CENTER` must be calibrated every battery change.
- **Sensor Filtering:** Moving average filter is applied to `distance_sensor` input to ignore noise from track joints.
- **Debugging:** Use `robot.print_pose_action()` to visualize the internal map state during test runs.

##  Reproducibility
To ensure the software behaves identically on a different hub:
1. Ensure firmware version matches the competition requirements.
2. Calibrate `WHEEL_DIAMETE` and `DRIVE_GEAR_RATIO` precisely (1mm error creates significant track drift).
3. Verify motor orientation and `STEER_INVERTED` settings in `fe_config.py`.

## Code first versions 
in the code folder
