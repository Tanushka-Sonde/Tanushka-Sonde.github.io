---
layout: page
title: "SketchWorld: Camera-Controlled Scene Generation from Sketches"
description: "Camera-controlled next-view forecasting with SCEdit tuners on a frozen Stable Diffusion 1.5."
category: Research
importance: 1
img: assets/img/projects/placeholder.svg # optional: replace with a thumbnail in assets/img/projects/
date_range: "Aug 2026 – Present"
---

**Aug 2026 – Present** · Ongoing Research Project, Computer Vision Lab, IIT Gandhinagar, Prof. Shanmuganathan Raman

## What I did

- Conducted a **literature-grounded benchmark** of six controllable diffusion methods on COCO, including ControlNet, T2I-Adapter and SCEdit, to inform the project's design.
- Implemented **Universal Few-shot Control (UFC)** on ControlNet 1.1, reaching 0.905 edge F1 from just 30 examples of an unseen condition type.
- Engineered a **camera-controlled next-view forecasting pipeline** predicting frames 3.3 seconds ahead using per-pixel motion maps and SCEdit tuners on a frozen Stable Diffusion 1.5.
- Diagnosed model behavior through **systematic input-ablation testing** across 1,000 scenes, isolating why three trained variants trail a geometric warp baseline.

**Tech:** ControlNet, T2I-Adapter, SCEdit, Universal Few-shot Control (UFC), Stable Diffusion 1.5, COCO
