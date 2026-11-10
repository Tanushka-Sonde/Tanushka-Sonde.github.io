---
layout: page
title: "Invisible SVD-Based Digital Image Watermarking System"
description: "Invisible QR-code watermarking with SVD implemented from scratch, plus robustness benchmarks."
category: Computer Vision
importance: 4
img: assets/img/projects/placeholder.svg # optional: replace with a thumbnail in assets/img/projects/
date_range: "Sep 2026"
github: https://github.com/Tanushka-Sonde/Invisible-SVD-Based-Digital-Image-Watermarking
---

**Sep 2026** · Individual Project, IIT Gandhinagar, ES 666 Computer Vision Assignment 1

<a class="btn btn-sm z-depth-0" role="button" href="https://github.com/Tanushka-Sonde/Invisible-SVD-Based-Digital-Image-Watermarking"><i class="fa-brands fa-github"></i> GitHub</a>

## What I did

- Implemented **Singular Value Decomposition from scratch** using eigendecomposition of Gram matrices to embed a QR-coded watermark inside RGB cover images.
- Built an **invisible watermark recovery pipeline** and quantified the invisibility-recoverability tradeoff via PSNR sweeps across eleven distinct alpha strengths.
- Benchmarked **robustness under Gaussian noise and JPEG compression**, showing compression degraded watermark quality far more gently than pixel noise.
- Automated a **QR-decodability threshold search** across four noise levels and four compression qualities to find minimum reliable embedding strength.

**Tech:** SVD (from scratch), QR codes, PSNR, JPEG compression
