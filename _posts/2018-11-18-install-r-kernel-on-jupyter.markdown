---
title: Install R Kernel on Jupyter
description: notes of installation of R kernel on MacOS X
categories:
 - notes
tags: R, Jupyter, IRkernel
---

> R for jupyteR

## Installation via supplied binary packages

Open terminal and run R. Then run the following commands in the console.

```
install.packages(c('repr', 'IRdisplay', 'evaluate', 'crayon', 'pbdZMQ', 'devtools', 'uuid', 'digest'))

devtools::install_github('IRkernel/IRkernel')
```

## Making the kernel available to Jupyter

On OS X, be sure to execute this in R started from the Terminal, not the R App!

```
IRkernel::installspec()
```