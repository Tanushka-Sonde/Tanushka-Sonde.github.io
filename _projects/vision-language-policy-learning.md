---
layout: page
title: "Vision-Language Policy Learning for Robotic Manipulation"
description: "Sim-to-real pipeline and dataset for OpenVLA-7B on a Franka FR3 (fine-tuning not completed)."
category: Robotics
importance: 2
date_range: "Jun – Jul 2025"
---

**Jun – Jul 2025** · Prof. Harish Palanthandalam-Madapusi, IIT Gandhinagar

## What I did

- Aimed to deploy ** OpenVLA-7B vision-language-action model** on the Franka FR3 robot to enable natural language prompt-driven task execution using real-time visual feedback, both in simulation and on the real robot.
- Designed and validated a **simulation-to-real pipeline**, building custom MuJoCo-based environments to test prompt interpretation and manipulation skills such as pick-and-place and object stacking.
- Developed **custom-designed differential inverse kinematics controllers** to create a MuJoCo-based pick-and-place proxy, used for high-quality data collection with synchronized image, language, and action logging; the resulting dataset was prepared for **LoRA-based fine-tuning**.
- While fine-tuning and real-robot deployment were not completed due to resource and timeline constraints, the system architecture and dataset are fully prepared for future integration and experimentation.

**Tech:** OpenVLA-7B, Franka FR3, MuJoCo, LoRA
