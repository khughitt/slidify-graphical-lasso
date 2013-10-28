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

--- .segue .dark

## Graphical Models

---

## Graphical Models

Graphical models provide a way to represent the conditional dependencies 
between a number of random variables. They provide a visual way of representing 
the joint distribution of the entire set of RVs.

<span class='red'>Components:</span>
- Vertices: random variables
- Edges: condition dependencies between RVs

<span class='red'>Types:</span>
- Directed: `Bayesian networks` (causal relationships)
- Undirected: `Markov random fields` / `Markov networks`

See [The Elements of Statistical Learning (ESM)](http://www-stat.stanford.edu/~tibs/ElemStatLearn/)
chapter 17 for an overview of undirected graphical models.

---

## Graphical Models

Constructing graphical models from data:

* <span class='blue2'>Model selection</span>: choosing the structure of the graph.
* <span class='blue2'>Learning</span>: Estimating edge weights from data.

![plot of chunk pgm_example](figure/pgm_example.png) 

---

## Undirected Graphical Models

### Gaussian Graphical Models
- Assume that the observations have a multivariate Gaussian distribution with
mean $\mu$ and covariance matrix $\Sigma$.

- The <span class='blue'>inverse covariance matrix</span> 
  $\Theta = \Sigma^{-1}$ (aka concentration matrix or precision matrix) 
  contains information about the <span class='blue'>partial covariances</span> 
  between each pair of nodes conditioned on all other variables (ESL.)

- If the $ij$th component of $\Theta$ is zero, then variables $i$ and $j$ are
  conditionally independent, given the other variables.

### Covariance graphs

- In covariance graphs or relevance networks, edges are present when the
<span class='blue'>covariance</span> is non-zero.

---

## Sparse Graphical Models

>- In some cases, you expect that the underlying data should be sparse.
>- Want to only keep significant edges.
>- An $L_1$ penalty on the estimation of $\Sigma^{-1}$ can be used to induce 
   sparseness.

---.segue .dark

## Using the Lasso for Sparse Graphical Models

---

## Meinshausen and Bühlmann (2006)

- Which components of $\Sigma^{-1}$ are non-zero?
- Fit a lasso regression for each variable using all other variables as
  predictors.
- Considered nonzero if $\Sigma^{-1}_{ij} \neq 0$ AND/OR 
  $\Sigma^{-1}_{ji} \neq 0$
- Shown asymptotically to find nonzero components

---

## Exact solutions

Other authors have suggested exact solutions:

- Yuan & Lin (2007)
- Banerjee et al. (2007)
- Dahl et al. (2007)

[Interior point optimization](http://en.wikipedia.org/wiki/Interior_point_method)
is used to determine an exact maximiation.

---



## System info


```r
sessionInfo()
```

```
## R version 3.0.2 (2013-09-25)
## Platform: x86_64-unknown-linux-gnu (64-bit)
## 
## locale:
##  [1] LC_CTYPE=en_US.utf8       LC_NUMERIC=C             
##  [3] LC_TIME=en_US.utf8        LC_COLLATE=en_US.utf8    
##  [5] LC_MONETARY=en_US.utf8    LC_MESSAGES=en_US.utf8   
##  [7] LC_PAPER=en_US.utf8       LC_NAME=C                
##  [9] LC_ADDRESS=C              LC_TELEPHONE=C           
## [11] LC_MEASUREMENT=en_US.utf8 LC_IDENTIFICATION=C      
## 
## attached base packages:
## [1] stats     graphics  grDevices utils     datasets  methods   base     
## 
## other attached packages:
##  [1] slidify_0.3.3       devtools_1.3        glasso_1.7         
##  [4] knitcitations_0.5-0 bibtex_0.3-6        knitr_1.5          
##  [7] igraph_0.6.5-2      colorout_1.0-0      vimcom_0.9-9       
## [10] setwidth_1.0-3     
## 
## loaded via a namespace (and not attached):
##  [1] digest_0.6.3   evaluate_0.5.1 formatR_0.9    httr_0.2      
##  [5] markdown_0.6.3 memoise_0.1    parallel_3.0.2 RCurl_1.95-4.1
##  [9] stringr_0.6.2  tools_3.0.2    whisker_0.3-2  XML_3.98-1.1  
## [13] xtable_1.7-1   yaml_2.1.8
```



