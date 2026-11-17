---
layout: post
title: "Getting OpenVLA ready for a Franka FR3: notes on a sim-to-real pipeline"
date: 2026-11-17
description: Building MuJoCo environments, a differential-IK data collector and a LoRA-ready dataset for a 7B vision-language-action model.
tags: [robotics, vla, mujoco]
categories: notes
---

<!-- DRAFT. Every "TODO" marks a detail only you know. Fill or delete each one, then move this file to
     _posts/YYYY-MM-DD-openvla-franka-sim-to-real.md to publish. -->

In summer 2025 I worked with Prof. Harish Palanthandalam-Madapusi at IIT Gandhinagar on deploying **OpenVLA-7B**, a vision-language-action model, on a **Franka FR3** arm. The goal was natural-language-driven manipulation: type "put the red block on the blue one," and the robot does it from live camera input, first in simulation and then on the real arm.

We didn't finish fine-tuning or real-robot deployment; we ran out of compute and time. Most of the hard engineering happens before that point, though, so here is what building it taught me.

## What a VLA actually needs from you

OpenVLA maps (image, instruction) → a robot action (an end-effector delta plus gripper state). To adapt it to a new robot and new tasks, you need **demonstrations in that exact format**: synchronized camera frames, the language instruction, and the action taken at each step.

TODO: one or two lines on why OpenVLA in particular (open weights, LoRA support, the Open X-Embodiment pretraining).

## Step 1: MuJoCo environments that mirror the real cell

I built custom MuJoCo scenes for the target skills, **pick-and-place** and **object stacking**.

TODO:
- The FR3 model you used (the MuJoCo Menagerie model or your own URDF conversion?)
- Camera placement, and how you matched it to the real camera
- How objects and colours were randomized

## Step 2: a scripted expert using differential IK

Teleoperating hundreds of demos by hand is slow and inconsistent. Instead I wrote **differential inverse kinematics controllers** that drive the end effector through scripted waypoints (approach, grasp, lift, place). This gives a clean, repeatable "expert" in simulation.

TODO:
- The IK formulation (damped least squares? null-space posture term?)
- Gains, control rate, joint-limit handling
- One failure you hit (grasp slipping, singularities, …) and how you fixed it

## Step 3: logging data in the format the model expects

Each step logs the RGB frame, the instruction string, and the action, all **time-synchronized**.

TODO:
- Number of episodes and tasks, and episode length
- Format (RLDS / TFDS like Open X-Embodiment?)
- How you normalized actions to match OpenVLA's action tokenization

## Step 4: getting ready for LoRA fine-tuning

The dataset was prepared for **LoRA** fine-tuning, which adapts a 7B model by training small low-rank adapters instead of the full weights.

TODO: what was ready (configs, data loaders), and what compute it would have needed.

## Where it stopped, honestly

Fine-tuning and real-robot deployment weren't completed because of resource and timeline constraints. The pipeline (environments, expert, dataset) is ready for whoever picks it up.

TODO: what you'd do differently, e.g. collect some real teleoperated demos early, check the camera calibration first, or start from a smaller policy.

## What I took away

TODO: two or three honest lessons. For example, how much of "robot learning" is really data plumbing; sim-to-real gaps you expected but didn't get to measure.
