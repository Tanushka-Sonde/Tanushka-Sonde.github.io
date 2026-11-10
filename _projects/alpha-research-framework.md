---
layout: page
title: "Alpha Research & Backtesting Framework"
description: "Quant research pipeline with LightGBM signals, purged walk-forward CV and a realistic backtester."
category: ML Systems
importance: 2
img: assets/img/projects/placeholder.svg # optional: replace with a thumbnail in assets/img/projects/
date_range: "Jan – Feb 2026"
github: https://github.com/Tanushka-Sonde/alpha-research-framework
---

**Jan – Feb 2026** · Individual Project

<a class="btn btn-sm z-depth-0" role="button" href="https://github.com/Tanushka-Sonde/alpha-research-framework"><i class="fa-brands fa-github"></i> GitHub</a>

## What I did

- Built an end-to-end quant research pipeline transforming raw OHLCV market data into ML-driven return signals using a **LightGBM** gradient-boosting model, with an optional **PyTorch LSTM** for sequential modeling.
- Implemented **purged walk-forward cross-validation** with a configurable embargo period to eliminate lookahead bias from overlapping forward-return labels, validated via a dedicated **pytest** suite.
- Engineered a realistic backtest engine simulating live trading with transaction costs, slippage, and **volatility-targeted position sizing**, capping single-name exposure and tracking turnover-driven cost drag.
- Designed an interactive **Streamlit** dashboard visualizing equity curves, drawdowns, feature importance, and regime-based performance splits, containerized via **Docker Compose** for reproducible deployment.

**Tech:** LightGBM, PyTorch (LSTM), pytest, Streamlit, Docker Compose
