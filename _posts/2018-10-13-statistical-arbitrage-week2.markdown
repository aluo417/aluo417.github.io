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

## Illustration by Simulated Examples

To understand shrinkage better, we shall test with simulated examples in MATLAB.

### Generate cross-section of portfolio manager talent (expected return)

Suppose there are 500 portfolio managers and we estimate their performance over 5-year track record. Given that their average monthly return is 0.5%, with 2% volatility for monthly returns. And we assume 0.25% cross-sectional standard deviation of their true skills.

```matlab
n=500;      % number of portfolio managers
t=60;       % number of months of track record (5 years)
mu=0.5;     % percentage return per month for average manager (6% per year)
delta=0.25; % cross-sectional standard deviation of skill levels

rng(1,'v4')
expret=mu+delta*randn(1,n);
histogram(expret)
```

* Expected Return (only affected by managers' skill)
![expected_ret](\assets\images\statistical-arbitrage\expected_ret.png)

### Generate realized-monthly performance

In realword, the performance of the portfolio is not solely determined by the managers' skill. Therefore, we shall embed volatility to monthly returns.

```matlab
omega=2;    % standard deviation of monthly returns (~7% per year)
realret=repmat(expret,[t 1])+omega*randn(t,n);
histogram(realret(:))
```
* Observed Monthly Realized Return
![realized_ret](\assets\images\statistical-arbitrage\realized_ret.png)

Then we estimate the average realized monthly returns for each manager.

```matlab
averet=mean(realret);
histogram(averet)
```

* Average Realized Return
![average_realized_ret](\assets\images\statistical-arbitrage\average_realized_ret.png)

Based on these parameters, it is natural to see more some portfolio managers may result underwater and some beat benchmark with superior performance.

### Regress Observed Average Return on True Expected Return

```matlab
b1 = regress(averet',[ones(n,1) expret'])
avefit1 = b1(1)+b1(2) * expret;
plot(expret, averet, '.', expret, avefit1, '-')
```

`b1 = [0.0295 0.9976]'`

* Historical Performance vs. True Skill
![average_realized_ret](\assets\images\statistical-arbitrage\avg_vs_expected_ret.png)

The near zero intercept and almost 1 slope mean the performance is very good reflection of true skill.

### Reversely, Regress True Expected Return on Observed Return

```matlab
b2 = regress(expret',[ones(n,1) averet'])
expfit2 = b2(1)+b2(2)*averet;
plot(averet,expret,'.',averet,expfit2,'-',averet,averet,'--')
```

`b2 = [0.2411 4884]'`

* True Skill vs. Historical Performance
![realized_average_ret](\assets\images\statistical-arbitrage\expected_vs_avg_ret.png)

### Compare Fitted Slope with Theoretical One

We calculate the adjusted return as shrinked one from observed performance, that is, to eliminate the affect from `omega`.

```matlab
% normalized by cross-sectional average expecter return
expret3=expret-mean(expret);
averet3=averet-mean(averet);
b3=regress(expret3',averet3')
expfit3=b3*averet3;
plot(averet3,expret3,'.',averet3,expfit3,'-',averet3,averet3,'--')
xlabel('Historical Performance Relative to Peer Average')
ylabel('True Skill Relative to Peer Average')
axis('equal')

% compare with theoretical shrinkage slope
Beta = delta^2/(delta^2+(omega^2/t))
plot(averet3,expret3,'.',averet3,expfit3,'-',averet3,averet3,'--', ...
   averet3,Beta*averet3,'-.')
xlabel('Historical Performance Relative to Peer Average')
ylabel('True Skill Relative to Peer Average')
axis('equal')
```

`b3 = 0.4884`

`Beta = 0.4839`

* Compared to Theoretical Slope
![compare](\assets\images\statistical-arbitrage\compare.png)

### Estimate Noise Magnitude from Panel Data of Monthly Returns

```matlab
sigmahat = var(realret);
omegahat = sqrt(mean(sigmahat));
```

`omega = 2.0000, omegahat = 2.0003`

### Estimate Cross-sectional Dispersion of True Skill Levels

```matlab
deltahat = sqrt(var(averet)-omegahat^2/t);
```

`delta = 0.2500, deltahat = 0.2427`

### Compare Fitted Slope with Estimated Slope

```matlab
betahat = deltahat^2/(deltahat^2+omegahat^2/t);
disp([b3 Beta betahat])
plot(averet3,expret3,'.',averet3,expfit3,'-',averet3,averet3,'--', ...
   averet3,Beta*averet3,'-.',averet3,betahat*averet3,':')
xlabel('Historical Performance Relative to Peer Average')
ylabel('True Skill Relative to Peer Average')
axis('equal')
```

`b3 = 0.2975, Beta = 0.2941, betahat = 0.2713`

* Estimated Slope
![betahat](\assets\images\statistical-arbitrage\betahat.png)


