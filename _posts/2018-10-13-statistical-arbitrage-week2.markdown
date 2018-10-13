---
title: statistical-arbitrage-week2
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

##

###