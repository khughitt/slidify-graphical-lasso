---
title       : Sparse inverse covariance estimation with the graphical lasso
subtitle    : Friedman, Jerome, Hastie, Trevor, Tibshirani, Robert
author      : Keith Hughitt
job         : 
framework   : io2012        # {io2012, html5slides, shower, dzslides, ...}
highlighter : highlight.js  # {highlight.js, prettify, highlight}
hitheme     : tomorrow      # 
widgets     : [mathjax]            # {mathjax, quiz, bootstrap}
mode        : selfcontained # {standalone, draft}
---

## Graphical Models

Graphical models provide ... to represent the conditional dependencies between
a number of random variables. They provide a visual way of representing the
joint distribution of the entire set of RVs (Hastie et al, 2009.)

Components
- <span class='blue2'>Vertices</span> random variables
- <span class='blue2'>Edges</span> missing edges imply condition independence

---

## Graphical Models

* <span class='blue2'>Model selection</span> choosing the structure of the graph.
* <span class='blue2'>Learning</span> Estimating edge weights from data.

![plot of chunk pgm_example](figure/pgm_example.png) 

