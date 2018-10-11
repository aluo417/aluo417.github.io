---
title: Application of Monte Carlo Simulation
description: This project is focused on the the application of Monte Carlo Simulations to solve statistical problems in financial engineering.
categories:
 - project
tags: Python
---

> The most common use of Monte Carlo Simulation in finance is when one needs to calculate the expected value of a functional $$\mathbb{E} f(X)$$

<!-- more -->

## Introduction & Discussion

### Data Set

The WRDS Compustat data for all stocks contains the fundamental informations including, cusip code for each ticker, fiscal year, total asset, average short term debt, long term debt, EBIT, EBITDA and cash flows. Then, the leverage ratio is created by the ratio of total debt over total asset. The data is collected from 1973 through 2015.

### Data Processing

We notice that due to financial report format, some cash flows items may be missing as the company report working capital instead. Since it is hard to collect precise short-term debt numbers, some of the short-term debt data are missing. The fiscal years with missing values of leverage ratio and cash flows are retained.

### Methodology

R Package “data.table” is used to process the data. The data visualization is accomplished through ggplot2 and htmlTable.

### Performance Analysis

For illustration purpose, we draw two stocks out from the stock universe that has more than 20-year history. We choose cash flows of financial activity, investment activity and operation activity to better represent the operating status of the company. Also, both EBITA and Basic EPS Excluding Extraordinary Items are used as representatitves for earnings.

And the showcase of the annual data can be seen in the figures below. The Rdata for all stocks is stored online and you can access it from [HERE](https://drive.google.com/open?id=1r4DB9YSqYHzpy-8OpAEm0RT5kjYxqdeq).

## Figures

* Fundamental Information of Selected Stock 1
![Selected Stock 1](/assets/images/all-stock-dataset/blog1.png)

* Fundamental Information of Selected Stock 1
![Selected Stock 1](/assets/images/all-stock-dataset/blog2.png)

* Fundamental Information of Selected Stock 2
![Selected Stock 2](/assets/images/all-stock-dataset/blog3.png)

* Fundamental Information of Selected Stock 2
![Selected Stock 2](/assets/images/all-stock-dataset/blog4.png)

## R Code

### Load the dependencies

```sh
# loading libraries
  library(zoo)
  library(magrittr)
  library(data.table)
  library(lubridate)
```

### Load raw data

```sh
  dset_data <- fread("./fundamentals.csv")
  colnames(dset_data) %<>% tolower
```

### Process the data

groupby the CUSIP and financial year, and calculate the ratios correspongding to that year.

```sh  
  # read from raw data
  finlset <- dset_data[, 
              .(((bast + dltt) / at), (fincf), (ivncf), (oancf), (ebitda), (epspx)),
              by = list(cusip, fyear)]
  colnames(finlset) <- c('cusip', 'fyear', 'leverage', 'fincf', 'ivncf', 'oancf', 'ebitda', 'epsbasic')
  save(finlset, file="./finlset.RData")
```