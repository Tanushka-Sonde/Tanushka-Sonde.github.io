---
layout: post
title: "What I learned reproducing a physics-based OCT classifier"
tags: [medical-imaging, reproduction]
categories: [notes]
---

<!-- Outline only: write the text yourself. Points taken from the project's resume bullets. -->

- Reproduced a deterministic physics-based OCT classification pipeline from scratch on a 517-scan benchmark, matching the original paper's published results across all four diagnostic classes.
- Extended the pipeline with anatomically-informed band and sector features built from retinal layer segmentation, evaluated across 3 independent OCT datasets collected on different scanners.
- Diagnosed dataset confounds behind the original paper's claims through systematic cross-scanner validation and permutation-based feature importance audits on held-out patients.
- Benchmarked the interpretable pipeline against a fine-tuned CNN baseline to quantify the real accuracy-interpretability tradeoff between physics-based and deep features.
