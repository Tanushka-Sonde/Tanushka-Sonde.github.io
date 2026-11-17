---
layout: post
title: "Teleoperating a robot arm with an ArUco marker and a pinch gesture"
date: 2026-11-17 07:00:00
description: RealSense + ArUco for position, MediaPipe for the gripper, Mink IK in MuJoCo, and why the real arm jittered when the simulation didn't.
tags: [robotics, computer-vision, ros]
categories: notes
toc:
  sidebar: left
---

My first robotics project at IIT Gandhinagar ran from January to April 2025. I worked on it with Afraz, advised by Prof. Harish Palanthandalam-Madapusi, with guidance from our TAs Isharath and Debojit Das.

The goal: make an **Addverb Heal** arm copy the motion of a marker in your hand, and open or close its gripper when you pinch your fingers. By the end, it could pick up a cube and drop it in a basket under teleoperation.

{% include figure.liquid path="assets/img/blog/teleop/pick-and-place.png" class="img-fluid rounded z-depth-1" zoomable=true caption="The Heal arm picking up a cube and placing it in the basket, under vision-based teleoperation with gesture-controlled gripping." %}

{% include video.liquid path="https://www.youtube.com/embed/wl9rbc5eun8" class="img-fluid rounded z-depth-1" %}

## How the project progressed

- **January:** learned ROS and read teleoperation papers.
- **February:** teleoperated the Heal arm from the keyboard.
- **March:** built the computer-vision and ArUco framework, plus a MuJoCo and Mink IK framework.
- **April:** integrated everything into the final version and worked on the real hardware.

The keyboard stage turned out to be more useful than it sounds. Joint mode uses 12 keys (each joint, in either direction). Cartesian mode moves along ±X/Y/Z and rotates in roll, pitch and yaw through the IK solver. It let us test the controller and calibrate the arm before trusting a camera with it.

## The pipeline

{% include figure.liquid path="assets/img/blog/teleop/pipeline.png" class="img-fluid rounded z-depth-1" zoomable=true caption="From the camera to the arm." %}

1. **Perception.** An Intel RealSense RGB-D camera tracks a printed 6×6 **ArUco marker** and publishes its position on the ROS topic `/aruco_position`. Pressing **C** sets the marker's current position as the origin, so after that every motion is relative to where your hand started.
2. **The simulator in the middle.** The marker position moves a target inside a **MuJoCo** simulation of the arm. The simulated arm follows it using **Mink**, a real-time IK solver that optimizes task-space and posture objectives together.
3. **The real arm mirrors the simulation.** A second node reads the simulated arm's state and sends **joint velocity commands** to the real robot's controller over ROS.
4. **The gripper.** **MediaPipe** hand tracking watches the thumb and index finger. Pinching them together closes the gripper, and spreading them opens it.

Putting the simulation in the middle means the IK and the constraints live in software we control, and we can test everything without risking the hardware.

## Results: smooth in simulation, jittery on the robot

In **simulation**, the arm tracked the marker smoothly and accurately. That confirmed the perception and ROS publishing were reliable and low-noise.

The **real robot** showed noticeable jitter under marker control, even though the simulation stayed smooth.

{% include figure.liquid path="assets/img/blog/teleop/joint-angles.png" class="img-fluid rounded z-depth-1" zoomable=true caption="Joint angles: MuJoCo simulation (blue) vs the real robot (orange) over one teleoperation run." %}

We narrowed down where the jitter came from:

- **The hardware can move smoothly.** There was no jitter when we commanded the robot directly, or when we dragged the simulation with the mouse.
- **The software is fine.** The simulation stays smooth, so the ROS pipeline itself works as intended.
- **The likely main cause is a rate mismatch.** The camera runs at **30 Hz** while the robot's control loop runs at **500 Hz**, so the controller sees the target jump in discrete steps instead of moving continuously.
- **A likely contributor is the communication path.** Delays or limits in the hardware interface can amplify those irregularities.

## What I'd do next

- **Predictive filtering** of the marker pose, to interpolate smoothly between 30 Hz camera updates.
- **IMU-based tracking**, which runs far faster than a camera.
- **Better communication and hardware response** between the PC and the robot controller.

## What I learned

The most useful habit was isolating each stage. We could only blame the rate mismatch, and not our code, because each part had been checked on its own: keyboard control on the real hardware, the vision pipeline in simulation, and mouse control in simulation. Real hardware punishes assumptions, and a simulator in the loop makes them much cheaper to test.

📄 [Poster (PDF)](/assets/pdf/posters/teleoperation-poster.pdf) · 💻 [Code on GitHub](https://github.com/IITGN-Robotics/Addverb-Heal-Teleop) · 🗂️ [Project page](/projects/arm-teleoperation-cv/)
