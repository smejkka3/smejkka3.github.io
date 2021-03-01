---
layout: post
title:  "F1Tenth Project - Object Tracking"
excerpt: "Chose a project to work on in F1Tenth which is a object tracking on the F1Tenth autonomous race-
car system and implement and test the proposed system on the F1Tenth vehicle, making use of its onboard 2D
LiDAR."
---

The project I'm going to be working on is object tracking using LiDAR initially. The algorithm is based on the paper [Wang et. al](https://www.robots.ox.ac.uk/~mobile/Papers/2015IJRR_wang.pdf) where the main goal is to implement solution for detection and tracking of moving object using 2D laser scanner.


## Description of tasks
Robust object tracking and localization is a critical need for autonomous cars, allowing for safe,
intelligent navigation and planning. Cars must be able to perform this task rapidly and accurately,
requiring algorithms with high output frequencies and low false-negative and false positive rates.
While much work has been done on object tracking and localization with full-scale cars, the

F1Tenth Object Tracking project seeks to implement tracking on the F1Tenth autonomous race-
car system. Initially, this project will build on the work of Dominic Zeng Wang, who proposed a
LiDAR-based object tracking system that uses the knowledge of the static local background to help identifying dynamic
objects from static ones in a principled and straightforward way. This project will
implement and test her proposed system on the F1Tenth vehicle, making use of its onboard 2D
LiDAR. Later, I will combine LiDAR-based tracking with visual tracking, in order to increase
the robustness of the system by increasing the signal-noise ratio from the vehicle’s sensors.
Given the restrictions posed by the pandemic, this project will heavily leverage virtual
environments like RViz and the F1Tenth team’s proprietary gym environment. If possible, I will
attempt to implement my system on the actual F1Tenth car for further real-life testing and
validation.

## Step by Step to Dos
### Phase 1:
Get Familiar with F1Tenth -- January 25 - February 3
- Learn about the F1Tenth car, the F1Tenth System, and the F1Tenth Simulator.
- Learn about the ROS framework.
- Learn about F1Tenth fundamentals with the labs linked here.

### Phase 2: Get Familiar with Fundamentals -- February 3 – February 14
- Carefully study Wang paper, along with related works.
- Learn how to use F1Tenth Gym environment.
- Get familiar with fundamentals of Bayes filtering.
- Write progress report / documentation as needed.

### Phase 3: Implementation -- February 14 – March 14
- Implement virtual sensing system specified in Wang paper on F1Tenth.
- Implement association algorithms for LiDAR data, divide it into meaningful pieces, and associate those pieces with tracked vehicles on F1Tenth.
- Identify areas where visual data can increase robustness of LiDAR system.
- Write progress report / documentation as needed.

### Phase 4: Expansion -- March 14 – April 11
- Develop pipeline to validate LiDAR tracking hypothesis with camera data, and camera
hypothesis with LiDAR data
- Write progress report / documentation as needed.

### Phase 3: Wrapping up Phase – April 11 – May 10
- Continue working on the previous goals that have not been accomplished.
- Documentation for future development (user guide / issues / achievements).
- Optional: Demonstrate to potential business partners / investors.
