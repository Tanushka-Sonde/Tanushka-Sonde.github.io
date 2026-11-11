---
layout: page
title: "Multi-Parametric Retinal Diagnostics: Reproduction and Extension"
description: "Reproduced a physics-based OCT classifier and stress-tested it with cross-scanner validation on 3 datasets."
category: Research
importance: 2
date_range: "Aug – Nov 2026"
github: https://github.com/Tanushka-Sonde/Multi-Parametric-Retinal-Diagnostics
---

**Aug – Nov 2026** · Project Course, IIT Gandhinagar, Prof. Prabhat Munshi

<a class="btn btn-sm z-depth-0" role="button" href="https://github.com/Tanushka-Sonde/Multi-Parametric-Retinal-Diagnostics"><i class="fa-brands fa-github"></i> GitHub</a>

## What I did

- Reproduced a **deterministic physics-based OCT classification pipeline** from scratch on a 517-scan benchmark, matching the original paper's published results across all four diagnostic classes.
- Extended the pipeline with **anatomically-informed band and sector features** built from retinal layer segmentation, evaluated across 3 independent OCT datasets collected on different scanners.
- Diagnosed dataset confounds behind the original paper's claims through systematic **cross-scanner validation** and permutation-based feature importance audits on held-out patients.
- Benchmarked the interpretable pipeline against a **fine-tuned CNN baseline** to quantify the real accuracy-interpretability tradeoff between physics-based and deep features.

**Tech:** OCT, retinal layer segmentation, permutation feature importance, CNN baseline
