---
layout: post
title: "The Scanner Was the Diagnosis: Physics and Fractals in Retinal OCT"
date: 2026-11-17 07:30:00
description: Reproducing a physics-based OCT classifier, finding the confound behind its perfect triage, and building explainable features that hold up across scanners.
tags: [medical-imaging, computer-vision, research]
categories: notes
toc:
  sidebar: left
---

This is the story behind our [project course poster](/assets/pdf/posters/retinal-diagnostics-poster.pdf) at IIT Gandhinagar. I worked with Tejaswini Rathod and Shubhabrata Sarkar, supervised by Prof. Prabhat Munshi.

The course was on non-destructive testing (NDT). The question was whether the physics used to inspect materials can also diagnose retinas, and do it in a way a clinician can read.

## The claim we started from

**OCT** (optical coherence tomography) is the standard retinal scan: a cross-section of the retina built from reflected light. CNNs classify OCT scans very accurately, but they are opaque. They can also learn the **scanner's signature** instead of the disease.

A published physics-based engine took a different route:

- Split each scan into a 15 × 10 grid.
- In every cell, measure two texture quantities: the **KT-1 slope** (how texture peaks fade under blur) and the **Hurst coefficient** (roughness across scales).
- Classify with a random forest.

It reported **100% healthy-eye triage** on the OCTID dataset. Our goal was to test that claim, then build physics features that are explainable *and* hold up across datasets and scanners.

## Step 1: reproduce it, then look at the data

I reproduced the engine from scratch and matched the paper:

- **NORMAL** precision and recall: **1.00 / 1.00**
- Overall accuracy: **68.3%** (paper: 71%)
- The same top-15 features (all Hurst), in the same grid rows

Then I looked at the data itself. In OCTID, **healthy and diseased scans come from different scanners**. Their image sizes don't even overlap, and every healthy file is a `.jpg` while every diseased file is a `.jpeg`. **Image size alone separates healthy from diseased with 100% accuracy.**

The confound also flows through the physics. The preprocessing upsamples healthy scans by about 1.32× and diseased ones by about 1.0×. Upsampling smooths texture, which raises the Hurst coefficient: the Spearman correlation between upsampling factor and mean H is 0.82.

The strongest test was to shrink the held-out diseased scans to the healthy scans' width and run the identical pipeline. The trained model then called **63 of 63 of them healthy**. The perfect triage was the model recognizing the scanner.

**Lesson #1:** check acquisition confounds before claiming triage performance.

## Step 2: test fairly

I added two more datasets, each from a single scanner:

- **OCTDL** (Optovue, 2,064 scans, 7 classes)
- **Kermany** (Spectralis, 2,401 scans)

Every split is **by patient**, so no patient appears in both training and test. The hardest test is **cross-scanner**: train on Spectralis, test on Optovue.

The evaluation setup:

- **Classifier:** logistic regression or LightGBM.
- **Validation:** patient-grouped 5-fold CV, repeated 3 times, with bootstrap 95% confidence intervals.
- **Metric:** balanced accuracy, because the classes are imbalanced.

## Step 3: physics features at three scales

The new pipeline extracts **171 features** in three layers. Every one has a physical meaning.

**1. Layer texture (111 features).** First trace the inner retinal surface (ILM) and the RPE layer, then flatten the retina so the layers lie horizontal. Next, split it into four anatomical bands: inner retina, outer retina, RPE and choroid. In each band, measure Hurst roughness, KT-1 slope, coherence (how layered the texture is), lacunarity (gappiness) and entropy.

On top of the texture, add direct disease measurements in micrometres:

- thickness and foveal dip
- macular hole gap
- fluid and bright foci
- RPE elevation

**2. Echo physics, the NDT layer (46 features).** Each image column is an A-scan, the same as an ultrasonic echo trace in materials testing. From it we compute:

- **Attenuation:** the Beer–Lambert slope of light loss with depth
- **Interface contrast:** echo strength at the ILM and RPE, an analogue of a reflection coefficient
- **6 dB half-max sizing:** the size of fluid pockets and holes
- **Shadowing:** signal loss behind bright reflectors
- **Speckle statistics**

We removed the 8 features that turned out to be scanner-dependent.

**3. Boundary geometry, the fractal layer (14 features).** This treats the ILM and RPE as coastlines.

## The retina as a coastline

If z(x) is the height of a boundary at column x, a self-affine curve satisfies

<p style="text-align:center; font-size:1.15rem;"><strong>E</strong> |z(x + d) − z(x)| ∝ d<sup>H</sup> &nbsp;⇒&nbsp; <strong>D = 2 − H</strong></p>

Here H is the Hurst exponent and D is the fractal dimension: 1 for a straight line, about 1.25 for Britain's coastline, and 2 for a curve that fills the plane.

{% include figure.liquid path="assets/img/blog/retina/coastline.png" class="img-fluid rounded z-depth-1" zoomable=true caption="A healthy ILM (D 1.09, smooth foveal pit) vs epiretinal membrane (D 1.22, fine wrinkles)." %}

Before trusting the estimators, we checked them on shapes with known answers:

- **Sierpiński carpet:** measured 1.8928, true value log 8 / log 3 = 1.8928
- **Koch curve:** measured 1.267, true value 1.262
- **Boundary Hurst estimator:** within ±0.01 on synthetic Brownian profiles

The same checks confirmed the resampling confound: 1.32× upsampling raises the original engine's H by up to 0.04.

{% include figure.liquid path="assets/img/blog/retina/known-fractals.png" class="img-fluid rounded z-depth-1" zoomable=true caption="Validating the estimators on known fractals." %}

## Results

{% include figure.liquid path="assets/img/blog/retina/results.png" class="img-fluid rounded z-depth-1" zoomable=true caption="Balanced accuracy on every task: the original engine, our physics features, and a ResNet-18." %}

| Task | Chance | Physics features (ours) |
|---|---:|---:|
| OCTID: CSR / DR / MH | 0.33 | **0.868** |
| OCTDL: 7 classes | 0.14 | **0.682** |
| Kermany: CNV / DME / drusen | 0.33 | **0.726** |
| Spectralis → Optovue (cross-scanner) | 0.33 | **0.671** (original engine 0.378) |

- **Beats the original engine everywhere:** +0.13 to +0.36 balanced accuracy over the original engine on every task.
- **Holds up across scanners:** 0.671 balanced accuracy, and **84%** of healthy Optovue eyes recognized, after training only on Spectralis.
- **NDT features help a little, and consistently:** +0.01 to +0.04, positive in 22 of 24 paired tests.
- **A CNN is still more accurate:** a fine-tuned ResNet-18 on the same folds reaches 0.86–0.94.

{% include figure.liquid path="assets/img/blog/retina/cross-scanner-confusion.png" class="img-fluid rounded z-depth-1" zoomable=true caption="Trained on Spectralis, tested on Optovue." %}

## What the fractal features add (and don't)

{% include figure.liquid path="assets/img/blog/retina/fractal-by-disease.png" class="img-fluid rounded z-depth-1" zoomable=true caption="ILM fractal dimension and RPE roughness by disease." %}

- **Meaningful:** epiretinal membrane has the highest ILM fractal dimension (1.16 vs 1.10 healthy). AMD and vascular occlusions roughen the RPE (4–5 vs 2.7 µm per column).
- **Scanner-stable:** between healthy eyes on the two scanners, the effect size is \|d\| ≤ 0.58. The strongest NDT contrasts are about 1.5.
- **Redundant for accuracy:** on their own they reach 2–3× chance (0.672 on OCTID, 0.533 across scanners). Added on top of texture and NDT features, they change accuracy by −0.015 to +0.020, which isn't significant.

They **explain**; they don't **add accuracy**.

## Take-aways

1. **Check acquisition confounds first.** A perfect score is a reason to look harder at the data.
2. **Physics at three scales** gives 0.68–0.87 balanced accuracy, and a clinician can read every feature.
3. **Fractal dimension is a validated, scanner-stable biomarker** for ERM and AMD, but redundant for accuracy.
4. **A CNN is more accurate.** Physics features fit best as the *explanation layer* of a hybrid system, not as a replacement.

**Limitations:**

- The boundary tracer is simple and jumps at some foveal pits.
- The lateral pixel spacing is assumed, not read from the scanner.
- Only a subset of Kermany was used.

📄 [Poster (PDF)](/assets/pdf/posters/retinal-diagnostics-poster.pdf) · 💻 [Code on GitHub](https://github.com/Tanushka-Sonde/Multi-Parametric-Retinal-Diagnostics) · 🗂️ [Project page](/projects/retinal-diagnostics/)

### References

1. Banerjee, Sarkar, Munshi, *Multi-Parametric Retinal Diagnostics: A Deterministic Physics-Based Engine for Tomographic Classification.*
2. Athe, Shakya, Munshi, Luke, Mewes, *Flow Meas. Instrum.* 33, 2013.
3. Mandelbrot, *The Fractal Geometry of Nature*, 1982.
4. Kermany et al., *Cell* 172(5), 2018.
5. Kulyabin et al., OCTDL, *Sci. Data* 11, 2024.
6. Gholami et al., OCTID, arXiv:1812.07056.
