---
layout: page
title: "Multi-Scale Template and Feature Matching for Scene Search"
description: "From-scratch filtering, multiscale NCC template matching, and Harris/DoG feature matching."
category: Computer Vision
importance: 3
date_range: "Sep – Oct 2026"
github: https://github.com/Tanushka-Sonde/Multi-Scale-Template-and-Feature-Matching-for-Scene-Search
---

**Sep – Oct 2026** · Individual Project, Computer Vision Assignment, Prof. Shanmuganathan Raman

<a class="btn btn-sm z-depth-0" role="button" href="https://github.com/Tanushka-Sonde/Multi-Scale-Template-and-Feature-Matching-for-Scene-Search"><i class="fa-brands fa-github"></i> GitHub</a>

## What I did

- Implemented **from-scratch filtering and denoising**, benchmarking box, Gaussian and median filters with PSNR and SSIM, lifting SSIM from 0.41 to 0.91.
- Built a **multiscale NCC template matcher** using integral images across 8 scales, reaching 1.000 IoU on the test scenes.
- Designed **Harris and Difference-of-Gaussian detectors** from structure tensors and an 8-level Gaussian scale ladder, building 128-dimensional gradient descriptors over a 4x4 cell grid that extracted up to 4,000 keypoints per scene.
- Engineered a **rotation invariant voting pipeline**, recovering a 31 degree rotated target from 16 of 827 matches.

**Tech:** NCC template matching, Harris and DoG detectors, PSNR, SSIM
