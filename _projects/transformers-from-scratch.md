---
layout: page
title: "Small Models and Attention: Transformers from Scratch"
description: "Label smoothing, causal self-attention and a minimal Vision Transformer, built from first principles."
category: Deep Learning & ML
importance: 2
img: assets/img/projects/placeholder.svg # optional: replace with a thumbnail in assets/img/projects/
date_range: "Sep 2026"
github: https://github.com/Tanushka-Sonde/Small_Models_and_Attention
---

**Sep 2026** · Individual Assignment, IIT Gandhinagar, Deep Learning Course, Prof. Nipun Batra

<a class="btn btn-sm z-depth-0" role="button" href="https://github.com/Tanushka-Sonde/Small_Models_and_Attention"><i class="fa-brands fa-github"></i> GitHub</a>

## What I did

- Trained **label-smoothed vs. hard-target MLP classifiers** on FashionMNIST, showing smoothing cut mean confidence from 0.81 to 0.71 with accuracy nearly unchanged across **10 epochs**.
- Built a **character-level next-letter predictor** with a learned embedding table, sampling names at **3 temperature settings** to analyze the diversity-coherence tradeoff.
- Implemented **causal self-attention from first principles**, verifying masked future positions carry exactly zero attention weight across **2 masking configurations**.
- Built a **minimal Vision Transformer** with patch embeddings and learned position tables, showing positional encoding caused an **82% accuracy drop** under patch shuffling versus 0% for the position-free variant.

**Tech:** FashionMNIST, MLP, causal self-attention, Vision Transformer
