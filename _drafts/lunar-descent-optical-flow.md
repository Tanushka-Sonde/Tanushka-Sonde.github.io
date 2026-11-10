---
layout: post
title: "Reconstructing a lunar descent trajectory with optical flow"
tags: [computer-vision, optical-flow]
categories: [notes]
---

<!-- Outline only: write the text yourself. Points taken from the project's resume bullets. -->

- Vectorized Shi-Tomasi corner scoring using integral-image structure tensors over 588x432 Chang'e-5 descent frames, accelerating feature selection 45x versus per-pixel loops.
- Developed iterative Lucas-Kanade optical flow with bilinear warping, shrinking error on 2.4 px shifts from 1.67 px to 0.011 px against one-shot updates.
- Seeded KLT tracking with SIFT correspondences for large inter-frame motion, recovering 10 px and 33 px displacements where zero-initialized solvers stalled below 2.6 px.
- Reconstructed a metric 3D lander trajectory from 143 image pairs via robust similarity fits, resolving a 168 s flight, a 100 m hover and 25 timing gaps.
