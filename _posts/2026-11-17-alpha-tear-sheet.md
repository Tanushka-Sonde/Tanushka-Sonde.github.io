---
layout: post
title: "Is the Signal Real? Building an Alpha Tear Sheet"
date: 2026-11-17 08:30:00
description: IC, Newey-West t-stats, IC decay and quantile spreads, explained one plot at a time.
tags: [quant, statistics]
categories: notes
toc:
  sidebar: left
---

A backtest Sharpe mixes two different things:

- **Signal quality:** does the model rank stocks correctly?
- **Portfolio choices:** sizing, rebalancing, costs and neutralization.

In my [previous post](/blog/2026/why-my-backtest-sharpe-was-lying/), the same signal swung from a Sharpe of +0.39 to −0.28 purely because of how the portfolio was built. So before building a portfolio, I want to judge the signal on its own. That is the job of an **alpha tear sheet**.

All numbers below come from my [alpha research framework](https://github.com/Tanushka-Sonde/alpha-research-framework): a pooled LightGBM model on 70 US large caps, tested out-of-sample from January 2018 to July 2026.

## 1. Information coefficient (IC)

The **IC** is the Spearman rank correlation, across all stocks on one date, between today's signal and the forward return that follows. A perfect signal has IC = 1 and noise has IC ≈ 0. In real equity markets, a mean IC of a few hundredths is already considered a useful signal.

Computing it per date and then averaging matters. Pooling every (date, stock) pair would mostly measure market moves, which a neutral book doesn't trade.

| Metric | Value |
|---|---:|
| Mean daily rank IC (5-day horizon) | 0.0054 |
| Days with positive IC | 50.5% |
| IC t-stat (Newey-West) | 0.93 |

{% include figure.liquid path="assets/img/blog/alpha/ic.png" class="img-fluid rounded z-depth-1" zoomable=true caption="Daily rank IC and its rolling mean." %}

## 2. Why the t-stat needs Newey-West

With a 5-day forward-return label, the label for Monday and the label for Tuesday share four days of returns. Consecutive daily ICs are therefore **autocorrelated**. A naive t-stat treats them as independent, which counts the same information several times and overstates significance.

**Newey-West (HAC)** standard errors correct for this by adding the autocovariances up to a lag. I set the lag to the label horizon. With that correction the t-stat is **0.93**, which is not statistically significant on its own. That's an honest result, and it matches what the overfitting statistics said.

## 3. IC decay: where does the information live?

Measuring the same signal's IC against forward returns at several horizons shows how quickly its information fades or builds:

| Horizon (days) | 1 | 5 | 21 |
|---|---:|---:|---:|
| Mean IC | 0.0023 | 0.0054 | 0.0089 |

The IC **rises** with horizon. This signal is slow and factor-like, not a short-term reversal effect. That has a direct consequence: trading it daily pays a lot in costs for information that only shows up over weeks. Sure enough, every daily-rebalanced configuration in my grid was the worst.

## 4. Quantile returns: is it monotone?

Each day, split the stocks into five buckets by signal and record each bucket's average next-day return. A real signal should look like a **staircase**, not just one good bucket at one end.

- Top minus bottom quintile, gross: **8.2% per year**
- Monotonicity (Spearman correlation between bucket rank and bucket return): **0.30**

The edge is mostly at the extremes, and the staircase is noisy.

{% include figure.liquid path="assets/img/blog/alpha/quantiles.png" class="img-fluid rounded z-depth-1" zoomable=true caption="Average return of each signal quintile." %}

## 5. Signal autocorrelation: what will turnover cost?

This is the rank correlation between the signal on day *t* and on day *t − 1*. It came out at **0.71**.

A higher value means a more stable ranking, so you trade less to follow it. At 0.71 the ranking still changes enough day to day that, without smoothing, daily turnover reached about 111%. An exponential moving average of the signal combined with weekly rebalancing brought it down to roughly 20%.

## 6. Factor attribution

Finally, regress the strategy's daily returns on known factors, with HAC errors:

- market
- momentum
- short-term reversal

Whatever is left over (the intercept) is the alpha. This regression exposed the hidden market beta of 0.19 in the dollar-neutral book.

## 7. What the model actually uses

{% include figure.liquid path="assets/img/blog/alpha/feature_importance.png" class="img-fluid rounded z-depth-1" zoomable=true caption="LightGBM feature importance." %}

The top features are classic, well-documented anomalies:

- illiquidity
- 12-1 momentum
- beta
- volatility
- distance from the 52-week high

That's reassuring, because the model found known effects rather than noise. It is also humbling, because those effects are weak among mega-caps, and the market already prices them.

## Verdict

The signal carries **a little real information**: positive IC that grows with horizon, a gross quintile spread, and sensible features. It is **not enough to survive costs** in a 70-stock mega-cap universe once the beta exposure is removed.

The tear sheet lets me say that precisely, instead of "the backtest didn't work." Cheaper execution, a broader universe, or slower rebalancing would be the next things to try.

### References

- Grinold & Kahn, *Active Portfolio Management*: the information coefficient and IC-based evaluation
- Newey & West (1987): HAC standard errors
