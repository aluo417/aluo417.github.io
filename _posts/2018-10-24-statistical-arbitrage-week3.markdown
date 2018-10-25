---
title: Statistical Arbitrage Week 3
description: course notes of statistical arbitrage class
categories:
 - notes
tags: financial, econometrics, arbitrage, statistics
---

> Ideas generation for ALPHA

<!-- more -->

## Definition of Alpha

- Source of expected return over and beyond whtat is explained by beta
- Time-varying \\(\rightarrow (T \times n)\\) matrix of alphas
	- \\(T = \\) number of days
	- \\(n = \\) number of stocks
- Cross-sectionally standardized every day:
	- Mean zero across all (active) stocks
		- Generate long-short signal
	- Variance one across all (active) stocks
		- Normalize volatility across different time point

## Winsorization of Alpha

- Substract robust estimate of the mean(such as mean with shrinkage)
- Divide by the standard diviation
- Every alpha > 3 is brought down to 3
- Every alpha < -3 is brought up to -3

## Some Sources of Alpha

### Value vs. Glamour

#### Some examples:

- Fama & French: B/M ratio
- Basu: E/P ratio
- Kothari & Shanken: D/P ratio
- Lakonishok, Schileifer & Vishny: C/P ratio
- ...
- Long-term Contrarian bucket

#### General Structure of Value Alpha

- <Fundamental> divided by market price
- Not the other way around
- Only market price is guaranteed to be greater than 0

#### Combining Alphas

- Let \\(\alpha_{ti}^1\\) be a standardized winsorized alpha for stock \\(i\\) on date \\(t\\)
- Let \\(\alpha_{ti}^2\\) be another alpha
- Aggregate Alpha as: $$\alpha_{ti} = w_1 \alpha_{ti}^1 + w_2 \alpha_{ti}^2$$
- Winsorize aggregate alpha

Generalization can be made through combining of many flavors of the value alpha to make better aggregate value alpha.

With consensus of analyst forecasts, numerators can also be:

- Analyst forecast of future earnings
- Analyst forecast of future sales
- Analyst forecast of future dividends
- Analyst forecast of future cash flow

The weights for future projections are determined by judgement and backtest.

### Momentum

- Straight momentum
	- Jegadeesh and Titman (1993) “Returns to Buying Winners and Selling Losers: Implications for Stock Market Efficiency” Journal of Finance
	- if high \\(\rightarrow\\) buy the stock
	- if low \\(rightarrow\\) sell the stock
- Echo momentum
	- Novy-Marx (JFE 2012) "Is momentum really momentun"
	- Momentum is primarily driven by firms' performance 12 to 7 months ago
- MomentumConsistency
	- Grinblatt and Moskowitz (JFE 2004) "Predicting stock price movements from past returns: the role of consistency and tax-loss selling"
	- Achieving a high past return with a series of steady positive months generates larger expected return than a high past return achieved with just a few extraordinary months
	- One-year winner (loser) consistency dummy is one if the monthly return of the stock was positive (negative) in at least eight of the one-year horizon’s 11 months
- Industry Momentum
	- Moskowitz and Grinblatt (JF 1999) "Do industries explain momentum?"
		- Industry momentum strategies are more profitable than individual stock momentum strategies.
		- Industry momentum strategies are robust to various specifications and methodologies, and they appear to be profitable even among the largest, most liquid stocks.
	- Industry portfolios exhibit significant momentum, even after controlling for size, book-to-market equity (BE/ME), individual stock momentum, the cross-sectional dispersion in mean returns, and potential microstructure influences.
	- Once returns are adjusted for industry effects, momentum profits from individual equities are significantly weaker and, for the most part, are statistically insignificant.
- Style Momentum
	- Chen and De Bondt (JEF 2004) "Style of momentum within the S&P-500 index"
	- | Cap\Type | Growth| Blend| Value|
	  |:---:|:---:|:---:|:---:|
	  | Small-cap|  | | |
	  | Mid-cap   |   |    |    |
	  |Large-Cap|  |  |  |
	- No-dividend style
- Momentum Acceleration
	- Gettleman and Marks (2006) "Acceleration Strategies"
	- Acceleration is defined as the change in six- month momentum relative to the cross- section of other firms.
	- Trading strategies based on acceleration offer significant abnormal profits of approximately 4.5% annually when controlled for other known regularities in equity returns.

### Volume

- Gervais, Kaniel and Mingelgrin (2001) “The high-volume return premium” Journal of Finance
	- Stocks experiencing unusually high (low) volume over a day or a week tend to appreciate (depreciate) over the next month
- Chordia, Subrahmanyam & Anshuman (JFE 2001) “Trading activity and expected stock returns”
	- If stocks had high volume 2 months ago then they tend to go down this month

### New issues puzzle

- Loughran and Ritter (JF 1995) “The New Issues
Puzzle”
- Companies that issue stock subsequently
underperform over the next 5 years
- Worse for IPO’s than for Seasoned Equity Offerings
- Cannot winsorize

### Post-earnings announcement drift

- Bernard and Thomas (JAR 1989) “Post-Earnings- Announcement Drift: Delayed Price Response or Risk Premium?”
- Stocks whose earnings are above (below) analyst consensus outperform (underperform) for two months afterwards
- Do not winsorize.

## Alpha Aggregation

- Short-term alphas balance long-term alphas
- Technical alphas balance fundamental alphas
- Contrarian alphas balance momentum alphas

|   | Short-Term  | Long-Term  |
|:---:|:---:|:---:|
|  CONTRARIAN | 1-month reversal of residuals, MAX factor, idiosyncratic skewness  | value , betting against beta (variance) |
| PRO-CYCLICAL  |  1-month trading volume, post-earning announcement drift, insider trading, implied volatility | momentum, profitability , disaggregation quality, short interest |

## Alphas Everywhere

Below are some most important factors for predicting the corss-section of US stock returns

| Factor  | Definition  |
|:-:|:-:|
| Residual Return  |  last month’s residual stock return unexplained by the market |
| Cash Flow-to-Price  |  12-month trailing cash flow-per-share divided by current price |
| Earnings-to-Price  |  12-month trailing earnings-per-share divided by current price |
| Return on Assets  |  12-month trailing total income divided by most recently reported total assets |
| Residual Risk  |  24-month trailing variance of residual stock return unexplained by market return |
| 12-month Return  | total return for the stock over trailing twelve months |
| Return on Equity  | 12-month trailing earnings-per-share divided by most recently reported book value-per-share  |
| Variance  |  24-month trailing variance of total stock return  |
| Book-to-Price  |  most recently reported book value of equity divided by current market price |
| Profit Margin  | 12-month trailing earnings before interest divided by 12-month trailing sales  |
| 3-month Return  |  total return for the stock over trailing 3 months |
| Sales-to-Price  | 12-month trailing sales-per- share divided by market price  |
