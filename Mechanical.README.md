# Mobility and Mechanical Design

Back to navigation: [README](./README.md)

## Mechanical Architecture

| Subsystem | Selected solution | Rationale |
|---|---|---|
| Chassis | `CHASSIS_TYPE` | Stable center of mass, easy service access, rigid mounting points |
| Drive | `RWD ` + `MOTOR_MODEL` | Meets acceleration and top-speed targets on WRO track |
| Steering | `Lego spike PRIME medium motor` + steering linkage type | Repeatable steering angle and low backlash |
| Tires | `We chose Spike Prime wheels (56mm diameter, 14mm thickness) for their compactness, grip on vinyl, and familiarity. Larger wheels would make the robot harder to control and take too much space; smaller wheels would complicate the design and lower ground clearance.` | Grip and consistency on plywood surface |

## Key Geometry

| Parameter | Value |
|---|---|
| Wheelbase | `XX mm` |
| Track width | `XX mm` |
| Ground clearance | `XX mm` |
| Robot mass (ready to run) | `XX g` |

## Design Decisions and Trade-offs

|Design decisions|` We choose lightweight frame because : Better Dynamics: According to Newton's second law (F=ma), lower mass yields higher acceleration and sharper braking. It also reduces centrifugal force, allowing the robot to take sharp turns at higher speeds without drifting.Faster Steering Response: A lighter chassis reduces friction and load on the steering motor. This minimizes latency between the computer vision command and the physical wheel turn, which is critical for precise lane tracking and obstacle avoidance.Power Stability: Heavy robots draw peak currents during acceleration, causing battery voltage drops that can crash or reboot sensitive . A lightweight frame ensures stable electronics and extends battery life.` |

- Lightweight frame was prioritized over heavy damping to keep steering response fast.
- Motor mounting was redesigned to reduce shaft misalignment under load.
- Steering linkage was shortened to improve turn-in precision at high speed.

## Torque and Speed Reasoning

- Target lap profile: `TARGET_LAP_TIME` with stable lane holding.
- Final drive ratio: `RATIO`.
- Measured no-load wheel speed: `RPM`.
- Estimated linear speed: `V = wheel_circumference * RPM / 60`.
- Measured stall/current limit margins are documented in [Power and Sensor Architecture](./02-power-sensors.md).




## Steering architecture 
|Steering architecture selesction| `We choose acerman steering geometry for the best steering perfomance , this steering system also contains stability while making turn . We came for this decision after testing paralell steering geometry system on our v1 robot . Tests have shown that parallel steering geometry has a poor steering angle compared to Ackerman steering geometry. `|

## Center of mass
|Center-of-mass| ` We placed the robot's center of mass in the rear to reduce friction on the front wheels and add more friction to the rear wheels for confident acceleration.`|

## Sensor architecture 
|Number of sensors and their placement| `We installed three infrared distance sensors, one on the front, right and one on the left, for continuous distance analysis, which we use to track the distance to obstacles/walls. We also use this data and the camera data to build odometry. Moreover sensors placed closer to the ground to see beter walls and have more accuracy` |

## Speed reasoning
|Speed reasoning| ` To understand which speed is most beneficial to use to minimize slippage and show a consistently good result, we conducted a couple of tests and changed the speeds in our code to understand at what point the wheels begin to slip, and after that we used a speed of 500 degrees per second.`|
 


## Mechanical Artifacts

- Chassis diagram: ![Chassis diagram](./assets/images/chassis-diagram.png)
- CAD/STL files location: `models/`
- Assembly notes location: `other/assembly-notes.md`

## Reproducibility Notes

A new team can rebuild the mechanical platform if the following are provided:

- Full CAD or dimensioned drawing set.
- BOM with exact motor/servo/tire references.
- Mounting orientation notes for sensors and controller.
- Steering neutral calibration reference.
