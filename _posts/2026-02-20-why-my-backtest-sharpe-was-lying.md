---
layout: post
title: "The Sharpe Ratio That Lied: Hunting Hidden Beta in My Backtest"
date: 2026-02-20 10:00:00
description: A Sharpe of 0.39 that turned out to be hidden market beta, and the statistics I used to catch it, in plain English.
tags: [quant, statistics, backtesting]
categories: notes
toc:
  sidebar: left
---

My first backtest of a cross-sectional ML signal on 70 US large caps showed a **net Sharpe of 0.39**. That's not spectacular, but it's respectable for a market-neutral long/short book on mega-caps. This post explains why that number was wrong and which tools showed it.

The short version:

- The book had a hidden **market beta of 0.19** (t = 7.8).
- Made truly beta-neutral, the same signal earns a **Sharpe of −0.28** after costs.
- Against the 24 configurations I tried, its **Deflated Sharpe Ratio is about 0**.
- **0 of 5** independent out-of-sample paths are profitable.

Everything here comes from my [alpha research framework](https://github.com/Tanushka-Sonde/alpha-research-framework).

## The setup

- **Universe:** 70 large caps across all 11 GICS sectors, 10 years of daily data.
- **Features:** 20 technical features plus 6 classic equity factors (12-1 momentum, distance from the 52-week high, beta, idiosyncratic volatility, Amihud illiquidity, max return). Every feature is **ranked across stocks each day**.
- **Target:** the cross-sectional rank of the 5-day forward return.
- **Model:** one pooled LightGBM model across all stocks.
- **Portfolio:** long the top quintile, short the bottom quintile, with 7 bps of costs on every trade.

## Lie #1: "market-neutral" wasn't

"Dollar-neutral" means the money on the long side equals the money on the short side. That is **not** the same as market-neutral. If the longs are high-beta stocks and the shorts are low-beta ones, the book is net long the market even though it is balanced in dollars. Over a bull market, that exposure looks like alpha.

That is exactly what happened. One of the model's top features was `beta_63`, so it learned to like high-beta stocks. A regression of daily returns on market, momentum and short-term reversal factors made it obvious:

| Same signal, portfolio built as… | Net Sharpe | Market beta (t) | Alpha (t) |
|---|---:|---:|---:|
| Dollar-neutral | 0.39 | **0.19 (7.8)** | 0.5% (0.19) |
| Dollar **and** beta neutral | **−0.28** | 0.04 (2.4) | −2.9% (−1.10) |

{% include figure.liquid path="assets/img/blog/alpha/ablation_beta.png" class="img-fluid rounded z-depth-1" zoomable=true caption="The same signal with and without beta neutralization." %}

The fix is to project the weights so they are orthogonal to both a vector of ones (dollar-neutral) and the vector of betas (beta-neutral). Once that exposure is gone, the edge goes with it.

**Lesson:** always regress a "neutral" strategy on the market before believing its Sharpe.

## Lie #2: one backtest path has no error bar

A walk-forward backtest gives you exactly one equity curve, and one Sharpe ratio with no error bar.

**Combinatorial Purged Cross-Validation (CPCV)** fixes that:

1. Split time into 6 groups.
2. Test on every combination of 2 groups: C(6,2) = 15 splits.
3. Stitch the splits into **5 complete, independent out-of-sample paths**.

Every split is purged of overlapping labels and embargoed, the same as in the walk-forward. The result is a distribution instead of a single number: Sharpe **−0.29 ± 0.34**, with 0 of 5 paths positive. The loss isn't caused by one unlucky split.

{% include figure.liquid path="assets/img/blog/alpha/cpcv_paths.png" class="img-fluid rounded z-depth-1" zoomable=true caption="Five CPCV out-of-sample paths." %}

## Lie #3: the best of 24 tries is biased upward

I didn't run one configuration. I ran a grid of **24**:

- forward-return horizon
- model
- portfolio rule
- quantile cut-off

If you pick the best of 24 and report its Sharpe, you're reporting the maximum of 24 noisy numbers, which is biased upward even if none of them has any skill.

### Probabilistic Sharpe Ratio (PSR)

PSR asks: given this track record's length, skew and fat tails, what's the probability the *true* Sharpe is above zero? For the main configuration it was **0.21**, so not convincing.

### Deflated Sharpe Ratio (DSR)

DSR raises the bar. It works out the Sharpe you would expect from the *luckiest* of N strategies with zero skill, using the spread of Sharpes you actually observed, and then asks whether your winner beats that.

With 24 trials, luck alone would be expected to produce a daily Sharpe of about 0.10, roughly **1.6 annualized**. The best trial was nowhere near that, so DSR ≈ **0.00**.

### Probability of Backtest Overfitting (PBO)

PBO, computed with CSCV over 12,870 splits, asks how often the trial ranked best in-sample ends up in the bottom half out-of-sample. It came out at **0.21**, which looks reassuring.

The full picture is more subtle. The trial *rankings* are stable, mostly because daily rebalancing is always the worst. But the in-sample winner still **loses money out-of-sample in 86% of splits**. Stable rankings don't mean profitability.

{% include figure.liquid path="assets/img/blog/alpha/experiments_pbo.png" class="img-fluid rounded z-depth-1" zoomable=true caption="All 24 trials, net of costs." %}

## Before neutralization, the warning signs were already there

Even before I found the beta problem, these tools were sceptical: DSR was 0.02 and PBO was 0.36, while the headline Sharpe looked fine. That gap is the reason to build the tools at all. A single backtest number tells you very little about whether a strategy is real.

## Things I'd tell myself before starting

- **Make every leakage control testable.** One test feeds the backtester a perfect-foresight signal (it must profit) and a same-day signal (it must not). That proves the P&L uses yesterday's weights.
- **Record every trial**, including the bad ones. You can't deflate a Sharpe if you've forgotten how many things you tried.
- **Costs decide.** All 8 daily-rebalanced trials lost heavily at 7 bps, with up to 111% daily turnover. Smoothing the signal and rebalancing weekly cut turnover to about 20% a day, but that still wasn't enough.
- **Be honest about the limits.** The universe has survivorship bias (today's large caps). The cost model is flat. The 24 trials are correlated, so the deflation is somewhat conservative.

The signal does have a little real information. That is a separate question, which I cover in the [next post on the alpha tear sheet](/blog/2026/alpha-tear-sheet/).

## References

- Bailey & López de Prado, "The Sharpe Ratio Efficient Frontier" (2012): Probabilistic Sharpe Ratio
- Bailey & López de Prado, "The Deflated Sharpe Ratio" (2014)
- Bailey, Borwein, López de Prado & Zhu, "The Probability of Backtest Overfitting" (2015)
- López de Prado, *Advances in Financial Machine Learning* (2018): purging, embargo, CPCV
