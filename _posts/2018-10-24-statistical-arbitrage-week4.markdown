---
title: Statistical Arbitrage Week4
description: course notes of statistical arbitrage class
categories:
 - notes
tags: financial, econometrics, arbitrage, statistics
---

> Portfolio Optimization

<!-- more -->

## Structure of Optimizer

- Risk Model
	- Shrinkage estimator of the covariance matrix of stock returns
- Transaction Cost Model
	- \\(1 \text{bp} + \frac12 \text{bid-ask spread}\\)
- Alpha
	- Weighted blend of various standardized, winsorized alphas

### Input

- position as of close of business on day \\(t-1\\)
- alphas using data observed up to day \\(t-1\\)
- transaction costs using data observed up to day \\(t-1\\)
- risk model using data observed up to day \\(t-1\\)
- constraints using data observed up to day \\(t-1\\)

### Objectives & Constraints

- Minimize risk: \\(x' \Sigma x\\)
- Maximize exposure to alpha: \\(\alpha' x\\)
- Neutralize exposure to beta: \\(\beta' x = 0\\)
- Minimize transaction costs: \\(\tau' \lvert x-w \rvert\\)
- Other constraints:
	- maximum trade size
	- maximum position size
	- maximum industry and country exposure

* Notations

| Variable  | Dimension  | Definition  |
|:-:|:-:|:-:|
| \\(\mathbf{x}\\)  | \\(n \times 1\\)  | desired portfolio weights  |
|  \\(\mathbf{w}\\) |  \\(n \times 1\\) |  initial portfolio weights |
| \\(\boldsymbol{\Sigma}\\)  | \\(n \times n\\)  |  covariance matrix of stock returns |
| \\(\boldsymbol{\alpha}\\)  | \\(n \times 1\\)  |  aggregate alphas |
| \\(\boldsymbol{\beta}\\)  | \\(n \times 1\\)  |  historical betas |
| \\(\boldsymbol{\tau}\\)  | \\(n \times 1\\)  |  transaction costs |

The parametric problems formed as:

$$
max_{\mathbf{x}} \boldsymbol{\alpha}' \mathbf{x} - \lambda \boldsymbol{\tau}' \lvert \mathbf{x}-\mathbf{w} \rvert - \mu \mathbf{x}' \boldsymbol{\Sigma} \mathbf{x}
$$

subject to:

- beta neutrality: \\(\boldsymbol{\beta}' \mathbf{x} = 0\\)
- max trade and position: \\(\boldsymbol{\gamma} \leq \mathbf{x} \leq \boldsymbol{\delta}\\)
	- Max trade size for \\(i^{th}\\) stock: \\(\theta_i\\)
		- \\(\Rightarrow w_i - \theta_i \leq x_i \leq w_i + \theta_i\\)
	- Max position size for \\(i^{th}\\) stock: \pi_i
		- \\(\Rightarrow -\pi_i \leq x_i \leq \pi_i\\)
- industry constraint: \\(-r^* \cdot \mathbf{1} \leq \mathbf{R}' \mathbf{x} \leq r^* \cdot \mathbf{1}\\)
	- Sectors are a factor of risk
	- Difficult to time sector performance
	- Constrain industry exposure
	- But not to zero (will incur transaction cost)
	- \\(r^* = \$ 300,000 \text{ limit for }\$ 50 \times 50M \text{ book size}\\)
	- Industry Dummy
		- \\(\rho\\) industries
		- Boolean matrix \\(\mathbf{R}\\) of dimension \\(n \times \rho\\)
		- \\(R(i, j) = 1\\) if \\(i^{th}\\) stock belongs to \\(j^{th}\\) industry else 0
		- Every row of matrix \\(\mathbf{R}\\) has exactly one entry equal to 1; all other entries are equal to 0
- country constraint: \\(-f^* \cdot \mathbf{1} \leq \mathbf{F}' \mathbf{x} \leq f^* \cdot \mathbf{1}\\)
	- Countries are a factor of risk
	- Difficult to time country performance
	- Constrain country exposure
	- But not to zero (will incur too much transaction cost)
	- \\(f^* = \$ 100,000 \text{ limit for } \$ 50 \times 50M \text{ book size}\\)
	- Industry Dummy
		- \\(\varphi \\) industries
		- Boolean matrix \\(\mathbf{F}\\) of dimension \\(n \times \varphi\\)
		- \\(F(i, j) = 1\\) if \\(i^{th}\\) stock belongs to \\(j^{th}\\) industry else 0
		- Every row of matrix \\(\mathbf{F}\\) has exactly one entry equal to 1; all other entries are equal to 0


### Output

**trade** to be executed on day \\(t\\)

$$
\text{Final position}(t+1) = \text{Final position}(t) + \text{trade}(t)
$$

### Backtest Process

1. Load all necessary data into memory
2. Create the alphas
3. Start from portfolio with zero dollar invested
4. Loop over all days in backtest period
	- Every day: call optimizer to find optimal rebalancing trade given initial position
	- End-of-day position becomes initial position of next day
5. Compute P&L

