<img width="1555" height="821" alt="WhatsApp Image 2026-08-30 at 00 00 28" src="https://github.com/user-attachments/assets/71c4bc31-0bbf-4efd-a893-e356acd725e6" /># Morphobot

**Autonomous Hybrid UGV-UAV Robotic Platform**

Morphobot is a final-year mechatronics project focused on developing a single robotic platform capable of operating as both an **Unmanned Ground Vehicle (UGV)** and a **quadcopter-style Unmanned Aerial Vehicle (UAV)**. The goal is to combine the endurance and efficiency of ground mobility with the obstacle-clearing capability of aerial mobility in one transformable autonomous robot.

The project covers the full development pipeline from **mechanical design, CAD, drivetrain design, 3D-print-oriented prototyping, structural analysis, electronics architecture, embedded control, ROS 2 simulation, autonomous navigation, and eventual sim-to-real deployment**.

---

## 1. Project Objectives

The main objectives of Morphobot are to:

- Design a transformable robot capable of switching between **UGV and UAV modes**.
- Develop the mechanical structure, wheel-leg transformation system, and actuator integration from scratch.
- Validate important structural components using **ANSYS Static Structural analysis**.
- Build a ROS 2 simulation model in **Gazebo** before physical deployment.
- Implement low-level motor, encoder, servo, and sensor control using **STM32**.
- Use a **Raspberry Pi 5** for high-level ROS 2 autonomy and perception.
- Use a **Pixhawk flight controller** for UAV stabilization and autonomous flight.
- Implement **SLAM and autonomous mapping**.
- Investigate **GNSS-denied navigation**.
- Develop **UGV/UAV mode-aware planning**, allowing the robot to decide whether to drive or fly based on terrain, energy, battery state, and route cost.

---

## 2. System Overview

Morphobot is divided into three major control layers:

```text
                 +-----------------------------+
                 |       Raspberry Pi 5        |
                 | ROS 2 / Mapping / Planning  |
                 | SLAM / Perception / Autonomy|
                 +-------------+---------------+
                               |
                  UART / USB / Serial / MAVLink
                               |
             +-----------------+------------------+
             |                                    |
+------------v-------------+         +------------v-------------+
|      STM32 Controller    |         |        Pixhawk           |
| Ground Drive / Encoders  |         | UAV Flight Controller    |
| Servo Transformation     |         | Stabilization / Missions |
| Low-Level Sensors        |         | ESC / BLDC Control       |
+------------+-------------+         +------------+-------------+
             |                                    |
     JGA Motors / Servos                    4-in-1 ESC / BLDCs
```

---

## 3. Mechanical Design

The mechanical design was developed in **SolidWorks** from an original concept. No open-source mechanical model matched the intended transformation mechanism, wheel arrangement, drivetrain, and UAV packaging requirements, so the geometry was developed through iterative CAD design.

Major mechanical subsystems include:

- Four large wheel assemblies.
- One-sided transformable leg structures.
- Servo-driven leg transformation mechanism.
- Wheel hub and axle interfaces.
- Custom bearing and retaining arrangement.
- Ground-drive spur gear transmission.
- Central chassis and electronics mounting structure.
- Battery mounting beneath the central frame.
- Integrated BLDC motor placement for UAV operation.

### Current wheel/drivetrain concept

- Wheel outer diameter: approximately **203 mm**
- Wheel inner diameter: approximately **200 mm**
- Wheel spur gear: **36T, module 1.5, 20° pressure angle**
- Motor pinion under consideration/finalized for prototype: **10T**
- Gear ratio: **36:10 = 3.6:1**
- JGA25-370 motor nominal speed: approximately **280 RPM**
- Estimated wheel speed after reduction: approximately **78 RPM**
- Theoretical ground speed: approximately **0.83 m/s**

### Transformation system

Each leg is actuated by a high-torque servo. The servo rotates the leg between the ground configuration and flight configuration while maintaining the BLDC and wheel geometry required for both modes.

---

## 4. CAD Development

The full robot assembly is being developed in SolidWorks with attention to:

- transformation clearances,
- servo horn positioning,
- gear center distance,
- propeller-to-wheel clearance,
- axle and retaining features,
- fastener access,
- electronics mounting,
- battery placement,
- 3D printing constraints,
- structural fillets and stress concentration reduction.

### CAD Images

> **[PLACE CAD OVERVIEW IMAGE HERE]**

> **[PLACE UGV CONFIGURATION IMAGE HERE]**

> **[PLACE UAV CONFIGURATION IMAGE HERE]**

> **[PLACE LEG / WHEEL ASSEMBLY IMAGE HERE]**

---

## 5. 3D Printing and Prototyping

The design is being prepared for iterative 3D printing so that individual subsystems can be tested before manufacturing the complete platform.

Current prototype priorities include:

- one complete leg,
- one wheel,
- wheel hub,
- gear meshing,
- axle fit,
- servo transformation geometry,
- JGA motor mounting,
- retaining/threaded interfaces.

The goal is to validate fit, stiffness, backlash, assembly sequence, and motor/servo alignment before committing to the full set of components.

### Prototype Images

> <img width="1458" height="754" alt="WhatsApp Image 2026-09-01 at 22 20 44" src="https://github.com/user-attachments/assets/e5537ae6-b7c1-4ee7-a932-c421ec23cd6c" />


> <img width="1040" height="653" alt="WhatsApp Image 2026-09-01 at 22 19 52" src="https://github.com/user-attachments/assets/d8acfb35-2a3a-4114-af10-981b3e4f8bc8" />


> <img width="845" height="765" alt="WhatsApp Image 2026-09-02 at 00 30 06" src="https://github.com/user-attachments/assets/5834e16c-6566-4a90-b8f5-c3275a1e3488" />


---

## 6. Structural Analysis - ANSYS

Critical components are analyzed in **ANSYS Static Structural** before final manufacturing and assembly.

Areas of interest include:

- leg bending under wheel and body loading,
- servo loading during transformation,
- wheel hub and axle stress,
- chassis stiffness,
- external arm/servo mount stress concentrations,
- landing and bump load cases,
- lateral loading during ground motion.

The analysis is used mainly as a design-validation tool to identify weak areas and determine where geometry, wall thickness, fillets, or print settings need improvement.

### ANSYS Results

> <img width="1600" height="666" alt="WhatsApp Image 2026-08-30 at 00 01 06" src="https://github.com/user-attachments/assets/ed0bf18b-d3c4-4c1c-99df-c063fcf8f0bc" />


<img width="1600" height="723" alt="WhatsApp Image 2026-08-30 at 00 16 29" src="https://github.com/user-attachments/assets/6accd29d-dfec-49b0-a5e9-8e13ca368e6c" />

<img width="1600" height="712" alt="WhatsApp Image 2026-08-30 at 00 16 04" src="https://github.com/user-attachments/assets/a186e861-19c2-46a9-87cf-30f450c438b5" />

## 7. Ground Mobility System


Morphobot uses four geared DC motors for ground locomotion.

### Ground-drive components

- **4 × JGA25-370 12 V geared DC motors**
- approximately **280 RPM** motor speed
- quadrature encoders for wheel-speed feedback
- spur gear reduction between motor and wheel
- skid-steer / differential-style ground control

The STM32 will handle:

- PWM generation,
- direction control,
- encoder counting,
- wheel RPM calculation,
- closed-loop speed PID,
- telemetry to the Raspberry Pi.

---

## 8. Transformation Actuation

The transformation mechanism uses **four high-torque metal-geared servos**, one per leg.

The transformation subsystem is responsible for:

- moving each wheel/leg between UGV and UAV positions,
- maintaining repeatable geometry,
- keeping all BLDC thrust axes aligned after transformation,
- providing position feedback/state information to the high-level controller.

The STM32 is responsible for generating servo PWM and executing transformation commands received from ROS 2.

---

## 9. UAV Propulsion System

The aerial propulsion system consists of:

- **4 × 2812 900KV BLDC motors**
- **4-in-1 60 A ESC**
- **6S 5200 mAh battery**
- approximately **7.5-inch propellers** due to wheel internal-diameter constraints
- Pixhawk flight controller

A major validation step is the **static thrust test** of the actual motor-propeller-wheel configuration. The wheel acts as a surrounding structure around the propeller, so installed thrust must be measured rather than relying only on manufacturer thrust tables.

The target is to maintain sufficient total thrust margin for the expected approximately 3 kg final vehicle mass.


## 10. Embedded Control Architecture

The low-level controller is based on STM32.

Planned software modules include:

```text
motor.c / motor.h       -> DC motor PWM and direction control
encoder.c / encoder.h   -> quadrature encoder reading and RPM
servo.c / servo.h       -> transformation servo control
pid.c / pid.h           -> reusable PID controller
comms.c / comms.h       -> Pi <-> STM32 communication
sensors.c / sensors.h   -> low-level sensor interfaces
```

The STM32 receives high-level commands from the Raspberry Pi and executes time-critical motor and servo control independently.

Communication with ROS 2 will initially use **UART / USB serial**, keeping the low-level firmware simple and deterministic.

---

## 11. ROS 2 and Gazebo Simulation

A URDF model of Morphobot has been created and imported into **Gazebo**.

Current simulation work includes:

- robot links and joints,
- wheel actuation,
- UGV movement,
- Gazebo physics,
- ROS 2 topics,
- RViz visualization,
- preparation for sensor plugins,
- preparation for servo/transformation joints,
- preparation for UAV integration.

The simulation provides a safe environment to test control logic and autonomy before deploying code to the physical robot.

### ROS / Gazebo Images

> 

https://github.com/user-attachments/assets/f04c90f4-92b0-407d-9b01-9ad189500795





## 12. ROS 2 Ground Navigation Pipeline

The planned UGV autonomy stack is:

```text
LiDAR / Depth Camera / Encoders
             |
             v
       Sensor Processing
             |
             v
     Localization / SLAM
             |
             v
           Nav2
             |
          /cmd_vel
             |
             v
      ROS Hardware Node
             |
          Serial
             |
             v
           STM32
             |
      4 Ground Motors
```

Encoder data returns from STM32 to ROS 2 for wheel odometry and state estimation.

---

## 13. Mapping and SLAM

One of the main autonomy objectives is for Morphobot to **build maps of unknown environments autonomously**.

The mapping stack will combine available sensors such as:

- RPLIDAR,
- depth camera,
- wheel encoders,
- IMU,
- optional ToF sensors.

The robot will use SLAM for:

- map generation,
- localization,
- obstacle avoidance,
- navigation in previously unknown spaces,
- route planning.

---

## 14. GNSS-Denied Navigation

A major research direction is operation in environments where GPS/GNSS is unavailable, unreliable, or intentionally denied.

The intended localization architecture is based on combining:

- wheel odometry in UGV mode,
- IMU data,
- LiDAR odometry / SLAM,
- visual or visual-inertial odometry where applicable,
- Pixhawk state estimation for UAV operation.

The long-term objective is for the robot to continue its mission without depending entirely on GNSS.

---

## 15. UGV/UAV Mode-Aware Planning

The main research extension of Morphobot is **autonomous locomotion-mode selection**.

Instead of being manually told when to drive or fly, the robot should evaluate the environment and select the most suitable mode.

Example decision variables include:

- terrain traversability,
- obstacle height,
- ground-route distance,
- flight-route distance,
- estimated energy usage,
- battery state,
- transformation cost,
- mission time,
- localization confidence.

Example concept:

```text
Ground route available and efficient
            -> continue as UGV

Ground route blocked / inaccessible
            -> transform
            -> take off
            -> cross obstacle
            -> land
            -> return to UGV mode
```

This makes the transformation mechanism part of the robot's autonomy rather than only a mechanical feature.

---

## 16. Sim-to-Real Development Plan

The overall development sequence is:

1. Complete mechanical CAD.
2. Print and test one leg/wheel prototype.
3. Validate critical parts using ANSYS.
4. Complete full mechanical assembly.
5. Implement STM32 motor, encoder, servo, and communication firmware.
6. Validate ground movement manually.
7. Complete ROS 2 UGV simulation and Nav2 integration.
8. Connect ROS 2 to the physical STM32 controller.
9. Validate manual/stabilized UAV flight using Pixhawk.
10. Add autonomous aerial waypoint control.
11. Implement SLAM and GNSS-denied localization.
12. Implement transformation state machine.
13. Implement autonomous UGV/UAV mode-aware planning.
14. Perform complete sim-to-real mission testing.

---

## 17. Current Status

Current development includes:

- [x] Morphobot concept and mechanical architecture
- [x] Major SolidWorks assembly development
- [x] Wheel and leg geometry
- [x] Ground drivetrain ratio selection
- [x] Initial ANSYS analysis of structural components
- [x] URDF export
- [x] UGV movement in Gazebo
- [ ] Complete physical mechanical assembly
- [ ] STM32 ground-drive firmware
- [ ] Encoder PID control
- [ ] Servo transformation control
- [ ] ROS 2 <-> STM32 serial interface
- [ ] SLAM integration
- [ ] Nav2 autonomous ground navigation
- [ ] Pixhawk UAV integration
- [ ] Static thrust validation
- [ ] GNSS-denied navigation
- [ ] Autonomous transformation
- [ ] UGV/UAV mode-aware planner

---

## 18. Technologies Used

### Mechanical
- SolidWorks
- ANSYS Mechanical
- 3D printing

### Embedded
- STM32
- STM32CubeIDE / HAL
- Raspberry Pi 5
- Pixhawk
- UART / USB Serial
- PWM
- quadrature encoders

### Robotics
- ROS 2
- Gazebo
- RViz
- URDF
- Nav2
- SLAM

### Sensors / Autonomy
- RPLIDAR
- depth camera
- IMU
- wheel encoders
- GNSS / GPS
- ToF sensors

---

## 19. Future Work

Future extensions may include:

- semantic terrain understanding,
- traversability estimation,
- localization-confidence-aware autonomy,
- sensor-failure detection,
- dynamic obstacle handling,
- autonomous recovery behaviors,
- multi-robot collaborative mapping,
- heterogeneous multi-agent task allocation,
- onboard edge-AI perception.

---

## 20. Project Media

### Full Robot

> **[PLACE HERO IMAGE / RENDER HERE]**


## Author

**Muhammad Hassan Ashfaq**  
BS Mechatronics Engineering, NUST  
GitHub: [DexterxLab](https://github.com/DexterxLab)

---

## Disclaimer

Morphobot is an academic final-year engineering project under active development. Mechanical dimensions, electronics, software architecture, and subsystem choices may change as prototyping and testing continue.
