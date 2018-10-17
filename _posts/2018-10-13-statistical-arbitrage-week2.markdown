---
title: Statistical Arbitrage Week2
description: course notes of statistical arbitrage class
categories:
 - notes
tags: financial, econometrics, arbitrage, statistics
---

> Is _OPTIMAL_ the *optimized*?

<!-- more -->

## Positioning
According to Markovitz's portfolio selection process, there are two stages:
- Turn observation and experience into beliefs about the future performances of available securities
- Utilize relevant beliefs about future performances to contruct portfolio.

We shall focus on the second stage.

### Optimization Problem

Under MV thoery, capital invested should be proportional to the expected return. However, it is doubtful how can we determine the expected return from information in the past? Even if it worked, there might be noise in the signal.

Therefore, taking estimation error into account, do not allocate your capital in proportion to past returns.

## Counter-Intuition
According to Stein's work, **Shrinkage** is a better application since sample mean is an inadmissible estimator.

### Model Setup
- _God_ draws **skill** acrroding to \\(\mathcal{N}(\overline{\mu}, \delta^2)\\). Fund \\(i\\) has expected return \\(\mu_i \sim \mathcal{N}(\overline{\mu}, \delta^2)\\)
- **Independently**, _Lady Luck_ draws \\(T\\) observations around expected value \\(\mu_i\\) with random error: \\(x_{t,i} \sim \mathcal{N}(\mu_i, T\omega^2)\\)

### Shrinkage Target
- Compute sample mean from \\(T\\) observations:

$$m_i = \frac{x_{1i}+x_{2i}+\ldots+x_{Ti}}{T}$$

- Compute the **grand mean** from \\(n\\) sample means:

$$\overline{m} = \frac{m_1+m_2+\ldots+m_n}{n}$$

- Shrink every sample mean towards grand mean:

$$\hat{m_i} = (1-\beta)\overline{m} + \beta m_i$$

- Shrinkage Slope Range
	- \\(\beta = 1\\): no shrinkage, use sample means
	- \\(\beta = 0\\): full shrinkage, all means are equal (Global Minimum Variance Portfolio)
	- Optimum: somewhere between 0 and 1

## Estimate \\(\beta\\)
_Lady Luck_ is independent from _God (skill)_, therefore \\(m_i -\mu_i \text{ is independent of } \mu_i - \bar{\mu}\\)

$$\begin{align}
\beta &= \frac{Cov\Big[(m_i - \mu_i) + (\mu_i-\bar{\mu}), \mu_i -\bar{\mu}\Big]}{Var\Big[(m_i - \mu_i) + (\mu_i - \bar{\mu})\Big]}\\
\beta &= \frac{Var\Big[\mu_i - \bar{\mu}\Big]}{Var\Big[m_i - \mu_i\Big] + Var\Big[\mu_i -\bar{\mu}\Big]}\\
\beta &= \frac{\delta^2}{\omega^2 + \delta^2}
\end{align}$$

### Estimate \\(\omega^2\\)
Intuitively, dispersion in the time-series contains information about the amount of noise, thus

$$\hat{\omega}^2 = \frac1n \sum_{i=1}^{n}{\hat{\sigma_i}^2}=\frac{1}{nT(T-1)} \sum_{i=1}^{n}{\sum_{t=1}^{T}{(x_{ti}-m_i)^2}}$$

### Estimate \\(\delta^2\\)
The cross-sectional disperson of expected returns is derived from 

$$\begin{align}
\mathbb{E}[(m_i-\bar{\mu})^2] &= Var\Big[(m_i-\mu_i) + (\mu_i -\bar{\mu})\Big]\\
&= Var\Big[m_i -\mu_i\Big] + Var\Big[\mu_i -\bar{\mu}\Big]\\
&= \omega^2 + \delta^2
\end{align}$$

where \\(\mathbb{E}[(m_i -\bar{\mu})^2] \text{ can be estimated by }\frac1n \sum_{i=1}^{n}{(m_i - \bar{m})^2}\\)

therefore: \\(\hat{\delta}^2 = \frac1n \sum_{i=1}^{n}{(m_i -\bar{m})^2} - \hat{\omega}^2\\)

### Estimate Shrinkage Slope \\(\hat{\beta}\\)

$$\begin{align}
\hat{\beta} &= \frac{\hat{\delta}^2}{\hat{\omega}^2 + \hat{\delta}^2}\\
&= 1 - \frac{1}{T(T-1)}\cdot \frac{\sum_{i=1}^{n}{\sum_{t=1}^{T}{(x_{ti} - m_i)^2}}}{\sum_{i=1}^{n}{(m_i -\bar{m})^2}}
\end{align}$$

## Shrinkage Estimator of the Covariance Matrix

$$\begin{align}
\hat{\beta} &= \frac{\hat{\delta}^2}{\hat{\omega}^2 + \hat{\delta}^2}\\
&= 1 - \frac{1}{T(T-1)}\cdot \frac{\sum_{i=1}^{T}{\lvert\lvert X_t X_t' - S\rvert\rvert^2}}{\lvert\lvert S - \bar{\sigma}\mathbb{1}\rvert\rvert^2}
\end{align}$$

