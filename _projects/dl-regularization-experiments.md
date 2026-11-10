---
layout: page
title: "Deep Learning and Regularization Experiments"
description: "Next-word prediction MLP, a regularization study on make_moons, and MLP/CNN models on MNIST."
category: Deep Learning & ML
importance: 4
img: assets/img/projects/placeholder.svg # optional: replace with a thumbnail in assets/img/projects/
date_range: "Oct 2025"
github: https://github.com/Tanushka-Sonde/ES335-Machine_learning-assignment-3
---

**Oct 2025** · Individual Project, Machine Learning, Prof. Nipun Batra

<a class="btn btn-sm z-depth-0" role="button" href="https://github.com/Tanushka-Sonde/ES335-Machine_learning-assignment-3"><i class="fa-brands fa-github"></i> GitHub</a>

## What I did

- Built a **Next-Word Prediction MLP** trained on both natural and structured text; implemented embeddings (**dim=64**), ReLU activations, and trained for up to **1000 epochs**. Visualized learned word embeddings using **t-SNE** to reveal meaningful semantic clusters.
- Deployed an interactive **Streamlit text generator app** with user controls for context length, temperature, and model variant; handled out-of-vocabulary inputs dynamically.
- Conducted a detailed **Regularization Study** on the `make_moons` dataset — compared early stopping, L1/L2 penalties, and logistic regression baselines; demonstrated **L1-induced sparsity (up to 70%)**, **L2-smoothing of decision boundaries**, and robust generalization under class imbalance.
- Trained and evaluated **MLP and CNN models** on MNIST achieving **>98% accuracy**; visualized 20-dimensional embeddings with t-SNE and tested cross-domain transfer to Fashion-MNIST. Compared with pretrained **AlexNet and MobileNet**, analyzing accuracy, F1-score, and inference latency.

**Tech:** t-SNE, Streamlit, MNIST, Fashion-MNIST, AlexNet, MobileNet
