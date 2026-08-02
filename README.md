# Quadruped-Robot-14DOF
An advanced 14-DOF quadruped robotic dog tailored for industrial applications and dynamic locomotion.

# 🐕 Industrial Quadruped Robot (12-14 DOF)

This repository contains the Research & Development (R&D) documentation for a highly dynamic, industrial-grade Quadruped Robot, developed for **Smart Methods**. Engineered for rigorous tasks including industrial inspection, exploration, and carrying payloads in unstructured terrains.

---

## 🎥 Mechanical Exploded View

https://github.com/user-attachments/assets/7962cb64-da32-4da7-a119-5c3f42d29c4a


* **3D CAD Workspace:** [Click here to view the Onshape 3D model](https://cad.onshape.com/documents/303f27d1ecfc229879eb5742/w/d1e5f713d53e054cbf8c910c/e/23fefe92cce89c0083036dff)

---

## 📑 Table of Contents
1. [Project Overview & Industrial Vision](#1-project-overview--industrial-vision)
2. [Mechanical Design & Material Science](#2-mechanical-design--material-science)
3. [Electronics & Actuators](#3-electronics--actuators)
4. [Sensors & Perception](#4-sensors--perception)
5. [Software Architecture & Control Algorithm](#5-software-architecture--control-algorithm-flowchart)
6. [Manufacturing & Sourcing Guide](#6-manufacturing--sourcing-guide-bom)

---

## 1. Project Overview & Industrial Vision
Moving beyond hobbyist designs, this platform is built to enterprise standards.

### High-End Technical Specifications:
* **Overall Dimensions:** 50 - 70 cm in length (Medium-scale Quadruped), striking the perfect balance between agility in tight spaces and physical robustness.
* **Total Weight:** < 14 kg (Including battery and sensors). Achieving this required advanced material selection to ensure a high Torque-to-Weight ratio.
* **Degrees of Freedom (DOF):** 12 to 14 DOF.
  * **12 Core DOF:** 3 per leg (Abduction/Adduction, Hip, and Knee) for omnidirectional locomotion.
  * **2 Optional DOF:** A 2-axis upper gimbal for mounting and stabilizing RGB-D cameras independent of the chassis pitch/roll.
* **Payload Capacity:** Engineered to carry an additional 3 - 5 kg reliably.

---

## 2. Mechanical Design & Material Science
The core mechanical challenge in legged robotics is minimizing leg inertia while maximizing structural integrity to withstand jumping and impact forces.

### Chassis & Load Distribution
* **Centralized Center of Mass (CoM):** The main trunk houses all heavy electronics and batteries.
* **Low Swing Inertia:** The knee motors are relocated to the upper hip area, utilizing a linkage mechanism to actuate the lower leg. This drastically reduces the moving mass of the distal limbs, enabling high-frequency leg swings with minimal energy expenditure.

### Material Selection
1. **Aviation-Grade Aluminum (6061-T6 / 7075):** Used exclusively for structural joints, motor brackets, and load-bearing pivots. It provides extreme rigidity and acts as a passive heatsink for the BLDC motors.
2. **Carbon Fiber Tubes/Plates:** Utilized for the lower leg segments (Tibia/Calf) to provide higher stiffness than steel at a fraction of the weight, absorbing ground impact efficiently.
3. **Industrial 3D Printing:** * **PA-CF (Nylon infused with Carbon Fiber):** Used for internal structural mounts. Withstands up to 150°C and offers extreme tensile strength.
   * **PC (Polycarbonate):** Used for outer shells and electronics covers due to its unmatched impact resistance.
   * **TPU (Thermoplastic Polyurethane):** Used for foot end-effectors to act as mechanical shock absorbers and provide high-friction grip.

### Joints, Hardware, & Fasteners
* **Quasi-Direct Drive (QDD):** Joints are driven by motors with integrated planetary gearboxes (1:6 to 1:10 ratio). This provides immense torque while maintaining "back-drivability" to protect the gearbox from sudden ground impacts.
* **Bearings:** Deep groove ball bearings isolate radial and axial loads from the motor shaft, ensuring frictionless motion.
* **Fasteners:** Class 10.9 Alloy Steel Hex Socket screws combined with **Heat-Set Threaded Inserts** melted into the 3D-printed parts, allowing infinite assembly cycles without stripping the plastic.

---

## 3. Electronics & Actuators
* **Actuators (BLDC):** 12 Quasi-Direct Drive Brushless DC Motors. Each features an integrated Field Oriented Control (FOC) driver and a high-resolution magnetic encoder for instantaneous proprioceptive feedback.
* **Dual-Controller Architecture:**
  * **Low-Level Microcontroller (Teensy 4.1 / STM32):** Programmed in bare-metal **C++** for ultra-low latency. Handles Inverse Kinematics (IK), reads IMU data, and controls motor torque at 500Hz+.
  * **High-Level Companion Computer (NVIDIA Jetson Orin Nano / RPi 5):** Runs **ROS 2** (Robot Operating System). Handles 3D vision, VSLAM mapping, and AI-based trajectory planning.
* **Industrial Communication:** Discarded I2C/PWM in favor of **CAN Bus** (Differential Signaling). Ensures 1Mbps data transfer across all 12 motors with total immunity to Electromagnetic Interference (EMI) caused by high current draws.
* **Power System:** 6S LiPo Battery (22.2V, 6000mAh, 60C discharge) connected via XT90 connectors and 12/14 AWG silicone wires. An isolated Power Distribution Board (PDB) with industrial buck converters safely steps down voltage for logic components.

---

## 4. Sensors & Perception
* **IMU (Inertial Measurement Unit):** A 9-axis industrial IMU (e.g., BNO085) placed exactly at the robot's CoM to feed high-frequency pitch, roll, and yaw data to the balance algorithm.
* **Vision System:** RGB-D camera (e.g., Intel RealSense D435i) mounted at the front for depth perception, obstacle avoidance, and terrain mapping.
* **Contact Sensors:** Force sensors at the foot pads to precisely detect the touchdown phase, essential for dynamic load distribution.

---

## 5. Software Architecture & Control Algorithm (Flowchart)
The robot's balance relies on a strict, real-time control loop executed continuously:

1. **Initialization:** System boot, CAN bus node verification, zero-position calibration for all encoders, and IMU zeroing.
2. **State Estimation:** Fusing IMU orientation data with motor encoder angles to calculate the exact spatial position of the CoM.
3. **Trajectory Planning:** Generating the gait pattern (Trot, Pace, or Bound) and calculating the spatial arch for the swing leg to reach the next foothold.
4. **Inverse Kinematics (IK):** Translating the desired Cartesian coordinates $(X, Y, Z)$ into specific joint angles $(\theta_1, \theta_2, \theta_3)$ for the hip and knee motors.
5. **Torque Control (PID):** Sending target angles and utilizing PID controllers to adjust motor torque, applying gravity compensation to keep the leg on its planned trajectory.
6. **Feedback Loop:** Comparing the actual state vs. the target state and adjusting instantly before the next millisecond cycle.

---

## 6. Manufacturing & Sourcing Guide (BOM)
* **Motors:** Sourced from specialized robotics manufacturers (e.g., T-Motor AK series or MyActuator RMD series).
* **Custom Machining & PCB:** Aluminum parts (STEP files) and custom Power Distribution Boards (Gerber files) are outsourced to high-precision manufacturers like **PCBWay** or **Xometry**.
* **Electronics:** Microchips, buck converters, and authentic AWG wiring sourced strictly from authorized distributors (Mouser / DigiKey) to avoid counterfeit failures under heavy load.
