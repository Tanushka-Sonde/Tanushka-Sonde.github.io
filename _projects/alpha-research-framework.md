---
layout: page
title: "Alpha Research & Backtesting Framework"
description: "Cross-sectional equity research with CPCV, a beta-neutral backtester and overfitting statistics (Deflated Sharpe, PBO)."
category: ML Systems
importance: 2
date_range: "Jan – Feb 2026"
github: https://github.com/Tanushka-Sonde/alpha-research-framework
---

**Jan – Feb 2026** · Individual Project

<a class="btn btn-sm z-depth-0" role="button" href="https://github.com/Tanushka-Sonde/alpha-research-framework"><i class="fa-brands fa-github"></i> GitHub</a> <a class="btn btn-sm z-depth-0" role="button" href="/blog/2026/why-my-backtest-sharpe-was-lying/"><i class="fa-solid fa-pen"></i> Blog post</a>

## What I did

- Built a cross-sectional equity research pipeline training a pooled **LightGBM** model on a **70-stock × 10-year** panel with 26 rank-normalized features (technical signals plus classic factors: 12-1 momentum, beta, Amihud illiquidity).
- Implemented **purged walk-forward** and **Combinatorial Purged CV** (15 splits → 5 out-of-sample paths) with embargo, and a vectorized backtester (**300× faster**) with costs, vol targeting and **dollar- and beta-neutral** portfolios.
- Built an **alpha tear sheet** (rank IC with Newey-West t-stats, IC decay, quintile spreads, factor attribution) and overfitting diagnostics (**Deflated Sharpe**, **PBO** via CSCV over 12,870 splits) across a 24-trial experiment grid.
- Exposed a naive **0.39 Sharpe** as hidden market beta (β = 0.19, t = 7.8): beta-neutral net Sharpe **−0.28**, DSR ≈ 0, 0/5 CPCV paths positive; backed by **31 pytest tests** with CI, a **Streamlit** dashboard and **Docker Compose**.

**Tech:** LightGBM, pandas, statsmodels, pytest, Streamlit, Docker Compose
