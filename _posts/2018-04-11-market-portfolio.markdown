---
title: Market Portfolio
description: Using CRSP Data to replicate market return as Kenneth French does.
categories:
 - project
tags: 
- R
- portfolio
---

> Construct the value-weighted market return using CRSP data, replicating the market return time series available in Kenneth French website. Also calculate the equal-weighted market return, and the lagged total market capitalization. Your output should be from January 1926 to December 2017, at a monthly frequency.

<!-- more -->

## Part 1
Before calculating the portfolio time series, I conduct a series of data cleaning as part of my PS Q1 function. Next, I describe my data cleaning process and their respective assumptions:

1. Universe of stocks: Following Ken French procedure, I restrict the sample to common shares (share codes 10 and 11) and to securities traded in the New York Stock Echange, American Stock Exchange, or the Nasdaq Stock Exchange (exchange codes 1, 2, and 3).
2. Missing returns: The sample missing of both RET and DLRET are removed from dataset. And `NA` are labeled for their respective missing code.
3. Delisting return calculation: We use cum-dividend returns for all the calculation. Delisting return is used when there is no listing return available. When both are avaiable, we use compounded rate of return.
4. Market Capitalization calculation: Since the price in the dataset is at the end of the period, we shall use 1-period lag to calculate the market capitalization. The market capitalization is computed as the product of share price and outstanding shares.
5. Portfolio weights: Here we calculate both equal-weighted and value-weighted return.
6. Sample period: Trying to include as much as possible, we can get dataset from Jan 1926 to Dec 2017.
7. Definition of portfolio weights: For equal-weighted portfolio, we take the arithmetic mean of the monthly return for all available stocks during that period. For value-weighted portfolio, the weight is computed from the weight of lagged market capitalization of respective stock.

### Input Dataset

|  Variable Name |  Variable Type |
|:---:|:---:|
|  PERMNO |  integer  |
|date|Date|
|SHRCD|integer|
|EXCHCD|integer|
|RET|character|
|DLRET|character|
|PRC|numeric|
|SHROUT|integer|

```r
PS1_Q1 <- function(CRSP_Stocks){
  # Computes the lagged stock market capitalization, value-weighted return and equal-weight return.
  #
  # Args:
  #   CRSP_Stocks:  PERMNO | Integer
  #                 date | Date
  #                 SHRCD | Integer
  #                 EXCHCD | Integer
  #                 RET | Factor
  #                 DLRET | Factor
  #                 PRC | Numeric
  #                 SHROUT | Integer
  #
  # Returns:
  #   Monthly_CRSP_Stocks:   Year | Integer
  #                          Month | Integer
  #                          Stock_lag_MV | Numeric
  #                          Stock_Ew_Ret | Numeric
  #                          Stock_Vw_Ret | Numeric
  
  # restricted samples
  universe <- input[(SHRCD %in% c(10, 11)) & (EXCHCD %in% c(1,2,3)),]
  
  # first-round clean
  test <- universe
  test[, date := as.Date(as.character(date), format = "%Y%m%d") ]
  test[, adjprc := abs(PRC)]
  test[, mkc := adjprc * SHROUT]
  test[, adjret := as.numeric(RET)]
  test[, adjdlret := as.numeric(DLRET)]
  test[adjret %in% c(-66.0, -77.0, -88.0, -99.0), adjret := NA]
  test[adjdlret %in% c(-55, -66, -88, -99), adjdlret := NA]
  
  # second-round clean
  cleaned_test <- test[(adjprc > 0) & (SHROUT > 0) & (!is.na(adjret) | !is.na(adjdlret)),]
  cleaned_test[, return := (ifelse(is.na(adjret), adjdlret, ifelse(is.na(adjdlret), adjret, (1+adjret)*(1+adjdlret)-1)))]
  cleaned_test[, year := year(date)]
  cleaned_test[, month := month(date)]
  cleaned_test[, lag_mkc := lag(mkc), by = .(PERMNO)]
  jj <- cleaned_test[!is.na(lag_mkc),]
  jj[, weighted_ret := return * lag_mkc]
  
  # group into year, month
  Stock_lag_MV <- jj[, .(sum(lag_mkc)), by = .(year, month)]
  Stock_Vw_Ret <- jj[, .(sum(weighted_ret)), by = .(year, month)]
  Stock_Ew_Ret <- jj[, .(mean(return)), by = .(year, month)]
  
  # generate output
  result <- merge(Stock_lag_MV, Stock_Vw_Ret, by = c("year", "month"), all = TRUE)
  result <- merge(result, Stock_Ew_Ret, by = c("year", "month"), all = TRUE)
  result[, vwreturn := (V1.y / V1.x)]
  output <- result[,.(year, month, V1.x/1000, V1, vwreturn)]
  colnames(output) <- c('Year', 'Month', 'Stock_lag_MV', 'Stock_Ew_Ret', 'Stock_Vw_Ret')
  return(output)
}
```
### Output Dataset

|Variable Name|Variable Type|Variable Description|
|:---:|:---:|:---:|
|Year|numeric|Year|
|Month|numeric|Month|
|Stock_lag_MV|numeric|Total market value the previous month (in millions)|
|Stock_Ew_Ret|numeric|Equal-weighted returns|
|Stock_Vw_Ret|numeric|Value-weighted returns|

## Part 2
Using the risk-free rate of return from French’s website, report the following moments of the market excess returns for both time series (4 decimal digits): annualized return, annualized volatility, annualized Sharpe ratio, skewness, and excess kurtosis. Annualized values should be calculated geometrically. You should be comparing between July 1926 to December 2017, at a monthly frequency.

### Input Dataset
The input comes with data from Kenneth French's website:

|  Variable Name |  Variable Type |
|:---:|:---:|
|  Year |  numeric  |
|Month|numeric|
|Market_minus_Rf|numeric|
|SMB|numeric|
|HML|numeric|
|Rf|numeric|

The summary statistics are in Table 1 below. Five statistics are reported: annualized mean, annualized standard deviation, annualized sharpe ratio, skewness, and excess kurtosis. In Column 1, statistics for the replicated value-weighted market portfolio of stocks calculated in the previous question are given. In Column 2, statistics for the value-weighted market portfolio of stocks from Ken French’s website are computed.

||Estimated FF Market Excess Return|Actual FF Market Excess Return|
|:---:|:---:|:---:|
|Annualized Mean|0.06719 | 0.06428 |
|Annualized Standard Deviation | 0.1846 | 0.1850|
|Annualized Sharpe Ratio | 0.3638 | 0.3473 |
|Skewness | 0.1676 | 0.1878 |
|Excess Kurtosis | 7.6617 | 7.8814 |

From previous question, we have a times series of value-weighted market returns, namely \\(\{r_t\}_{t=1}^T\\). Let the market return from French’s website be given by \\(\{r_t^F \}_{t=1}^T\\). I compute these statistics as follows:

1. Sample period: Monthly from July 1926 to December 2017.
2. Skewness: Skewness of the monthly return is calculated without annualization or logs using the full sample.
3. Excess Kurtosis: Excess kurtosis of the monthly return is calculated without annualization or logs using the full sample.
4. Annualized Mean: For annualized mean, we first calculate the geometric mean of the monthly return over the period. Then doing annulization using \\(\hat{\mu}_A = (1 + \hat{\mu}_M)^{12} - 1\\)
5. Annualized Standard Deviation: For annualized standard deviation, we first calculate the standard deviation of the monthly return over the period. Then doing annulization using \\(\hat{\sigma}_A = \sqrt{12}\hat{\sigma}_M\\)
6. Sharpe Ratio: Calculated from the ratio of annualized mean over annualized standard deviation.

## Part 3
Report (up to 8 decimal digits) the correlation between your time series and French’s time series, and the maximum absolute difference between the two time series. Is it zero? If not, justify whether the difference is economically negligible or not. What are the reasons a nonzero difference? You should be comparing between July 1926 to December 2017, at a monthly frequency.

Table 2 below shows the time-series correlation between the replicated value-weighted market portfolio of stocks and the value-weighted market portfolio of stocks from Ken French’s website. I also report the maximum difference between the two series.

The sample is restricted between July 1926 and December 2017.

The difference between the replicated portfolio and the one from French’s website is not zero because of different ways in data processing and sample selection. Also Kenneth French's dataset truncate the numbers to 2 digits. As the correlation is very close to 1, the moments of the two time-series are relatively similar. For my replication, the maximum absolute difference is around 1%, it is considered to be too large if economical decisions are to be maded based on my results.

`Correlation: 0.99980530`
`Maximum Absolute Difference: 0.01168131`