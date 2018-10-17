---
title: VaR Techniques
description: Different VaR techniques
categories:
 - project
tags: R, VaR
---

>

## Loading libraries
```r
library(data.table)
library(dplyr)
library(ggplot2)
```

## Historical VaR and exponential weighted 1-day 99%-VaR

```r
aa <- fread("./homework3_data.csv")
aa[, date := as.Date(as.character(date), format = "%m/%d/%y")]

aa_n1 <- aa %>%
  filter(year(date) == 2006 ) %>%
  nrow(.)
aa_n2 <- aa %>%
  filter(year(date) == 2007 | year(date) == 2008) %>%
  nrow(.)

### historical VaR
hist_VaR <- rep(0,aa_n2)
## exponential weighted
lmbd <- 0.995
weighted_VaR <- rep(0,aa_n2)

for(i in (aa_n1+1):(aa_n1 + aa_n2)){
  hist_VaR[i-aa_n1] <- sort(aa$gain[1:(i-1)])[ceiling((i-1)*0.01)]
  
  ind <- seq(1, i-1)
  bb <- aa[1:i-1]
  bb[, weight := lmbd^(i-1-ind) * (1-lmbd) / (1 - lmbd^(i-1))]
  setkey(bb, gain)
  bb[, cum_weight := cumsum(weight)]
  bb_index <- which(bb$cum_weight > 0.01)[1]
  weighted_VaR[i - aa_n1] <- bb$gain[bb_index]
}

aa[, historical_VaR := c(rep(NA, aa_n1), hist_VaR)]
aa[, exp_weighted_VaR := c(rep(NA, aa_n1), weighted_VaR)]
aa[, historical_exceptions := ifelse(gain < historical_VaR, gain, NA)]
aa[, exp_weighted_exceptions := ifelse(gain < exp_weighted_VaR, gain, NA)]

aa[year(date) == 2007 | year(date) == 2008,] %>%
  ggplot(aes(x = date))+
  geom_line(aes( y = gain), color = "blue")+
  geom_line(aes( y = historical_VaR), color = "red")+
  geom_line(aes( y = exp_weighted_VaR), color = "orange")+
  geom_point(aes(y = historical_exceptions), color = "red", shape = 4)+
  geom_point(aes(y = exp_weighted_exceptions), color = "orange", shape = 0)
```

* EVaR vs. Historical 99% 1-day VaR
![EVaR vs Historical](\assets\images\vart\vart1.png)

From the plot we see all the exponential weighted VaR exceptions are captured by historical method. There are five exceptions only captured by historical method. This makes sense as exponential weighted method places more weight on more recent movements, for a period with increasing volatility the VaR is larger than the result from historical method. On the other hand, exponential weighted method places less weight on the far-end volatile period.

## 95% Confidence Intervals of the Historical VaR and the Exponential Weighted VaR
### Historical VaR
#### Parametric Method
We approximate the gain by normal distribution assumption with its sample mean and sample standard deviation.

$$\begin{aligned}
x &= \mu - \sigma \Phi^{-1}(0.01)\\
f(x) &= \frac{exp(-\frac{(x-\mu)^2}{2\sigma^2})}{\sigma \sqrt{2\pi}}\\
StdDev(VaR) &= \frac{1}{f(x)} \sqrt{\frac{0.99 \times 0.01}{n}}
\end{aligned}$$

```r
l_bound <- rep(0, aa_n2)
u_bound <- rep(0, aa_n2)
for(i in (aa_n1+1):(aa_n1 + aa_n2)){
  appx_mean <- mean(aa$gain[1:(i-1)])
  appx_sd <- sd(aa$gain[1:(i-1)])
  x <- appx_mean + appx_sd*qnorm(.01)
  fx <- 1 / (appx_sd * sqrt(2*pi)) * exp(-(x - appx_mean)^2 / (2*appx_sd^2))
  VaR_std <- 1 / fx * sqrt(.99 * .01 / (i-1))
  
  l_bound[i-aa_n1] <- hist_VaR[i-aa_n1] + VaR_std * qnorm(.025)
  u_bound[i-aa_n1] <- hist_VaR[i-aa_n1] + VaR_std * qnorm(.975)
}

aa[, lower_bound := c(rep(NA, aa_n1), l_bound)]
aa[, upper_bound := c(rep(NA, aa_n1), u_bound)]
aa[year(date) == 2007 | year(date) == 2008,] %>%
  ggplot(aes(x = date))+
  geom_line(aes(y = historical_VaR)) +
  geom_ribbon(aes(ymin = lower_bound, ymax = upper_bound), alpha = 0.3)+
  labs(y = "Historical VaR", title = "95% Confidence Interval of Historical VaR")
```

* 95% Confidence Interval of Historical VaR
![95% c.i. VaR](\assets\images\vart\vart2.png)

#### Bootstrap Method

```r
VaR_99 <- function(sample){
  l <- length(sample)
  VaR <- sort(sample)[ceiling(.01*l)]
  return(VaR)
}

bsp_l_bound <- rep(0, aa_n2)
bsp_u_bound <- rep(0, aa_n2)
for(i in (aa_n1+1):(aa_n1 + aa_n2)){
  B <- 1000 # 1000 sets
  n <- i-1 # number of sample
  boot.samples <- matrix(sample(aa$gain[1:n], size = B * n, replace = TRUE), B, n)
  boot.statistics <- apply(boot.samples, 1, VaR_99)
  boot.statistics.sorted <- sort(boot.statistics)
  
  bsp_l_bound[i-aa_n1] <- boot.statistics.sorted[25]
  bsp_u_bound[i-aa_n1] <- boot.statistics.sorted[975]
}

aa[, bsp_lower_bound := c(rep(NA, aa_n1), bsp_l_bound)]
aa[, bsp_upper_bound := c(rep(NA, aa_n1), bsp_u_bound)]
aa[year(date) == 2007 | year(date) == 2008,] %>%
  ggplot(aes(x = date))+
  geom_line(aes(y = historical_VaR)) +
  geom_ribbon(aes(ymin = bsp_lower_bound, ymax = bsp_upper_bound), alpha = 0.3)+
  labs(y = "Historical VaR", title = "95% Confidence Interval of Historical VaR")
```

* Bootstrap VaR 95% Confidence Interval
![Bootstrap VaR](\assets\images\vart\vart3.png)

### Exponential Weighted VaR
To bootstrap the 95% confidence interval of exponential weighted VaR, we use similar ways in bootstraping historical VaR. However, since the gains are weighted by time, we can assign probability of being sampled during the sampling stage.

```r
### exponential decay
exp_bsp_l_bound <- rep(0, aa_n2)
exp_bsp_u_bound <- rep(0, aa_n2)
for(i in (aa_n1+1):(aa_n1 + aa_n2)){
  B <- 1000 # 1000 sets
  n <- i-1 # number of sample
  lmbd <- .995
  ind <- seq(1, n) # index of sequence
  prob <- lmbd^(n-ind) * (1-lmbd) / (1 - lmbd^n)
  boot.samples <- matrix(sample(aa$gain[1:n], size = B * n, replace = TRUE, prob = prob), B, n)
  boot.statistics <- apply(boot.samples, 1, VaR_99)
  boot.statistics.sorted <- sort(boot.statistics)
  
  exp_bsp_l_bound[i-aa_n1] <- boot.statistics.sorted[25]
  exp_bsp_u_bound[i-aa_n1] <- boot.statistics.sorted[975]
}

aa[, exp_bsp_lower_bound := c(rep(NA, aa_n1), exp_bsp_l_bound)]
aa[, exp_bsp_upper_bound := c(rep(NA, aa_n1), exp_bsp_u_bound)]
aa[year(date) == 2007 | year(date) == 2008,] %>%
  ggplot(aes(x = date))+
  geom_line(aes(y = exp_weighted_VaR)) +
  geom_ribbon(aes(ymin = exp_bsp_lower_bound, ymax = exp_bsp_upper_bound), alpha = 0.3)
```

* Exponential Weighted VaR
![EVaR](\assets\images\vart\vart4.png)

## Assess the Normality of the Gain Distribution
From the plot and test statistic of Shapiro-Wilk test, the gain does not follow a normal distribution as the p-value is almost zero. And its kurtosis is too high for normal distribution.

* Geometric Density Plot
![Gplot](\assets\images\vart\vart5.png)

* qqplot
![qqplot](\assets\images\vart\vart6.png)

`Shapiro-Wilk normality test

data:  aa$gain
W = 0.95533, p-value = 3.656e-11

Skewness: 0.08193283 
Excess Kurtosis: 4.16565`

## Normalize gains with estimated volatility
From the plot we see the distribution of normalized gain is more negatively-skewed than the unnormalized gain. And the Shapiro-Wilk test stats gives that the normalized gains are closer to normal distribution. And its excess kurtosis is much closer to zero.

```r
monthly_sigma <- rep(NA, aa_n1+aa_n2)
for(i in 22:(aa_n1+aa_n2)){
  monthly_sigma[i] <- sd(aa[(i-21):(i-1), gain])
}
aa[, monthly_volatility := monthly_sigma]
aa[, norm_gain := gain / monthly_volatility]

norm_gain <- na.omit(aa$norm_gain)
aa %>%
  ggplot(aes(norm_gain))+geom_density()
# plot(density(norm_gain))

qqplot.data(aa$norm_gain)

shapiro.test(aa$norm_gain)
cat("Skewness:", moments::skewness(aa$norm_gain, na.rm = T), "\n")
cat("Excess Kurtosis:", moments::kurtosis(aa$norm_gain, na.rm = T)-3)
```

* Geometric Density Plot
![Gplot2](\assets\images\vart\vart7.png)

* qqplot
![qqplot2](\assets\images\vart\vart8.png)

`Shapiro-Wilk normality test

data:  aa$norm_gain
W = 0.9862, p-value = 0.0001652

Skewness: -0.3525669 
Excess Kurtosis: 0.8193637`

## Normalize gains with future volatility
From the plot we see the distribution of normalized gain is slightly positively-skewed but very close to zero skewness. And the Shapiro-Wilk test stats gives that the normalized gains are closer to normal distribution compared to the above two exmaples. And its excess kurtosis is even much closer to zero.

```r
future_monthly_sigma <- rep(NA, aa_n1+aa_n2)
for(i in 22:(aa_n1+aa_n2)){
  future_monthly_sigma[i-21] <- sd(aa[(i-21):(i-1), gain])
}
aa[, future_monthly_volatility := future_monthly_sigma]
aa[, future_norm_gain := gain / future_monthly_volatility]

aa %>%
  ggplot(aes(future_norm_gain))+geom_density()

qqplot.data(aa$future_norm_gain)

shapiro.test(aa$future_norm_gain)
cat("Skewness:", moments::skewness(aa$future_norm_gain, na.rm = T), "\n")
cat("Excess Kurtosis:", moments::kurtosis(aa$future_norm_gain, na.rm = T)-3)
```

* Geometric Density Plot
![Gplot3](\assets\images\vart\vart9.png)

* qqplot
![qqplot3](\assets\images\vart\vart10.png)

`Shapiro-Wilk normality test

data:  aa$future_norm_gain
W = 0.9951, p-value = 0.1348

Skewness: 0.07253502 
Excess Kurtosis: -0.05601283`




