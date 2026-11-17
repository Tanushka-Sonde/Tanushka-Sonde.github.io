---
layout: post
title: "Teleoperating a robot arm with an ArUco marker and a hand gesture"
date: 2026-11-17
description: RealSense + ArUco for position, MediaPipe for the gripper, Mink IK in MuJoCo, and a real Addverb Heal arm that follows along.
tags: [robotics, computer-vision, ros]
categories: notes
---

<!-- DRAFT. Fill the TODOs, add your GIFs/images under assets/img/blog/teleop/, then move this file to
     _posts/YYYY-MM-DD-teleoperating-a-robot-arm-with-a-marker.md to publish. -->

My first robotics project at IIT Gandhinagar (January to April 2025, with Prof. Harish Palanthandalam-Madapusi) was making an **Addverb Heal** arm copy the motion of a marker in my hand, and open or close its gripper when I pinched my fingers. Here's how the pieces fit together.

{% include video.liquid path="https://www.youtube.com/embed/wl9rbc5eun8" class="img-fluid rounded z-depth-1" %}

## The idea: let the simulator be the middleman

The simplest design would send camera positions straight to the robot. We put a **MuJoCo** simulation in between instead:

1. A camera tracks a target, and the target moves inside the simulation.
2. The simulated arm follows the target using inverse kinematics.
3. The real arm mirrors the simulated arm through joint velocity commands.

The simulator is where the IK and the constraints live, so bad inputs get filtered before they reach real hardware.

TODO: add the flowchart (`media/flow chart.png` in the repo) as an image here.

## Perception: a RealSense and a 6×6 ArUco marker

An **Intel RealSense** camera tracks a printed **6×6 ArUco marker** and publishes its position on `/aruco_position`. Pressing **C** sets the marker's current position as the origin, so after that every motion is relative. You move your hand from wherever is comfortable, and the robot moves by the same amount.

TODO: camera resolution and FPS; how you mapped camera axes to robot axes; any scaling or filtering of the marker position.

## Control: Mink IK, then joint velocities over ROS

The simulated arm solves **constraint-aware inverse kinematics** with [Mink](https://github.com/kevinzakka/mink), which optimizes task-space and posture objectives together in real time. A second script reads the simulated arm's state and publishes **joint velocity commands** to the real robot's controller over **ROS (Noetic)**, with the controller set to velocity mode.

TODO: control rate, and how you turned the sim's state into velocity commands (gains? simple P control on the joint error?).

## The gripper: a pinch gesture with MediaPipe

**MediaPipe** hand tracking watches the thumb and index finger. Bringing them together closes the gripper, and spreading them opens it.

TODO: the threshold you used, and how you avoided flicker (hysteresis? debouncing?).

## A keyboard fallback

The repo also has a **keyboard teleop** package: joint-space control with 12 keys (each joint ± direction) and Cartesian control (±X/Y/Z, roll/pitch/yaw) through the same IK. It was invaluable for testing and calibration before trusting the vision pipeline.

## What went wrong (the useful part)

- **Sudden motions trip the safety stop.** Jerky marker movement triggers the arm's safety constraints, and you have to reset the base controller. Moving slowly and smoothly is part of the procedure.
- **Origin-setting can take a few tries.** The README says to press C more than once until the robot responds.
- TODO: anything else, e.g. latency, lighting affecting ArUco detection, or IK near singularities.

## What I'd do next

TODO: for example, filter the marker pose (Kalman or one-euro filter), add orientation tracking, or use force feedback.

Code: [IITGN-Robotics/Addverb-Heal-Teleop](https://github.com/IITGN-Robotics/Addverb-Heal-Teleop).
