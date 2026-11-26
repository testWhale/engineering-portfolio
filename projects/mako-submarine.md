# MAKO Submarine Engineering

**Constrained project · non-game project** · MAKO human-powered submarine team, 2025–2026 · real-time telemetry, computer vision

## Overview

MAKO is Singapore's and Southeast Asia's first human-powered submarine. I was the team's lead developer (submarine) and worked on its mechanical and electrical design. My software contributions were the pilot-facing telemetry dashboard and a computer-vision pipeline for finding underwater gates.

## Demonstration

![Bench prototype: Arduino Uno wired on a breadboard to GY-68 and GY-511 sensor breakout boards, driving a 16x2 LCD that reads "Altitude: 55.87"](../media/mako-sensor-bench-prototype.jpg)

*An early bench prototype of the sensor-to-display path: an Arduino Uno reads two breakout boards on a breadboard, labelled GY-68 and GY-511 (commonly a BMP180 barometric pressure sensor and an LSM303 accelerometer/magnetometer), and shows a live altitude reading on a 16x2 LCD. The sensor set used in the submarine (MPU-9250, MS5837-30BA, RPM sensor) is listed under [My role and contributions](#my-role-and-contributions).*

## My role and contributions

**My work**

- Built a real-time telemetry GUI for inside the submarine. It translates raw sensor data from a 9-axis IMU (MPU-9250), a depth and pressure sensor (MS5837-30BA) and an RPM sensor into information the pilot can use.
- Built the dashboard software: a Flask server and a Three.js 3D view that shows the submarine's yaw, pitch and roll, with separate IMU and depth panels.
- Adapted an existing open-source YOLO11 + OpenCV object-detection pipeline to detect lines, so the submarine could identify and navigate through underwater gates, and ran practical tests with it.

**Team results** (shared with the whole MAKO team)

- 3rd place at **eISR 2026** in Best Run, Best Speed and Best Time.
- A **33% improvement over 2025**.

## Problem

The pilot of a human-powered submarine is underwater and pedalling, with very limited visibility and no outside references. They need to know the vessel's orientation, depth and propulsion speed at a glance, and the course is defined by gates that have to be found and passed through.

## Constraint

- **Physical environment:** the pilot is underwater and fully occupied with propulsion, so information has to be readable instantly.
- **Hardware integration:** data comes from three separate sensors (MPU-9250 IMU, MS5837-30BA depth/pressure, RPM) that have to be turned into usable pilot information.
- **Vision under water:** a general-purpose object detector is not trained to recognise pool lines or race gates.
- **Competition deadline:** the work had to be ready for eISR 2026.

## What I Tested

- The telemetry GUI with the MAKO sensor hardware (MPU-9250, MS5837-30BA, RPM sensor).
- Locally, the published dashboard code: the Flask server serves the page, the JavaScript and the 3D models, and the CSV replay utility converts logged time/speed data to JSON.
- In practical sessions, the OpenCV-based detection pipeline for identifying and navigating through underwater gates.

## What Failed

- **The off-the-shelf detector could not find gates.** The starting pipeline ([yuvarajpanditrathod/Real-Time-Object-Detection-using-YOLO11](https://github.com/yuvarajpanditrathod/Real-Time-Object-Detection-using-YOLO11)) uses pretrained YOLO11 models for general object detection from webcam, image and video. Those models have no class for pool lines or gates.
- **The published dashboard version is not wired to live sensors.** In the code in the public repository, orientation and depth come from on-screen controls. A placeholder car model stands in for the submarine because the submarine model is in USDZ format, which the Three.js GLTFLoader cannot load.

## What I Changed

- Extended the detection pipeline to detect **lines**, so the submarine could identify and navigate through gates.
- Built the dashboard around separate IMU and depth panels, and a 3D model whose rotation follows yaw, pitch and roll (applied in YXZ order).
- Documented the dashboard's remaining gaps openly in its README: no live sensor link, a placeholder model, a missing depth readout and no RPM panel.

## What Happened

- The team achieved a **33% improvement over 2025** and placed **3rd at eISR 2026** in Best Run, Best Speed and Best Time.
- The dashboard is public with verified setup instructions: [testWhale/SubmarineDashboard](https://github.com/testWhale/SubmarineDashboard).
- The detection pipeline is a credited public fork: [testWhale/Real-Time-Object-Detection-using-YOLO11](https://github.com/testWhale/Real-Time-Object-Detection-using-YOLO11). My line/gate changes are not yet committed there, and no detection-accuracy or speed figures are claimed.

## Technologies

Python, Flask, JavaScript, Three.js, lil-gui, glTF, Ultralytics YOLO11, OpenCV, PyTorch, MPU-9250, MS5837-30BA, RPM sensor

## Supporting evidence

- Photo of the sensor bench prototype (see [Demonstration](#demonstration))
- [SubmarineDashboard repository](https://github.com/testWhale/SubmarineDashboard): source, architecture, setup verification and known limitations
- [YOLO11 fork with MAKO notice](https://github.com/testWhale/Real-Time-Object-Detection-using-YOLO11): credited upstream project and a description of the MAKO adaptation

## Source availability

| Component | Availability |
|---|---|
| Telemetry dashboard | Public, [testWhale/SubmarineDashboard](https://github.com/testWhale/SubmarineDashboard) |
| Gate-detection pipeline | Public fork of a third-party project; my line-detection changes are not yet published |
| Sensor ingestion / firmware | Not published |

[← Back to portfolio](../README.md)
