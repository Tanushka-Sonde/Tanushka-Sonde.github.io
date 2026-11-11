---
layout: page
title: "Smart Labeling Pipeline for Scalable Data Annotation"
description: "Labeling pipeline combining human annotation, Snorkel weak supervision, active learning and LLMs."
category: Data Engineering
importance: 1
date_range: "Sep 2025"
---

**Sep 2025** · Software Tools and Techniques for AI Assignment, Prof. Nipun Batra

<a class="btn btn-sm z-depth-0" role="button" href="https://colab.research.google.com/drive/1p_m4IEGk4RipVAuHvS4MfAQpMojEDIYV?usp=sharing"><i class="fa-solid fa-book-open"></i> Colab notebook</a>

## What I did

- Built an end-to-end **data labeling pipeline** combining human annotation, weak supervision, active learning, and LLM-based labeling to reduce manual annotation effort.
- Established a **gold-standard dataset** from 3 annotators and implemented **Fleiss' Kappa from scratch** to measure inter-annotator agreement (κ ≈ 0.85) with automated conflict resolution.
- Applied **Snorkel weak supervision** with heuristic labeling functions to generate programmatic labels for 200 reviews and analyzed coverage and conflict statistics.
- Implemented **active learning strategies** (least-confidence and entropy sampling) with TF-IDF + logistic regression, demonstrating improved labeling efficiency compared to random sampling.
- Integrated **Gemini LLM-based bulk labeling** with few-shot prompting and built a noise-detection module to identify high-confidence label disagreements.

**Tech:** Snorkel, TF-IDF, logistic regression, Gemini
