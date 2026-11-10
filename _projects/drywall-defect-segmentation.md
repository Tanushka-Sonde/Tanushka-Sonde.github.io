---
layout: page
title: "Prompted Drywall Defect Segmentation using CLIP-Conditioned Models"
description: "Text-prompted crack and taping-area segmentation with a from-scratch CLIP-UNet using FiLM conditioning."
category: Computer Vision
importance: 5
img: assets/img/projects/placeholder.svg # optional: replace with a thumbnail in assets/img/projects/
date_range: "Apr 2026"
github: https://github.com/Tanushka-Sonde/Prompted_Segmentation_for_Drywall_QA
---

**Apr 2026** · Individual Project

<a class="btn btn-sm z-depth-0" role="button" href="https://github.com/Tanushka-Sonde/Prompted_Segmentation_for_Drywall_QA"><i class="fa-brands fa-github"></i> GitHub</a>

## What I did

- Designed a **text-prompted binary segmentation system** distinguishing **crack** and **taping-area** defects from a single natural-language query, trained on **960 COCO-annotated** drywall images.
- Built **CLIP-UNet with FiLM conditioning** from scratch, projecting CLIP text embeddings into per-channel **gamma-beta modulation** of a ResNet-encoder U-Net decoder for prompt-guided mask prediction.
- Fine-tuned the decoder of a pretrained **CLIPSeg (rd64-refined)** checkpoint under frozen encoders, comparing against the scratch model on **mIoU, Dice, Precision, and Recall**.
- Achieved **0.503 mIoU** and **0.645 Dice** with the scratch model, outperforming fine-tuned CLIPSeg by **5 mIoU points** while using **183M** parameters at **22.5ms** inference latency.

**Tech:** CLIP, U-Net, FiLM, ResNet, CLIPSeg, COCO annotations
