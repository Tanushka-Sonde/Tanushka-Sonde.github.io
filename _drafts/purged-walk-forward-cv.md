---
layout: post
title: "Purged walk-forward CV: avoiding lookahead bias in backtests"
tags: [quant, backtesting, ml]
categories: [notes]
---

<!-- Outline only: write the text yourself. Points taken from the project's resume bullets. -->

- Built an end-to-end quant research pipeline transforming raw OHLCV market data into ML-driven return signals using a LightGBM gradient-boosting model, with an optional PyTorch LSTM for sequential modeling.
- Implemented purged walk-forward cross-validation with a configurable embargo period to eliminate lookahead bias from overlapping forward-return labels, validated via a dedicated pytest suite.
- Engineered a realistic backtest engine simulating live trading with transaction costs, slippage, and volatility-targeted position sizing, capping single-name exposure and tracking turnover-driven cost drag.
- Designed an interactive Streamlit dashboard visualizing equity curves, drawdowns, feature importance, and regime-based performance splits, containerized via Docker Compose for reproducible deployment.
