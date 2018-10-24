---
title: Application of Monte Carlo Simulation
description: This project is focused on the the application of Monte Carlo Simulations to solve statistical problems in financial engineering.
categories:
 - project
tags: Python
---

> The most common use of Monte Carlo Simulation in finance is when one needs to calculate the expected value of a functional $$\mathbb{E} f(X)$$

<!-- more -->

## Introduction

Assume $$g(\cdot)$$ is the density function of random variable $$X$$. Then we can express the expectation as an integral:

\\[ \mathbb{E}f(X) = \int_{-\infty}^{\infty}{f(t)g(t)dt} \\]

If this integral can’t be computed explicitly, then Monte Carlo simulation techniques are adopted to estimate it. The idea is to use the Law of large Numbers (LLN) to estimate the integral.

Suppose $$\{X_i\}_{i=1}^{n}$$ is a sample of _i.i.d._ random variables with the same distribution as $$X$$ and $$\mathbb{E}(X)=\mu, Var(X)=\sigma^2 < \infty$$.

For the Sample Mean for random sample $$\{ f(X_1), f(X_2),...,f(X_n) \}$$ defined as 

\\[ \overline{fX_n} = \frac{f(X_1), f(X_2),...,f(X_n)}{n} \\]

we have



