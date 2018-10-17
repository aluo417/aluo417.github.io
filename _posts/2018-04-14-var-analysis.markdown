---
title: VaR during the 2008 Financial Crisis
description: 
categories:
 - project
tags: VaR, crisis, R
---

> All major banks use Value-at-Risk as a measure of market risk. As part of their disclosure to investors, banks report how they measure and manage market risk, including how they use Value-at-Risk. They also report on how their Value-at-Risk models have performed.

<!-- more -->

## Citigroup's Practices Concerning the Market Risk
For trading portfolio, Citigroup as an example, it calculates the VaR as the potential decline in the value of a position or portfolio assuming normal market conditions. Their model incorporate the factor sensitivities of the trading portfolio with the volatilities and correlations of those factors and is expressed as the risk to the Company over a one-day holding period at 99% confidence level. Factors include Interest rate, foreign exchange, equity and commodity.

In 2008, there are 21 negative trading-related net loss greater than \\$400 million. Approximately, there are 24 VaR exceptions observed in 2008 according to the VaR as of Dec 31, 2008.

As a result of the financial crisis, VaR computation requires a global view to prevent sysmatic market risk.

## VaR on each day of 2008 from daily stock price from 2006 to 2008
```r
library(tidyquant)
date_start <- "2006-01-01"
date_end <- "2008-12-31"
bank_tickers <- c("GS", "UBS", "JPM", "C", "BCS", "MS", "DB", "BAC", "BNPQY", "CS")
bank_stocks <- tq_get(bank_tickers, get = "stock.prices", from = date_start, to = date_end)

bank_returns <- bank_stocks %>%
  group_by(symbol) %>%
  tq_transmute(select = adjusted,
               mutate_fun = periodReturn,
               period = "daily",
               col_rename = "return")
```

### 99% 1-day VaR for the stock return using the historical method with all past data in the sample

```r
C <- bank_returns %>%
  filter(symbol == 'C')
  
C_n1 <- C %>%
  filter(year(date) == 2006 | year(date) == 2007) %>%
  nrow(.)
C_n2 <- C %>%
  filter(year(date) == 2008) %>%
  nrow(.)

C_VaR <- rep(0,C_n2)
for(i in (C_n1+1):(C_n1 + C_n2)){
  
  C_VaR[i-C_n1] <- sort(C$return[1:(i-1)])[ceiling((i-1)*0.01)]
  
}

C_2008 <- C %>%
  filter(year(date) == 2008) %>%
  mutate(VaR = C_VaR)

save(C_2008, file = "q2a.RData")

C_2008 %>%
  ggplot(aes(x = date)) +
  geom_line(aes(y = return*100, color = "Return")) +
  geom_line(aes(y = VaR*100, color = "VaR")) +
  labs(title = "VaR Measurement",
       subtitle = "Citi Group Year 2008 Daily Return",
       caption = "Fig. 1",
       x = "date", y = "Return [%]")
```

* VaR Measurement
![VaR Measurement](\assets\images\var\var1.png)

### Backtest
With 99% 1-day VaR, the number observations excedding VaR over the year 2008 is supposed to be 2~3. When we back-test with the historical data, the exceptions number is as following:

```r
# number exceptions observed
C_exceptions <- sum(C_2008$return < C_2008$VaR)
cat("Exceptions: ", C_exceptions, "\n")
# probability of the above number
C_exceptions_prob <- dbinom(C_exceptions, C_n2, .01)
cat("The probability for the occurence of these exceptions:", C_exceptions_prob)
```

`Exceptions:  27 
The probability for the occurence of these exceptions: 1.553984e-19`

### Relation to the 2008 10-K
The exception number is relatively close to the exceptions of Citigroup's trading portfolio. To some extent it measures risk level during that period. However, our VaR only computated from the stock returns. While Citigroup used multiple factors to calculate for each component of VaR.

## VaR for all 10 banks

### VaR for a portfolio with \\$1m in the odd-numbered banks and \\$2m in the even-numbered banks using historical method

```r
split_list <- split(bank_tickers, 1:2)
names(split_list) <- c("odd", "even")

portfolio_ret <- bank_returns %>%
  mutate(gl = ifelse(symbol %in% split_list$odd, return * 1e6, return * 2e6)) %>%
  group_by(date) %>%
  summarise(port_gl = sum(gl))

portfolio_VaR <- rep(0,C_n2)
for(i in (C_n1+1):(C_n1 + C_n2)){
  portfolio_VaR[i-C_n1] <- -sort(portfolio_ret$port_gl[1:(i-1)])[ceiling((i-1)*0.01)]
}

portfolio_2008 <- portfolio_ret %>%
  filter(year(date) == 2008) %>%
  mutate(VaR = portfolio_VaR)

portfolio_2008 %>%
  ggplot(aes(x = date)) +
  geom_line(aes(y = port_gl, color = "Gain/Loss")) +
  geom_line(aes(y = -VaR, color = "VaR")) +
  labs(title = "Portfolio Gain/Loss",
       subtitle = "Portfolio Daily Gain/Loss over 2008",
       caption = "Fig. 2",
       x = "date", y = "Gain/Loss [$]")
```

* Portfolio P&L
![P&L](\assets\images\var\var2.png)

### Calculation of DVaR and CVaR
To calculate the DVaR and CVaR, we use numerical approximation:

$$\begin{aligned}
DVaR_i &= \frac{\partial VaR_{total}}{\partial x_i} \\
&= \frac{\Delta VaR_{total}}{\Delta x_i}\\
CVaR_i &= x_i \frac{\partial VaR_{total}}{\partial x_i}\\
&= \frac{\Delta VaR_{total}}{\Delta x_i / x_i}
\end{aligned}$$

```r
for(i in bank_tickers){
  temp_gl <- bank_returns %>%
    mutate(D_gl = ifelse(symbol %in% split_list$odd, 
                       ifelse(symbol == i, return*(1e6+1), return*1e6), 
                       ifelse(symbol == i, return*(2e6+1), return*2e6)),
           C_gl = ifelse(symbol %in% split_list$odd, 
                         ifelse(symbol == i, return*(1e6*1.01), return*1e6), 
                         ifelse(symbol == i, return*(2e6*1.01), return*2e6))) %>%
    group_by(date) %>%
    summarise(
      D_portfolio_gl = sum(D_gl),
      C_portfolio_gl = sum(C_gl)
    )
      
  DVaR <- vector()
  CVaR <- vector()
  for(j in (C_n1+1):(C_n1 + C_n2)){
    D_temp_portfolio_VaR <- -sort(temp_gl$D_portfolio_gl[1:(j-1)])[ceiling((j-1)*0.01)]
    C_temp_portfolio_VaR <- -sort(temp_gl$C_portfolio_gl[1:(j-1)])[ceiling((j-1)*0.01)]
    
    DVaR <- append(DVaR, (D_temp_portfolio_VaR - portfolio_VaR[j-C_n1]) / 1)
    CVaR <- append(CVaR, (C_temp_portfolio_VaR - portfolio_VaR[j-C_n1]) / .01)
  }
  DVaR_name <- paste("DVaR", i, sep = '.')
  CVaR_name <- paste("CVaR", i, sep = '.')
  portfolio_2008 <- portfolio_2008 %>%
    mutate(!!DVaR_name := DVaR,
           !!CVaR_name := CVaR)
}

portfolio_2008 %>%
  select(-port_gl, -VaR, -starts_with("CVaR.")) %>%
  gather(key = "bank", value = "DVaR", -date) %>%
  ggplot(aes(x = date, y = DVaR, color = bank)) + 
  geom_line() +
  labs(title = "DVaR",
       subtitle = "Sensitivity of Portfolio VaR to Position Change",
       caption = "Fig. 3",
       y = "DVaR")

portfolio_2008 %>%
  select(-port_gl, -starts_with("DVaR.")) %>%
  gather(key = "bank", value = "CVaR", -date, -VaR) %>%
  ggplot(aes(x = date)) + 
  geom_bar(aes(y = CVaR, fill = bank), stat='identity') +
  geom_line(aes(y = VaR), color="black") +
  labs(title = "CVaR",
       subtitle = "Component CVaR Sums up to Portfolio VaR [Black Line]",
       caption = "Fig. 4",
       y = "CVaR")
```

* DVaR
![DVaR](\assets\images\var\var3.png)

* CVaR
![CVaR](\assets\images\var\var4.png)

### Comparison & Analysis
We notice in Fig.3 that the portfolio VaR is most sensitive to the position change in Morgan Stanley based on the historical data from 2006 to 2008. The result from Fig.4 also confirms that the sum of CVaR equals the portfolio VaR with tiny error.

