---
title: Stocks Fundamental Data
description: This project is focused on the collection of annual fundamental data for all stocks.
categories:
 - project
tags: R
---

> The data sets come from [WRDS](https://wrds-web.wharton.upenn.edu/wrds/). The annual data for earnings, cash flows, and leverage ratios of corresponding fiscal years are collected. Measurement error due to sample selection, and processing of missing data for stocks with short history may affect the conclusion.

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
![Selected Stock 1](/assets/images/blog1.png)

## R Code

```sh
# loading libraries
  library(zoo)
  library(magrittr)
  library(data.table)
  library(lubridate)
  
  # load raw data
  dset_data <- fread("./fundamentals.csv")
  colnames(dset_data) %<>% tolower
  
  # read from raw data
  finlset <- dset_data[, 
              .(((bast + dltt) / at), (fincf), (ivncf), (oancf), (ebitda), (epspx)),
              by = list(cusip, fyear)]
  colnames(finlset) <- c('cusip', 'fyear', 'leverage', 'fincf', 'ivncf', 'oancf', 'ebitda', 'epsbasic')
  save(finlset, file="./finlset.RData")
```

## Screenshots

* Desktop
![Desktop Preview](http://iissnan.com/nexus/next/desktop-preview.png)

* Sidebar

![Desktop Sidebar Preview](http://iissnan.com/nexus/next/desktop-sidebar-preview.png)

* Sidebar (Post details page)

![Desktop Sidebar Preview](http://iissnan.com/nexus/next/desktop-sidebar-toc.png)

* Mobile

![Mobile Preview](http://iissnan.com/nexus/next/mobile.png)


## Installation

Check whether you have `Ruby 2.1.0` or higher installed:

```sh
ruby --version
```

Install `Bundler`:

```sh
gem install bundler
```

Clone Jacman theme:

```sh
git clone https://github.com/Simpleyyt/jekyll-theme-next.git
cd jekyll-theme-next
```

Install Jekyll and other dependencies from the GitHub Pages gem:

```sh
bundle install
```

Run your Jekyll site locally:

```sh
bundle exec jekyll server
```

More Details：[Setting up your GitHub Pages site locally with Jekyll](https://help.github.com/articles/setting-up-your-github-pages-site-locally-with-jekyll/)


## Features

### Multiple languages support, including: English / Russian / French / German / Simplified Chinese / Traditional Chinese.

Default language is English.

```yml
language: en
# language: zh-Hans
# language: fr-FR
# language: zh-hk
# language: zh-tw
# language: ru
# language: de
```

Set `language` field as following in site `_config.yml` to change to Chinese.

```yml
language: zh-Hans
```

### Comment support.

NexT has native support for `DuoShuo` and `Disqus` comment systems.

Add the following snippets to your `_config.yml`:

```yml
duoshuo:
  enable: true
  shortname: your-duoshuo-shortname
```

OR

```yml
disqus_shortname: your-disqus-shortname
```

### Social Media

NexT can automatically add links to your Social Media accounts:

```yml
social:
  GitHub: your-github-url
  Twitter: your-twitter-url
  Weibo: your-weibo-url
  DouBan: your-douban-url
  ZhiHu: your-zhihu-url
```

### Feed link.

> Show a feed link.

Set `rss` field in theme's `_config.yml`, as the following value:

1. `rss: false` will totally disable feed link.
2. `rss:  ` use sites' feed link. This is the default option.

    Follow the installation instruction in the plugin's README. After the configuration is done for this plugin, the feed link is ready too.

3. `rss: http://your-feed-url` set specific feed link.

### Up to 5 code highlight themes built-in.

NexT uses [Tomorrow Theme](https://github.com/chriskempson/tomorrow-theme) with 5 themes for you to choose from.
Next use `normal` by default. Have a preview about `normal` and `night`:

![Tomorrow Normal Preview](http://iissnan.com/nexus/next/tomorrow-normal.png)
![Tomorrow Night Preview](http://iissnan.com/nexus/next/tomorrow-night.png)

Head over to [Tomorrow Theme](https://github.com/chriskempson/tomorrow-theme) for more details.

## Configuration

NexT comes with few configurations.

```yml

# Menu configuration.
menu:
  home: /
  archives: /archives

# Favicon
favicon: /favicon.ico

# Avatar (put the image into next/source/images/)
# can be any image format supported by web browsers (JPEG,PNG,GIF,SVG,..)
avatar: /default_avatar.png

# Code highlight theme
# available: normal | night | night eighties | night blue | night bright
highlight_theme: normal

# Fancybox for image gallery
fancybox: true

# Specify the date when the site was setup
since: 2013

```

## Browser support

![Browser support](http://iissnan.com/nexus/next/browser-support.png)
