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

<span class='blue2'>Components:</span>
- Vertices: random variables
- Edges: condition dependencies between RVs

<span class='blue2'>Types:</span>
- Directed: Bayesian networks (causal relationships)
- Undirected: Markov random fields / Markov networks

See [ESL](http://www-stat.stanford.edu/~tibs/ElemStatLearn/) chapter 17 for
basics of undirected graphical models.

---

## Graphical Models

* <span class='blue2'>Model selection</span>: choosing the structure of the graph.
* <span class='blue2'>Learning</span>: Estimating edge weights from data.

![plot of chunk pgm_example](figure/pgm_example.png) 

---

## Inverse Covariance Matrix

The inverse covariance matrix $\Theta = \Sigma^{-1}$ contains information about the
<i>partial covariances</i> between each pair of nodes conditioned on all other
variables (ESL.)

- If the $ij$th component of $\Theta$ is zero, then variables $i$ and $j$ are
  conditionally independent, given the other variables.

---

## Meinshausen and Bühlmann (2006)

- Which components of $\Sigma^{-1}$ are non-zero?
- Fit a lasso regression for each variable using all other variables as
  predictors.
- Considered nonzero if $\Sigma^{-1}_{ij} \neq 0$ AND/OR 
  $\Sigma^{-1}_{ji} \neq 0$

---

