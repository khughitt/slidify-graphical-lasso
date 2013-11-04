---
title       : Sparse inverse covariance estimation with the graphical lasso
subtitle    : Friedman, J., Hastie, T., Tibshirani, R.
author      : Keith Hughitt
job         : 
framework   : io2012        # {io2012, html5slides, shower, dzslides, ...}
highlighter : highlight.js  # {highlight.js, prettify, highlight}
hitheme     : tomorrow      # 
widgets     : [mathjax]            # {mathjax, quiz, bootstrap}
github      :
  user: khughitt
  repo: slidify-graphical-lasso
mode        : selfcontained # {standalone, draft}
--- .segue .dark

<!-- Custom Styles -->
<style type='text/css'>
    slides > slide {
        height: 800px;
        margin-top: -400px;
    }
    img {
        max-height: 560px;
        max-width: 964px;
    }
    slide a {border-bottom: none;}
    .source { bottom: 35px; }
    .references li { font-size: 18px; }
</style>

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

* <span class='blue2'>Model selection</span>: choosing the structure of the 
  graph.
* <span class='blue2'>Learning</span>: Estimating edge weights from data.

<div class='source'>ESL p626</div>

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

## Cell Signaling

---

## Cell signaling

The basic goal of cell signaling to provide a mechanism for cells to respond
convey a message from one part of a cell to another.

![signaling basics](assets/img/1Signal_Transduction_Pathways_Model.jpg)
<div class='source'>http://commons.wikimedia.org/wiki/File:1Signal_Transduction_Pathways_Model.jpg</div>

---

## Cell signaling

### Examples of cell signaling

- Cell to cell communication (e.g. quorum sensing)
- Responding to host (or pathogen) molecules
- Respond to external physiological, nutrient, etc. conditions
- Chemotaxis (movement)

---

## Cell signaling

### Cell membrane

Cell signaling often begins at the cell membrane ("Signal transduction").
![cell membrane](assets/img/1000px-Cell_membrane_detailed_diagram_en.svg.png)
<div class='source'>Source: http://en.wikipedia.org/wiki/File:Cell_membrane_detailed_diagram_en.svg</div>

---

## Cell signaling

### Lipid rafts
Lipid rafts help to bring together components that function together for a
particular signaling pathway: receptors, co-receptors, etc.

![lipid rafts](assets/img/Cell-Wave-Final.png)
<div class='source'>source: http://www.lanl.gov/science/1663/august2011/story3full.shtml</div>

---

## Cell signalling network

It's not quite a 1:1 process...
![Figure 2](assets/img/Sachs_F2.large.jpg)
<div class='source'>Sachs 2003; Figure 2</div>

---

## Cell signalling network

Figure 1A: Network inference using flow cytometry data (Sachs et al 2005)
![Figure 1a](assets/img/Sachs_F1a.large.jpg)

---.segue .dark

## Flow Cytometry

---

## Flow Cytometry

Flow cytometry a high-throughput single-cell method that is generally
used for one of two things:

1. Counting cells
2. Sorting cells

However, it can also be used to measure many other properties of cells
(expression, morphology, enzymatic activity, etc.)

![flow cytometry](assets/img/flow-cytometry_500.jpg)
<div class='source'>http://www.semrock.com/flow-cytometry.aspx</div>

---

## Flow Cytometry

- Molecules of interest (e.g. proteins involved in a signaling cascade) are 
"tagged" with an antibody containing a fluorescent protein.
- By using different fluorescent proteins (each of which emits at a different
  wavelength) multiple proteins can be bound and measured simultaneously.

![flow cytometry antibodies](assets/img/resources_FACS_2_de.jpg)
<div class='source'>http://www.antibodies-online.com/resources/17/1247/What+is+flow+cytometry+FACS+analysis/</div>

---

## Flow Cytometry

### Multicolor Flow Cytometry

- Advantages
    1. Quantitative
    2. Can interrogate levels of multiple molecules at the same time
    3. Processes many individual cells (large $n$)
- Disadvantages
    1. Spectral overlap
    2. Variable brightness of fluorochromes

---

## Flow Cytometry

### Spectral Overlap

![Fluorochromes](assets/img/1-s2.0-S0022175900002295-gr1.jpg)
<div class='source'>Baumgarth and Roederer, 2000</div>

---

## Cell signalling network

Figure 1B-C: Network inference using flow cytometry data (Sachs et al 2005)
![Figure 1bc](assets/img/Sachs_F1b.large.jpg)

---.segue .dark

## Previous Work on Using the Lasso for Sparse Graphical Models

---

## Meinshausen and Bühlmann (2006)

>- Which components of $\Sigma^{-1}$ are non-zero?
>- Fit a lasso regression for each variable using all other variables as
   predictors.
>- Considered nonzero if $\Sigma^{-1}_{ij} \neq 0$ AND/OR 
   $\Sigma^{-1}_{ji} \neq 0$
>- Shown asymptotically to find nonzero components

---

## Exact solutions

Other authors have suggested exact solutions:

- Yuan & Lin (2007)
- Banerjee et al. (2007)
- Dahl et al. (2007)

[Interior point optimization](http://en.wikipedia.org/wiki/Interior_point_method)
is used to determine an exact maximiation.

---

## Graphical Lasso

- Exact solution based on coordinate descent approach in Banerjee et al. (2007)

<span class='red' style='font-weight: 700;'>Approach</span>

>- $N$ observations, $x_i, i=1,\ldots,N$ with dimension $p$, mean $\mu$ and 
   covariance $\Sigma$
>- let $\Theta = \Sigma^{-1}$ and let $S$ be the <span class='blue'>empirical
   covariance matrix</span>:
    $$S = \frac{1}{N} \sum_{i=1}^N (x_i - \overline{x})(x_i - \overline{x})^T$$
>- <span class='blue'>Goal:</span> Maximize the log-likelihood
    $$\text{log det} \Theta - \text{tr}(S\Theta) - \rho\Vert \Theta \Vert_1$$
   over non-negative definite matrices $\Theta$.
>- Above expression is the "Gaussian log-likelihood of the data, partially
   maximized with respect to the mean parameter $\mu$.

--- .segue .dark

## An interesting methods discussion...

---

## Algorithm (Friedman et al. 2007):

1. Start with $W = S + \rho I$. The diagonal of $W$ remains unchanged in what
follows.
2. For each $j = 1,2,\ldots p,1,2,\ldots p,\ldots,$ solve the lasso problem:
<span class='blue'>
    $$ min_\beta \{ \frac{1}{2} \Vert W^{1/2}_{11} \beta - b\Vert^2 + \rho \Vert \beta \Vert_1 \}$$
</span>
   where $b = W^{-1/2}_{11} s_{12}$, which takes as input the inner products 
  $W_{11}$ and $s_{12}$.  This gives a $p -1$ vector solution $\hat{\beta}$. 
  Fill in the corresponding row and column $W$ using $w_{12} = W_{11} \hat{\beta}$.
3. Continue to convergence.

In `glasso`, the procedure stops when the average absolute change in $W$ is
less than $t \cdot  \text{ave} |S^{-\text{diag}}|$ where $S^{-\text{diag}}$ are
the off-diagonal elements of the empirical coveriance matrix $S$ and $t$ is
a fixed threshold, set by default at 0.001.

--- .segue .dark

## An interesting algorithm discussion...

---

## Performance

- Simulated data for sparse and dense scenarios:
- <span class='blue'>Sparse</span>
  *  $(\Sigma^{-1})_{ii} = 1$, 
  * $(\Sigma^{-1})_{i,i-1} = (\Sigma^{-1})_{i-1,i} = 0.5$
  * 0 otherwise

- <span class='blue'>Dense</span>
  * $(\Sigma^{-1})_{ii} = 2$,
  * $(\Sigma^{-1})_{ii'} = 1$ otherwise

- Compared performance to `COVSEL` method from Banerjee et al. (2007).

- <span class='red'>Result</span>: Graphical lasso is 30-4000 times faster than
COVSEL and 2-10 slower than the approximate method.

- Even for dense problems, finishes in ~1min for p=1000 features. (Hard to tell
 from graph how it will scale to many more features, however).

---

## Performance

![Figure 1](assets/img/F1.large.jpg)

---

## Cell signalling network

To demonstrate the usefulness of the graphical lasso on real-world problems,
the method is applied to cytometry data from Sachs et al. (2003) 

![Figure 2](assets/img/F2.large.jpg)

Figure 2: Original network derived in Sachs et al.
- $p$ = 11 proteins
- $n$ = 7466 cells

---

## Cell signalling network

Figure 3: Undirected graphs generated using graphical lasso with various settings for
the regularization parameter, $\rho$. 
![Figure 3](assets/img/F3.large.jpg)

---

## Cell signalling network

Figure 4: Profile of coefficients as the total $L_1$ norm of the coefficient
vector increases and $\rho$ decreases. (The density of network increases as 
$L_1$ increases.)

![Figure 4](assets/img/F4.large.jpg)

---

## Cell signalling network

Figure 5: LHS - tenfold cross-validation. RHS - regression sum of squares of
the exact graphical lasso vs. Meinhausen-Buhlmann approximation.

![Figure 5](assets/img/F5.large.jpg)

--- .segue .dark

## Summary

---

## Summary

1. Friedmann et al. present a fast $L_1$ penalized approach for inducing
sparsity in graphical models.
2. Builds on previous methods, starting with the blockwise coordinate descent
approach from Banerjee et al. (2007.)
3. Method arrives at at exact result and performs significantly faster
than previous methods.
4. Free implementation of the graphical lasso written in fortran and R is
available: `glasso`.

--- .references

## References





- Nicole Baumgarth, Mario Roederer,   (2000) A Practical Approach to Multicolor Flow Cytometry For Immunophenotyping.  <em>Journal of Immunological Methods</em>  <strong>243</strong>  77-97  <a href="http://dx.doi.org/10.1016/S0022-1759(00)00229-5">10.1016/S0022-1759(00)00229-5</a>
- J. Friedman, T. Hastie, R. Tibshirani,   (2007) Sparse Inverse Covariance Estimation With The Graphical Lasso.  <em>Biostatistics</em>  <strong>9</strong>  432-441  <a href="http://dx.doi.org/10.1093/biostatistics/kxm045">10.1093/biostatistics/kxm045</a>
- Nicolai Meinshausen, Peter Bühlmann,   (2006) High-Dimensional Graphs And Variable Selection With The Lasso.  <em>The Annals of Statistics</em>  <strong>34</strong>  1436-1462  <a href="http://dx.doi.org/10.1214/009053606000000281">10.1214/009053606000000281</a>
- K. Sachs,   (2005) Causal Protein-Signaling Networks Derived From Multiparameter Single-Cell Data.  <em>Science</em>  <strong>308</strong>  523-529  <a href="http://dx.doi.org/10.1126/science.1105809">10.1126/science.1105809</a>
- Hastie Trevor,   (2009) The elements of statistical learning data mining, inference, and prediction.


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
##  [1] LC_CTYPE=en_US.UTF-8       LC_NUMERIC=C              
##  [3] LC_TIME=en_US.UTF-8        LC_COLLATE=en_US.UTF-8    
##  [5] LC_MONETARY=en_US.UTF-8    LC_MESSAGES=en_US.UTF-8   
##  [7] LC_PAPER=en_US.UTF-8       LC_NAME=C                 
##  [9] LC_ADDRESS=C               LC_TELEPHONE=C            
## [11] LC_MEASUREMENT=en_US.UTF-8 LC_IDENTIFICATION=C       
## 
## attached base packages:
## [1] stats     graphics  grDevices utils     datasets  methods   base     
## 
## other attached packages:
## [1] knitcitations_0.4-7 bibtex_0.3-6        knitr_1.5          
## [4] igraph_0.6.5-2      slidify_0.3.3       colorout_1.0-0     
## [7] vimcom_0.9-9        setwidth_1.0-3     
## 
## loaded via a namespace (and not attached):
##  [1] digest_0.6.3   evaluate_0.5.1 formatR_0.9    httr_0.2      
##  [5] markdown_0.6.3 RCurl_1.95-4.1 stringr_0.6.2  tools_3.0.2   
##  [9] whisker_0.3-2  XML_3.98-1.1   xtable_1.7-1   yaml_2.1.8
```


<!-- Custom JavaScript -->
<script src="http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.7.min.js"></script>
<script type='text/javascript'>
$(function() {
    $("p:has(img)").addClass('centered');
});
</script>


