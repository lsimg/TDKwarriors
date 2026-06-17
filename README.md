# TDKwarriors — WRO Future Engineers 2026

Welcome to the official repository of **Team TDKwarriors** for **WRO Future Engineers 2026**.

This repository contains the complete engineering documentation, software, CAD files, wiring diagrams, and development history of our autonomous robot vehicle.

---

# Team

## Members

* **Mussabek Nurtas**
* **Mukhtar Gulsim**

## Team Photo

![Team photo](./assets/images/team-official.png)

---

# Project Overview

Our robot is a compact autonomous vehicle designed for the **WRO Future Engineers 2026** competition.

The robot is capable of completing both competition tasks:

### Open Challenge

* Complete 3 autonomous laps
* Maintain stable lane following
* Navigate using distance sensors

### Obstacle Challenge

* Detect red and green pillars
* Choose the correct passing side
* Avoid obstacles
* Recover to the driving line
* Complete 3 laps
* Perform autonomous parking

---

# Robot Photos

## Front View

![Vehicle front](./assets/images/vehicle-front.png)

## Back View

![Vehicle back](./assets/images/vehicle-back.png)

## Left View

![Vehicle left](./assets/images/vehicle-left.png)

## Right View

![Vehicle right](./assets/images/vehicle-right.png)

## Top View

![Vehicle top](./assets/images/vehicle-top.png)

## Bottom View

![Vehicle bottom](./assets/images/vehicle-bottom.png)

---

# Mobility and Mechanical Design

## Chassis and Drive System

The robot uses a LEGO Technic based chassis with rear-wheel drive and front-wheel steering.

For steering, we selected the **SPIKE Medium Motor** because it provides significantly higher positioning accuracy compared to the Powered Up motor.

### Engineering Trade-Off

| Option             | Advantage               | Disadvantage               |
| ------------------ | ----------------------- | -------------------------- |
| SPIKE Medium Motor | High steering precision | Lower torque               |
| Powered Up Motor   | Higher torque           | Lower positioning accuracy |

Since steering precision is critical for lane following and parking, we prioritized accuracy over power.

---

## Mechanical Evolution

During development, the robot underwent multiple design revisions.

### Version 1

* Original steering mechanism
* Wider chassis
* Standard wheels

### Version 2

After testing, we discovered that the original steering mechanism produced inconsistent turns.

Changes:

* Returned to a parallel steering system
* Removed steering gears to eliminate backlash
* Improved steering zero-position accuracy
* Shifted center of mass toward the rear

### Version 3

Further improvements included:

* Reduced chassis width
* Improved maneuverability
* Installed silicone wheels
* Improved traction on the competition field
* Finalized OpenMV-Arduino-Technic Hub architecture

---

## Mechanical Layout

### Chassis Design

![Chassis CAD](./assets/images/chassis-cad.png)

### Steering System

![Steering system](./assets/images/steering-system.png)

---

## Testing and Improvements

| Test                    | Observation                          | Improvement              |
| ----------------------- | ------------------------------------ | ------------------------ |
| Steering Accuracy Test  | Inconsistent turning behavior        | Parallel steering system |
| Traction Test           | Wheel slip during turns              | Silicone wheels          |
| Parking Test            | Wide chassis reduced maneuverability | Narrower chassis         |
| Sensor Calibration Test | Position drift detected              | Wall-based calibration   |

---

# Power and Sensor Architecture

## Power Distribution

The robot is powered by two 4V lithium batteries.

Most electronic components operate at 3.3V, therefore we use a buck converter to efficiently regulate the voltage.

Advantages of this solution:

* Improved efficiency
* Reduced power loss
* Compact installation
* Better battery utilization

The converter is mounted below the Arduino on a custom 3D-printed bracket.

---

## Power Architecture Diagram

![Power architecture](./assets/images/power-diagram.png)

---

## Wiring Diagram

![Wiring Diagram](./assets/Wire.jpeg)

---

# Sensor Placement

The robot uses:

* OpenMV Camera
* Left Distance Sensor
* Front Distance Sensor
* Right Distance Sensor

![Sensor placement](./assets/images/sensor-placement.png)

---

## Sensor Placement Trade-Off

The side and front sensors are mounted at 90-degree angles.

### Advantages

#### Simpler Mathematics

Sensor readings directly correspond to wall distance.

No projection calculations are required.

#### Direction Detection

The robot can determine driving direction based on which wall disappears first from the sensor field of view.

### Disadvantages

This arrangement creates blind spots around 45 degrees.

We accepted this compromise because it greatly simplifies navigation calculations.

---

# Calibration Method

After extensive testing, we determined that wall-based calibration provides the most reliable performance.

The robot continuously maintains an optimal distance from the inner wall.

---

## Calibration Procedure

1. Place the robot parallel to a wall.
2. Read left and right sensor values.
3. Set target distance.
4. Calculate position error.
5. Apply steering correction.
6. Repeat continuously during operation.

---

## Calibration Flowchart

![Calibration Flowchart](./assets/images/calibrateflowchart.png)

---

# Software Architecture

## System Overview

The robot consists of three main processing units:

* OpenMV Camera
* Arduino Mega
* LEGO Technic Hub

---

## Open Challenge Data Flow

1. Distance sensors collect measurements.
2. Arduino processes sensor data.
3. Arduino sends data to the Technic Hub using LumpDeviceBuilder.
4. Technic Hub calculates navigation decisions.
5. Hub sends commands to motors.

---

## Obstacle Challenge Data Flow

1. OpenMV detects colored pillars.
2. OpenMV determines obstacle direction.
3. OpenMV sends command to Arduino.
4. Arduino forwards command to Technic Hub.
5. Technic Hub performs avoidance maneuver.

---

## Software Architecture Diagram

![Software Architecture](./assets/images/software-architecture.png)

---

## Open Challenge Flow

![Open Challenge flowchart](./assets/images/open-challenge-flowchart.png)

---

## Obstacle Challenge Flow

![Obstacle Challenge flowchart](./assets/images/obstacle-challenge-flowchart.png)

---

## Software State Machine

The software is organized around the following states:

* INIT
* CALIBRATE
* WAIT_CAMERA
* DRIVE
* DETECT
* AVOID
* RECOVER
* FINISH

![State machine](./assets/images/state-machine.png)

---

# Why We Use Arduino as a Communication Bridge

After multiple experiments, we found that OpenMV communicates more reliably with Arduino than directly with the Technic Hub.

Benefits:

* Better compatibility
* Simpler debugging
* More stable communication
* Easier software integration

The Arduino then transfers information to the Technic Hub through the LumpDeviceBuilder library.

---

# Systems Thinking and Engineering Decisions

## Constraints

During development we faced several engineering constraints:

* Limited available space
* Processing power limitations
* Weight distribution requirements
* Sensor field-of-view restrictions
* Competition time constraints

---

## Design Trade-Offs

### Steering Accuracy vs Motor Power

Benefit:

* Higher steering accuracy

Cost:

* Lower torque

### Sensor Simplicity vs Blind Spots

Benefit:

* Easier calculations

Cost:

* 45° blind zones

### Buck Converter vs Resistors

Benefit:

* Higher efficiency

Cost:

* Slightly more hardware complexity

---

# Bill of Materials (BOM)

| Component          | Quantity |
| ------------------ | -------- |
| LEGO Technic Hub   | 1        |
| SPIKE Medium Motor | 1        |
| Drive Motor        | 1        |
| Arduino Mega       | 1        |
| OpenMV H7 Plus     | 1        |
| Distance Sensors   | 3        |
| Buck Converter     | 1        |
| Lithium Batteries  | 2        |

---

# Reproducibility

This repository contains all information required to reproduce our robot.

Included:

* Source Code
* CAD Files
* Wiring Diagrams
* Software Documentation
* Calibration Method
* Build Instructions

---

# Build Instructions

## Step 1

Assemble the chassis according to the CAD files.

## Step 2

Install steering and drive motors.

## Step 3

Mount all sensors.

## Step 4

Install Arduino and OpenMV.

## Step 5

Connect all electronics according to the wiring diagram.

## Step 6

Upload software to OpenMV, Arduino and Technic Hub.

## Step 7

Perform calibration.

## Step 8

Run validation tests.

---

# Repository Structure

```text
/
├── assets/
│   ├── images/
│   └── Wire.jpeg
│
├── CAD/
│   ├── chassis
│   ├── sensor_mount
│   └── electronics_mount
│
├── Code/
│   ├── Arduino/
│   ├── OpenMV/
│   └── TechnicHub/
│
├── Documentation/
│
└── README.md
```

---

# Current Repository Status

The repository currently contains:

* Robot photos
* Team photo
* Sensor placement
* Wiring diagram
* Flowcharts
* State machine
* Engineering documentation

Future updates may include:

* Additional test results
* Performance metrics
* New robot revisions

---

# Goal of This Repository

The purpose of this repository is to document the complete development process of our WRO Future Engineers 2026 robot, provide reproducible engineering documentation, and demonstrate the evolution of our design from concept to competition-ready autonomous vehicle.
