---
title: cashflow-to-price-ratio
description: characteristic study, behavioral finance
category: Photo
photos:
- \assets\images\cfp\monthly_ls_cp_rec.png
categories:
 - academic project
tags: 
---

> Short-term and long-term performance of C/P strategy

<!-- more -->

## Cashflow-to-Price
In this project, I replicate Cashflow-to-Price as one of the set of stock market anomalies discussed in class for the period 1985 to 2017. I use a WRDS account along with Python and R to process data and implement the portfolios. The project has two stages, the first is to replicate the main result from “Contrarian Investment, Extrapolation, and Risk”" (Lakonishok, Shleifer and Vishny 1994). Secondly, I sorting stocks into decile portfolios by the Cashflow-to-Price and Book Value-to-Market in every month t and calculating average returns on these portfolios at time t + 1 and computing the returns that are long-short the extreme deciles and calculating the t-stat for that average long-short return.

## Dataset
The WRDS Compustat data and CRSP data for all stocks contains the fundamental and trading informations we need. To use both datasets, we need to link them by matching the CCM-LinkTable provided by WRDS. Then, the book-to-market ratio (B/M) and cashflow-to-price ratio (C/P) are created according to the formula below. The data is collected from 1959 through 2017. Besides, to supplement the lack of Book Equity in early years of the time period, I use Davis’s Book Value Dataset.

$$
\begin{aligned}
B/M &= \frac{\text{Stockholders' Equity}}{\text{Price}\times \text{Number of Outstanding Shares}}\\
CF/P &= \frac{\text{Net Operating Cash Flow}}{\text{Price}\times \text{Number of Outstanding Shares}}
\end{aligned}
$$

### Compustat Data Fetching
We notice that due to financial report format, some items may be missing in the record. Therefore, following the Fama & French's methodology and WRDS methodology, we can calculate the alternatives as our desired values. Therefore, alternative values in the financial statements are queried as well.

### CRSP Data Fetching
The stock universe is defined as common stocks (share codes 10 and 11) listed in New York Stock Echange, American Stock Exchange, or the Nasdaq Stock Exchange (exchange codes 1, 2, and 3) from year 1959 through year 2017.

## Data Processing
To form decile portfolio based on lag variable, we calcualte one-month lag varaibles corresponding to C/P and B/M ratios. According to `LSV 1994`, the portfolios are formed at the end of April each year. Then we subset the period between April 1968 to March 1989 as defined in the paper. Finally the datasets are saved for further portfolio construction in `Python`.

## Performance Analysis
This section composes of two sub-section. First we shall replicate the result in the LSV 1994 paper and extend the results into recent 32 years, then we examine the monthly long-short decile portfolio to see if C/P anomalies significant in different periods.

### Replication
As defined in LSV 1994, $R_t$ is the average return in year $t$ after formation. $AR$ is the average annual return over 5 postformation years. $CR_5$ is the compounded 5-year return assuming annual rebalancing.

#### Panel A

|    |     1|     2|     3|     4|     5|     6|     7|     8|     9|    10|
|:---|-----:|-----:|-----:|-----:|-----:|-----:|-----:|-----:|-----:|-----:|
|R1  | 0.055| 0.084| 0.119| 0.136| 0.142| 0.149| 0.167| 0.180| 0.185| 0.197|
|R2  | 0.040| 0.094| 0.103| 0.121| 0.144| 0.146| 0.156| 0.173| 0.180| 0.176|
|R3  | 0.060| 0.110| 0.132| 0.158| 0.150| 0.155| 0.169| 0.196| 0.204| 0.200|
|R4  | 0.069| 0.110| 0.134| 0.160| 0.161| 0.160| 0.186| 0.189| 0.202| 0.198|
|R5  | 0.097| 0.135| 0.154| 0.161| 0.173| 0.169| 0.188| 0.185| 0.187| 0.186|
|AR  | 0.064| 0.107| 0.128| 0.147| 0.154| 0.156| 0.173| 0.185| 0.191| 0.191|
|CR5 | 0.379| 0.628| 0.878| 1.031| 1.049| 1.170| 1.261| 1.455| 1.459| 1.592|

#### Panel B

|    |     1|     2|     3|     4|     5|     6|     7|     8|     9|    10|
|:---|-----:|-----:|-----:|-----:|-----:|-----:|-----:|-----:|-----:|-----:|
|R1  | 0.085| 0.127| 0.128| 0.161| 0.165| 0.172| 0.177| 0.169| 0.182| 0.184|
|R2  | 0.085| 0.116| 0.120| 0.136| 0.155| 0.165| 0.154| 0.168| 0.186| 0.193|
|R3  | 0.097| 0.125| 0.156| 0.151| 0.168| 0.172| 0.180| 0.195| 0.191| 0.210|
|R4  | 0.123| 0.126| 0.151| 0.155| 0.164| 0.182| 0.174| 0.191| 0.200| 0.204|
|R5  | 0.132| 0.147| 0.163| 0.169| 0.165| 0.172| 0.179| 0.177| 0.198| 0.190|
|AR  | 0.104| 0.128| 0.143| 0.154| 0.163| 0.173| 0.173| 0.180| 0.191| 0.196|
|CR5 | 0.581| 0.938| 0.906| 1.220| 1.290| 1.357| 1.379| 1.364| 1.444| 1.404|

In addition, following LSV 1994 methodology, I separate the firms having negative cashflow-to-price ratio as extra decile `0`. Let's take a look at their performance as a portfolio.

* Decile Portfolio Size in Replication Period
![decile_rep](\assets\images\cfp\decile_rep.png)

As we see above, by excluding the negative C/P stocks in our portfolio, the stock universe loses 1/3 of its size. Then we track the performance of this decile portfolio over the period. We observe some extremely high return and average annual return of 10% over 1968-1989.

* Annual Return of Negative C/P Portfolio
![annual_ret_rep](\assets\images\cfp\annual_ret_rep.png)

#### Monthly Decile Long-Short Portfolio
To examine the short-term performance of the C/P strategy, we construct monthly long-short strategies based on B/M and C/P sorting.

* Monthly Return of B/M Long-Short Portfolio
![monthly_ls_rep](\assets\images\cfp\monthly_ls_rep.png)

One Sample t-test Result
Alternative Hypothesis: true mean is not equal to 0

t = 4.9883, df = 251, p-value = 1.139e-06

95 percent confidence interval: [0.007612749, 0.017545834]

sample estimates: mean of $x = 0.01257929$

In comparison to B/M strategy, monthly C/P strategy performance:

* Monthly Return of C/P Long-Short Portfolio
![monthly_ls_cp_rep](\assets\images\cfp\monthly_ls_cp_rep.png)

One Sample t-test
Alternative Hypothesis: true mean is not equal to 0

t = 5.8377, df = 251, p-value = 1.63e-08

95 percent confidence interval: [0.007099405, 0.014328576]

sample estimates: mean of $x = 0.01071399$

### Extension to Recent Period
Following the methodology, I calculate the metrics for the portfolios from year 1985 to year 2017.
As defined in LSV 1994, $R_t$ is the average return in year $t$ after formation. $AR$ is the average annual return over 5 postformation years. $CR_5$ is the compounded 5-year return assuming annual rebalancing.

#### Panel A

|    |     1|     2|     3|     4|     5|     6|     7|     8|     9|    10|
|:---|-----:|-----:|-----:|-----:|-----:|-----:|-----:|-----:|-----:|-----:|
|R1  | 0.091| 0.116| 0.150| 0.157| 0.154| 0.168| 0.165| 0.165| 0.184| 0.207|
|R2  | 0.108| 0.143| 0.139| 0.149| 0.166| 0.161| 0.161| 0.148| 0.167| 0.204|
|R3  | 0.122| 0.150| 0.146| 0.141| 0.152| 0.149| 0.145| 0.164| 0.174| 0.183|
|R4  | 0.138| 0.158| 0.146| 0.155| 0.150| 0.169| 0.165| 0.160| 0.196| 0.188|
|R5  | 0.150| 0.151| 0.170| 0.161| 0.152| 0.160| 0.153| 0.174| 0.177| 0.186|
|AR  | 0.122| 0.144| 0.150| 0.153| 0.155| 0.161| 0.158| 0.162| 0.180| 0.194|
|CR5 | 0.345| 0.593| 0.849| 0.925| 0.924| 1.030| 1.019| 1.033| 1.183| 1.272|

#### Panel B

|    |     1|     2|     3|     4|     5|     6|     7|     8|     9|    10|
|:---|-----:|-----:|-----:|-----:|-----:|-----:|-----:|-----:|-----:|-----:|
|R1  | 0.139| 0.145| 0.157| 0.161| 0.154| 0.162| 0.154| 0.175| 0.187| 0.212|
|R2  | 0.152| 0.144| 0.132| 0.148| 0.151| 0.153| 0.160| 0.175| 0.179| 0.210|
|R3  | 0.159| 0.150| 0.162| 0.142| 0.156| 0.144| 0.155| 0.164| 0.169| 0.199|
|R4  | 0.195| 0.165| 0.159| 0.153| 0.147| 0.163| 0.164| 0.168| 0.180| 0.198|
|R5  | 0.168| 0.192| 0.145| 0.165| 0.168| 0.159| 0.170| 0.150| 0.191| 0.197|
|AR  | 0.162| 0.159| 0.151| 0.154| 0.155| 0.156| 0.161| 0.166| 0.181| 0.203|
|CR5 | 0.728| 0.772| 0.909| 0.985| 0.950| 0.972| 0.885| 1.103| 1.213| 1.386|

Similarly, I separate the firms having negative cashflow-to-price ratio as extra decile `0`. Let's take a look at their performance as a portfolio.

* Decile Portfolio Size in Recent Period
![decile_rec](\assets\images\cfp\decile_rec.png)

Then we track the performance of this decile portfolio over the period. We observe some extremely high return and average annual return of 10% over 1985-2017.

* Annual Return of Negative C/P Portfolio in Recent Period
![annual_ret_rec](\assets\images\cfp\negative_rec.png)

#### Monthly Decile Long-Short Portfolio

* Monthly Return of B/M Long-Short Portfolio in Recent Period
![monthly_ls_rec](\assets\images\cfp\monthly_ls_rec.png)

One Sample t-test
Alternative Hypothesis: true mean is not equal to 0

t = 5.396, df = 383, p-value = 1.197e-07

95 percent confidence interval: [0.00705787, 0.01514992]

Sample estimates: mean of $x = 0.01110389$

In comparison to B/M strategy, monthly C/P strategy performance:

* Monthly Return of C/P Long-Short Portfolio in Recent Period
![monthly_ls_cp_rec](\assets\images\cfp\monthly_ls_cp_rec.png)

One Sample t-test
alternative hypothesis: true mean is not equal to 0

t = 4.6553, df = 383, p-value = 4.466e-06

95 percent confidence interval: [0.005571137, 0.013717897]

Sample estimates: mean of $x = 0.009644517$

## Conclusion
In this article, I examine the conclusion from LSV 1994 paper by replicating the portfolio returns over the same period. To check if the anomalies still exist in recent period, I construct portfolios following same methodlogy. Given evidence in previous section, high C/P stocks are identified as value stocks in both short-run and long-run because their decile returns are following increasing order and the decile long-short portfolio has significant positive return.

## Code for Portfolio Construction

### R Code for Compustat Data Fetch

```r
# connect to WRDS api
wrds <- dbConnect(Postgres(), 
                  host='wrds-pgdata.wharton.upenn.edu',
                  port=9737,
                  user="user",
                  password='password',
                  sslmode='require',
                  dbname='wrds')

# query Compustat fundamental
res <- dbSendQuery(wrds,"select gvkey, datadate, oancf, ib, act, che, lct, dlc, txp, dp, \
                    at, lt, pstkl, txditc, mib, pstkrv, pstk, seq, ceq, txdb, itcb \
                   from comp.funda \
                   where indfmt='INDL' \
                   and datafmt='STD' \
                   and popsrc='D' \
                   and consol='C' \
                   and datadate >= '01/01/1959'")

data.comp.funda <- dbFetch(res, n = -1) # n=-1 denotes no max but retrieve all record
dbClearResult(res)
save(data.comp.funda, file = "./Data/data.comp.funda.RData")

# query ccmlink
res <- dbSendQuery(wrds, "select gvkey, lpermno as permno, linktype, linkprim, \
                   linkdt, linkenddt \
                   from crsp.ccmxpf_linktable \
                   where substr(linktype,1,1)='L' \
                   and (linkprim ='C' or linkprim='P')")
data.ccmlink <- dbFetch(res, n = -1) 
dbClearResult(res)
save(data.ccmlink, file = "./Data/data.ccmlink.RData")

# merge linked permno onto Compustat dataset
# Merge the linked permno onto Compustat dataset
data.ccm <-  data.ccmlink %>%
  # use only primary links (from WRDS Merged Compustat/CRSP examples)
  filter(linktype == "LU" | linktype =="LC" | linktype =="LS") %>%
  merge(data.comp.funda, by.x="gvkey", by.y="gvkey") %>%  
  # inner join, keep only if permno exists
  mutate(datadate = as.Date(datadate), 
         permno = as.factor(permno),
         linkdt = as.Date(linkdt),
         linkenddt = as.Date(linkenddt),
         linktype = factor(linktype, levels=c("LC", "LU", "LS")),
         linkprim = factor(linkprim, levels=c("P", "C"))) %>%
  # bonce the date within the linked period range
  filter(datadate >= linkdt & (datadate <= linkenddt | is.na(linkenddt))) %>%
  arrange(datadate, permno, linktype, linkprim) %>%
  distinct(datadate, permno, .keep_all = T) %>%
  data.table
save(data.ccm, file = "./Data/data.ccm.RData")
rm(data.comp.funda, data.ccmlink) # release memory

data.comp <- data.ccm %>%
  group_by(permno) %>%
  mutate(datadate = as.yearmon(datadate)) %>%
  arrange(datadate, permno) %>%
  distinct(datadate, permno, .keep_all = T)
save(data.comp, file = "./Data/data.comp.RData")
```

### R Code for CRSP Data Fetch

```r
# query CRSP stock data
res <- dbSendQuery(wrds, "select a.permno, a.permco, a.date, b.shrcd, b.exchcd,
                   a.ret, a.retx, a.shrout, a.prc
                   from crsp.msf as a
                   left join crsp.msenames as b
                   on a.permno=b.permno
                   and b.namedt<=a.date
                   and a.date<=b.nameendt
                   where a.date between '01/01/1959' and '12/31/2017'
                   and b.exchcd between 1 and 3
                   and b.shrcd between 10 and 11")
crsp.m <- dbFetch(res, n = -1) 
dbClearResult(res)

res <- dbSendQuery(wrds, "select permno, dlstdt, dlret
                   from crspq.msedelist")
crsp.msedelist <- dbFetch(res, n = -1)
dbClearResult(res)

# unify the date
crsp.m %<>%
  data.table %>%
  mutate(date = as.yearmon(date))
crsp.msedelist %<>%
  data.table %>%
  mutate(date = as.yearmon(dlstdt)) %>%
  select(-dlstdt)

# merge to get whole crsp
data.crsp <- crsp.m %>%
  merge(crsp.msedelist, by = c("date", "permno"), all=T, allow.cartesian=T) %>%
  mutate(permno = as.factor(permno), permco = as.factor(permco), shrcd = as.factor(shrcd), 
  exchcd = as.factor(exchcd)) %>%
  mutate(retadj = ifelse(!is.na(ret), ifelse(!is.na(dlret), 
  (1+ret)*(1+dlret)-1, ret), ifelse(!is.na(dlret), dlret, NA))) %>%
  arrange(date, permno) %>%
  group_by(permno) %>%
  mutate(shrcd = na.locf(shrcd, na.rm=FALSE), exchcd = na.locf(exchcd, na.rm=FALSE))

# calculate me and sum up subsidiary permco
data.crsp %<>%
  mutate(meq = shrout * abs(prc)) %>% # me for each permno
  group_by(date, permco) %>%
  mutate(ME = sum(meq)) %>% # to calc market cap, merge permnos with same permco
  arrange(date, permco, desc(meq)) %>%
  group_by(date, permco) %>%
  slice(1) %>% # keep only permno with largest meq
  ungroup
rm(crsp.m, crsp.msedelist) # release memory
```

### R Code for Data Processing

```r
# get first non-NA
coalesce <- function(...) {
  # CF <- coalesce(oancf, ib-sum(diff_act,-diff_che,-diff_lct,diff_dlc,diff_txp,-dp))
  Reduce(function(x,y) {
    i<-which(is.na(x))
    x[i] <- if(length(y)==1) {y} else{y[i]}
    x
  },
  list(...))
}

data.comp.a <- data.comp %>%
  group_by(permno) %>%
  mutate(diff_act = act-lag(act), diff_che=che-lag(che), 
  diff_lct=lct-lag(lct), diff_dlc=dlc-lag(dlc), diff_txp=txp-lag(txp)) %>%
  mutate(CF = coalesce(oancf,
                       ib-(diff_act-diff_che-diff_lct+diff_dlc+diff_txp-dp),
                       ib-(diff_act-diff_che-diff_lct+diff_dlc-dp),
                       ib-(diff_act-diff_che-dp)),# consistent w/ WRDS definition
         BE = coalesce(seq, ceq + pstk, at - lt - mib, at - lt) + 
           coalesce(txditc, txdb + itcb, 0) - 
           coalesce(pstkrv, pstkl, pstk, 0) 
           # consistent w/ French website variable definitions
  ) %>%
  arrange(datadate, permno) %>%
  select(datadate, permno, CF, BE)
save(data.comp.a, file="./Data/data.comp.a.RData")
rm(data.ccm, data.comp) # release memory

# CRSP cleaning

Fill_TS_NAs <- function(main) {
  # takes datatable with date and permno as columns and fills in NAs where there are gaps
  # make date range continuous
  core <- main[,.(date, permno)]
  # find first and last dates
  date.bookends <- core %>%
    group_by(permno) %>%
    summarize(first = first(date), last = last(date))
  
  # generate all dates for all permnos then trim those outside the range
  output <- core %>%
    cbind(., 1) %>% # create 3rd column so spread can be applied
    spread(., permno, V2) %>%
    gather(., permno, V2, -date) %>%
    merge(date.bookends, by="permno", all.x=T) %>%
    group_by(permno) %>%
    filter(date >= first & date <= last) %>%
    select(date, permno) %>%
    merge(., main, by=c("date", "permno"), all.x=T)
  
  return(output)
}

#  calculate size
data.crsp.a <- data.crsp %>%
  select(date, permno, shrcd, exchcd, shrout, prc, retadj, ME) %>%
  mutate(ME = ME/1000) %>%  # convert from thousands to millions
  filter((shrcd %in% c(10, 11)) & (exchcd %in% c(1, 2, 3))) %>%
  Fill_TS_NAs %>% # fill in gap dates within each permno
  mutate(permno = as.factor(permno)) %>%
  group_by(permno) %>%
  mutate(port.weight = as.numeric(ifelse(!is.na(lag(ME)), lag(ME), ME/(1+retadj))), 
  # calc portweight as ME at beginning of period
         port.weight = ifelse(is.na(retadj) & is.na(prc), NA, port.weight)) %>% 
         # remove portweights calc for date gaps
  ungroup %>% 
  rename(retadj.1mn = retadj) %>%
  arrange(date, permno) %>%
  distinct(date, permno, .keep_all = T) # remove duplicates
save(data.crsp.a, file = "./Data/data.crsp.a.RData")
rm(data.crsp) # release memory

# merge with Davis Book Equity data
data.Davis.bkeq <- read.csv("./Data/Davis Book Equity.csv")
# obtained from Ken French website
data.Davis.bkeq[data.Davis.bkeq == -999 | data.Davis.bkeq == -99.99] <- NA
data.Davis.bkeq <- data.Davis.bkeq %>%
  mutate(permno = factor(PERMNO)) %>% # rename
  data.table %>%
  select(-FirstYr, -LastYr) %>%
  gather(Date, Davis.bkeq, -permno, na.rm=TRUE) %>%
  mutate(date = as.yearmon(ymd(paste0(substr(Date, 2, 5),"-6-01")))) %>%
  select(-Date)

 # propagate to previous n rows
na_locf_until = function(x, n) {
  # in time series data, fill in na's untill indicated n
  l <- cumsum(! is.na(x))
  c(NA, x[! is.na(x)])[replace(l, ave(l, l, FUN=seq_along) > (n+1), 0) + 1]
}

# merge and keep the continuity by filling NAs
data.both.m <- data.comp.a %>%  
  mutate(date = datadate + (16-month(datadate))/12) %>% 
  # map to next year April period when data is known
  merge(data.crsp.a, ., by=c("date", "permno"), all.x=T, allow.cartesian=T) %>%
  merge(data.Davis.bkeq, by=c("date", "permno"), all.x=T, allow.cartesian=T) %>%
  arrange(permno, date, desc(datadate)) %>%
  distinct(permno, date, .keep_all = T) %>%
  group_by(permno) %>%
  # fill in Compustat and Davis data NA's with latest available for subsequent year
  # filling max of 11 previous months means gaps may appear when fiscal year end changes
  mutate(Davis.bkeq = na_locf_until(Davis.bkeq, 11)) %>%
  ungroup %>% 
  mutate(datadate = yearmon(datadate)) %>%
  arrange(date, permno)
save(data.both.m, file = "./Data/data.both.m.RData") 
rm(data.comp.a, data.crsp.a, data.Davis.bkeq) # release memory
```

### R Code for Sorting Preparation

```r
# add lag values for sorting
data.both.FF.m <- data.both.m %>%
  group_by(permno) %>%
  mutate(BE = coalesce(BE, Davis.bkeq), 
  # data available by end-of-Jun based on Compustat Date mapping 
         ME.Dec = as.numeric(ifelse(month(date)==4 & lag(ME,6)>0, lag(ME,6), NA)), 
         # set value to previous Dec ME 
         ME.Apr = as.numeric(ifelse(month(date)==4, ME, NA)),
         CFP = as.numeric(ifelse(month(date)==4 & ME.Dec>0, CF/ME.Dec, NA)),
         BM.FF = as.numeric(ifelse(month(date)==4 & ME.Dec>0, BE/ME.Dec, NA)), 
         # as defined by FF
         lag.ME.Apr = lag(ME.Apr), 
         # monthly data so only lag by 1 month
         lag.BM.FF = lag(BM.FF),
         lag.CFP = lag(CFP)
         )

data.both.FF.m %<>%
  mutate_all(funs(ifelse(!is.infinite(.), ., NA))) %>% 
  # code Inf values as NAs
  select(date, datadate, permno, exchcd, prc, retadj.1mn, ME, port.weight, ME.Apr:lag.CFP) %>%
  arrange(date, permno) %>%
  group_by(permno) %>% 
  # propagate to entire fyear
  mutate(ME.Apr = na_locf_until(ME.Apr, 11), 
         BM.FF = na_locf_until(BM.FF, 11),
         CFP = na_locf_until(CFP, 11),
         lag.ME.Apr = na_locf_until(lag.ME.Apr, 11), 
         lag.BM.FF = na_locf_until(lag.BM.FF, 11),
         lag.CFP = na_locf_until(lag.CFP, 11)) %>%
  ungroup %>%
  mutate(port.weight = ifelse(is.na(port.weight), 0, port.weight)) 
  # necessary to avoid NAs for weighted ret calc
  mutate(date=yearmon(date))

# replicated period
replicated_range <- data.both.FF.m %>%
filter(year(date)>=1968 & year(date) <= 1989) 
write.csv(replicated_range, './replicated_range.csv')

# latest period
latest_range <- data.both.FF.m %>%
filter(year(date)>=1985 & year(date)<=2017)
write.csv(latest_range, './latest_range.csv')
```

### Python Code for Portfolio Construction

```python
import pandas as pd
import numpy as np
import datetime as dt
# read in corresponding data
df = pd.read_csv('./replicated_range.csv', index_col=0)

# interested data
df1 = df[['date', 'permno', 'retadj.1mn', 'lag.BM.FF', 'lag.CFP']].copy()

# transform date and create portfolio formation period
df1['date'] = df1['date'].apply(lambda x: dt.datetime.strptime('1 '+x, '%d %b %Y'))
df1['month'] = df1['date'].apply(lambda x: x.month)
df1['year'] = df1['date'].apply(lambda x: x.year)
df1['formed'] = ((df1['year']-1968)*12 + df1['month']-4)//12

# sample period
df_formation = df1[(df1['month']==4) & (df1['formed'] >= 0) & (df1['formed'] < 21)].copy()
# add label for buy-and-hold annual strategy
df_formation['BM_bin_a'] = df_formation.groupby('formed')
.apply(lambda x: pd.qcut(x['lag.BM.FF'],10, labels=range(10))).values

def split_CFP_pos_neg_and_rank(series):
    pos = list(filter(lambda x: x>=0, series))
    ranks = pd.qcut(pos, 10, labels=range(10))
    res = []
    for elem in series:
        if pd.isnull(elem):
            res.append(elem)
        elif elem < 0:
            res.append(-1)
        else:
            ind = pos.index(elem)
            rank = ranks[ind]
            res.append(rank)
    return res
        
df_formation['CFP_bin_a'] = np.concatenate(df_formation.groupby('formed')
.apply(lambda x: split_CFP_pos_neg_and_rank(x['lag.CFP'])).values)

# join the labeled to original
df_decile = df1.set_index(['date', 'permno'])
.join(df_formation[['date', 'permno', 'BM_bin_a', 'CFP_bin_a']]
.set_index(['date', 'permno']), how='left')
df_decile = df_decile[(df_decile['formed'] >= 0) & (df_decile['formed'] < 21)]

# fill the annual label to each month
df_horizon['BM_bin_a'] = df_horizon.groupby(['formed',
'permno'])['BM_bin_a'].fillna(method='ffill')
df_horizon['CFP_bin_a'] = df_horizon.groupby(['formed',
'permno'])['CFP_bin_a'].fillna(method='ffill')

# label each month for monthly rebalance portfolio
df_horizon['BM_bin_m'] = df_horizon.groupby('date')
.apply(lambda x: pd.qcut(x['lag.BM.FF'],10, labels=range(10))).values
df_horizon['CFP_bin_m'] = np.concatenate(df_horizon.groupby('date')
.apply(lambda x: split_CFP_pos_neg_and_rank(x['lag.CFP'])).values)


# calculate portfolio returns
def Rt_decile_avg_return(df, portfolio, t):
    port_year_t = []
    horizon = len(set(df['formed']))
    for bin_num in range(10):
        port_year_t.append(
        np.mean(list(map(lambda x: df.loc[(df['permno'].isin(portfolio[bin_num][x])) & 
                                      (df['formed']==(x+t-1))]
                                      .groupby('permno')
                                      .apply(lambda x: np.prod(1+x['retadj.1mn'])-1)
                                      .mean(), range(horizon-t+1)))))
    return np.asarray(port_year_t) # annualize

## B/M portfolio returns
BM_portfolio = df_horizon[df_horizon['month']==4]
.groupby(['BM_bin_a', 'formed']).apply(lambda x: x['permno'])
BM_year_1, BM_year_2, BM_year_3, BM_year_4, BM_year_5 = 
map(lambda x: Rt_decile_avg_return(df_horizon, BM_portfolio, x), range(1, 6))
BM_AR = np.vstack([BM_year_1, BM_year_2, BM_year_3, BM_year_4, BM_year_5]).mean(axis=0)
BM_CR_5 = df_horizon.groupby(['BM_bin_a', 'formed', 'permno'], as_index=False)['retadj.1mn']
.apply(lambda x: np.prod(1+x) - 1).reset_index()\
.groupby(['BM_bin_a', 'formed'], as_index=False)[0].mean()\
.groupby('BM_bin_a', as_index=False).rolling(window=5).apply(lambda x: np.prod(x+1)-1)\
.groupby('BM_bin_a').mean()[0]

## CF/P portfolio returns
CFP_portfolio = df_horizon[df_horizon['month']==4]
.groupby(['CFP_bin_a', 'formed']).apply(lambda x: x['permno'])
CFP_year_1, CFP_year_2, CFP_year_3, CFP_year_4, CFP_year_5 = 
map(lambda x: Rt_decile_avg_return(df_horizon, CFP_portfolio, x), range(1, 6))
CFP_AR = np.vstack([CFP_year_1, CFP_year_2, CFP_year_3, CFP_year_4, CFP_year_5]).mean(axis=0)
CFP_CR_5 = df_horizon[df_horizon['CFP_bin_a']!=-1]
.groupby(['CFP_bin_a', 'formed', 'permno'], as_index=False)['retadj.1mn']
.apply(lambda x: np.prod(1+x) - 1).reset_index()\
.groupby(['CFP_bin_a', 'formed'], as_index=False)[0].mean()\
.groupby('CFP_bin_a', as_index=False).rolling(window=5).apply(lambda x: np.prod(x+1)-1)\
.groupby('CFP_bin_a').mean()[0]
```
