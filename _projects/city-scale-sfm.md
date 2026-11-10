---
layout: page
title: "City-Scale 3D Reconstruction: Structure from Motion from Tourist Photos"
description: "City-scale Structure-from-Motion from tourist photos, with 9 core algorithms implemented from scratch."
category: Computer Vision
importance: 2
img: assets/img/projects/placeholder.svg # optional: replace with a thumbnail in assets/img/projects/
date_range: "Nov 2026"
github: https://github.com/Tanushka-Sonde/Photos-to-3d-sfm
---

**Nov 2026** · Course Project, ES666, IIT Gandhinagar

<a class="btn btn-sm z-depth-0" role="button" href="https://github.com/Tanushka-Sonde/Photos-to-3d-sfm"><i class="fa-brands fa-github"></i> GitHub</a>

## What I did

- Built a **city-scale Structure-from-Motion pipeline** turning **unlabelled tourist photos** into 3D landmark models, implementing **9 core algorithms from scratch** in NumPy/SciPy, passing **89/89 tests** against OpenCV and scikit-learn.
- Cut pair verification from **40.4M to 77.8K candidates (0.19%)** using **tf-idf visual-word retrieval** and query expansion, separating 9 landmarks from **2,000 distractor images**.
- Reconstructed the **Trevi Fountain** with **507/528 cameras** placed via skeletal-set SfM, PnP and **bundle adjustment** at **0.76 px reprojection error**, plus an **824K-point dense cloud**.
- Merged two independent St Peter's maps using **Umeyama similarity alignment** (3.2% error) and rebuilt a **self-captured scene**, raising cameras placed from **8 to 55 of 58** by diagnosing missing EXIF focal lengths.

**Tech:** NumPy, SciPy, OpenCV, scikit-learn
