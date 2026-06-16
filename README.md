# TDKwarriors — WRO Future Engineers 2026

Welcome to the official repository of **Team TDKwarriors** for **WRO Future Engineers 2026**.

This repository contains the current public documentation of our autonomous robot car project for the 2026 season.

## Team

- **Mussabek Nurtas**
- **Mukhtar Gulsim**

## Team Photo

![Team photo](./assets/images/team-official.png)

## Project Overview

Our project is a compact autonomous robot car built for the **WRO Future Engineers** category.

The robot is designed to complete both competition tasks:

- **Open Challenge** — complete 3 autonomous laps on the track
- **Obstacle Challenge** — detect red and green pillars, pass on the correct side, complete 3 laps, and perform parking

At the current stage, the robot includes:

- front-mounted **OpenMV camera**
- **three distance sensors** (left, front, right)
- steering and rear drive system
- compact LEGO Technic-based vehicle platform
- early flowcharts and software state-machine documentation

## Robot Photos

### Front View
![Vehicle front](./assets/images/vehicle-front.png)

### Back View
![Vehicle back](./assets/images/vehicle-back.png)

### Left View
![Vehicle left](./assets/images/vehicle-left.png)

### Right View
![Vehicle right](./assets/images/vehicle-right.png)

### Top View
![Vehicle top](./assets/images/vehicle-top.png)

### Bottom View
![Vehicle bottom](./assets/images/vehicle-bottom.png)

## Sensor Placement

The robot currently uses:

- **OpenMV camera**
- **left distance sensor**
- **front distance sensor**
- **right distance sensor**

The current placement of the camera and sensors is shown below:

![Sensor placement](./assets/images/sensor-placement.png)

## Open Challenge Flow

The current Open Challenge logic is:

1. Detect track or walls
2. Follow lane or path
3. Complete 3 laps
4. Stop in the finish section

![Open Challenge flowchart](./assets/images/open-challenge-flowchart.png)

## Obstacle Challenge Flow

The current Obstacle Challenge logic is:

1. Detect red or green pillar
2. Choose the correct passing side
3. Avoid obstacle
4. Recover to lane
5. Complete 3 laps
6. Search for parking
7. Perform parallel parking

![Obstacle Challenge flowchart](./assets/images/obstacle-challenge-flowchart.png)

## Software State Machine

The current software is organized around the following main states:

- `INIT`
- `CALIBRATE`
- `WAIT_CAMERA`
- `DRIVE`
- `DETECT`
- `AVOID`
- `RECOVER`
- `FINISH`

![State machine](./assets/images/state-machine.png)

## Current Hardware Summary

The robot currently includes:

- LEGO Technic chassis
- rear drive motor
- front steering motor
- OpenMV camera
- three distance sensors
- onboard electronics and custom mounts

## Current Repository Status

This repository currently contains:

- robot photos
- team photo
- challenge flowcharts
- software state machine
- sensor placement image

More technical documentation will be added later, including:

- wiring diagram
- chassis diagram
- detailed software architecture
- testing metrics
- engineering decisions
- full reproducibility notes

## Goal of This Repository

The purpose of this repository is to document the development of our WRO Future Engineers 2026 robot and show the current progress of our engineering work.And make our robot Reproducibility


## Mobility and Mechanical Design

### Chassis and Drive System

We selected the SPIKE Medium Motor for the steering system because it provides significantly higher positioning accuracy compared to the LEGO Powered Up motor. Precise steering is critical for maintaining stable lane following and accurate turning. The trade-off of this decision is lower torque and power, which we accepted in exchange for improved steering precision.

### Design Evolution

During the development process, the robot underwent several major revisions.

After testing the first version, we discovered that the original steering mechanism did not provide consistent and repeatable turning behavior. As a result, we returned to a parallel steering system that offered more predictable movement.

Additional improvements included:

* Removal of gears from the steering mechanism to reduce backlash and improve steering accuracy.
* Improved center position calibration ("zero position") of the steering system.
* Relocation of the center of mass toward the rear of the robot.
* Reduction of the robot width to improve maneuverability and simplify parking maneuvers.
* Replacement of the original wheels with silicone wheels after testing revealed insufficient traction on the competition field.

### Mechanical Layout

**Mechanical Design Diagram**

*Insert chassis CAD image here.*

**Steering System Diagram**

*Insert steering mechanism diagram here.*

### Testing and Improvements

| Test                   | Observation                                          | Improvement                          |
| ---------------------- | ---------------------------------------------------- | ------------------------------------ |
| Steering accuracy test | Original steering system produced inconsistent turns | Switched to parallel steering system |
| Traction test          | Wheels slipped during acceleration and turning       | Replaced wheels with silicone wheels |
| Parking test           | Wide chassis reduced maneuverability                 | Reduced robot width                  |

---

# Power and Sensor Architecture

## Power Distribution

The robot is powered by two 4V lithium batteries.

Most electronic components in our system operate at 3.3V. Instead of using resistor-based voltage reduction, we chose to use a voltage converter (buck converter) that efficiently regulates the voltage from the batteries to 3.3V.

The converter is mounted underneath the Arduino on a custom 3D-printed component, helping to reduce space usage and improve the overall ergonomics of the robot.

### Power Architecture Diagram

*Insert power distribution diagram here.*

### Wiring Diagram

(./assets/Wire.jpeg)

---

## Sensor Placement and Design Trade-Offs

We positioned our distance sensors on the front, left, and right sides of the robot at 90-degree angles.

This decision was made for several reasons:

### Advantages

#### Simpler Mathematics

The sensors measure distances directly without requiring projection calculations. This simplifies software development and improves processing efficiency.

#### Direction Detection

The side sensors help determine the direction of movement around the track.

For example:

* If the left wall disappears from the left sensor's field of view, the robot can infer the track direction.
* If the right wall disappears first, the robot can determine the opposite direction.

### Disadvantages

This configuration creates blind spots at approximately 45 degrees relative to the robot.

We accepted this limitation because the benefits of simpler calculations and reliable direction detection outweighed the disadvantages.

### Sensor Layout Diagram

*Insert sensor placement diagram here.*

---

## Calibration Method

After extensive testing, we determined that the most reliable calibration method is based on distance sensor measurements.

The robot continuously maintains an optimal distance from the inner wall of the track. By using wall distance measurements as a reference, the robot can accurately adjust its position and maintain a consistent driving path.

### Calibration Procedure

1. Read side sensor values.
2. Compare measured distance to target distance.
3. Calculate position error.
4. Apply steering correction.
5. Repeat continuously during operation.

### Calibration Flowchart

(./assets/images/calibrateflowchart)

---

# Software Architecture and Obstacle Strategy

## System Architecture

### Open Challenge

The system operates as follows:

1. Distance sensors collect environmental data.
2. Arduino receives sensor information.
3. Arduino sends processed data to the Technic Hub using the LumpDeviceBuilder library.
4. The Technic Hub converts the incoming data into numerical values.
5. Navigation algorithms determine the required movement.
6. Motor commands are sent to the drive and steering motors.

### Obstacle Challenge

The obstacle challenge uses the OpenMV camera.

1. The OpenMV camera detects colored objects.
2. The camera determines the required navigation command.
3. The command is sent to the Arduino.
4. Arduino forwards the information to the Technic Hub.
5. The Technic Hub executes the appropriate maneuver.

### Why We Chose Arduino as the Communication Bridge

After multiple experiments, we found that the OpenMV camera integrates more reliably with Arduino than directly with the Technic Hub through a custom communication protocol.

Using Arduino as an intermediary provides:

* More stable communication.
* Easier debugging.
* Better compatibility with OpenMV.
* Simpler software architecture.

The Arduino then communicates with the Technic Hub using the LumpDeviceBuilder library.

### Software Flowchart

*Insert software flowchart here.*

### State Machine

*Insert state machine diagram here.*

---

# Systems Thinking and Engineering Decisions

## Constraints

During development we faced several engineering constraints:

* Limited available space inside the robot.
* Limited processing power on embedded devices.
* Weight distribution requirements.
* Sensor field-of-view limitations.
* Competition time constraints.

## Design Trade-Offs

### Steering Accuracy vs Motor Power

We selected the SPIKE Medium Motor because steering precision was more important than torque.

**Benefit:** Higher steering accuracy.

**Cost:** Reduced steering power.

### Sensor Simplicity vs Blind Spots

We mounted sensors at 90-degree angles.

**Benefit:** Simpler calculations and direction detection.

**Cost:** 45-degree blind zones.

### Voltage Converter vs Resistor-Based Regulation

We chose a buck converter.

**Benefit:** Better efficiency and lower power loss.

**Cost:** Slightly increased hardware complexity.

---

# Version History

## Version 1

* Original steering mechanism.
* Original wheel configuration.
* Wider chassis.

## Version 2

* Parallel steering system introduced.
* Steering gears removed.
* Center of mass adjusted.

## Version 3

* Silicone wheels installed.
* Improved sensor calibration method.
* OpenMV-Arduino-Technic Hub communication architecture finalized.

---

# Reproducibility

This repository contains all information required to reproduce the robot:

* Source code
* CAD files
* Wiring diagrams
* Mechanical drawings
* Calibration procedures
* Build instructions

## Build Instructions

### Hardware Assembly

*Insert assembly guide here.*

### Electronics Assembly

(./assets/Wire.jpeg)


### Calibration Procedure

*Insert calibration instructions here.*

---

# Repository Structure

```text
/
├── CAD/
├── Wiring/
├── Code/
├── Documentation/
├── Images/
└── README.md
```

