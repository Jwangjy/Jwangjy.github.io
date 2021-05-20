---
title: "Index of useful R functions for my reference"
categories:
  - Code Index
tags:
  - R
  - Code
excerpt: "Reference index of R code"
header:
  overlay_image: /assets/images/posts/r reference/r-reference-bg.jpg
  overlay_filter: 0.5 # same as adding an opacity of 0.5 to a black background
  caption: "Photo credit: [**Unsplash**](https://unsplash.com)"
toc: true
---

# Influential Variable Check
```r
COOKS = cooks.distance(fit)
DFBETAS = dfbetas(fit)
DFFITS = as.numeric(dffits(fit))
```

# Loop Function
Simple loop for max likelihood:
```r
grid = seq(17, 19, len = 10000)
value = rep(0, length(grid))

for (i in 1:length(grid)) {
value[i] = Likelihood(x, y, grid[i])
}

plot(grid, value, type="l", xlab="beta1", ylab="likelihood")
grid[which.max(value)]
```

# Plotly
```r
Colorscale options in r:
"Blackbody", "Bluered", "Blues", "Earth", "Electric", "Greens", "Greys", "Hot", "Jet", "Picnic", "Portland", "Rainbow", "RdBu", "Reds", "Viridis", "YlGnBu", "YlOrRd"
```

# Stepwise Regression
```r
Base R:
forwards = step(null, scope = list(lower=formula(null), upper = formula(full)), direction = "forward")
backwards = step(full, scope = list(lower=formula(null), upper = formula(full)), direction = "backward")
bothways = step(null, list(lower = formula(null), upper = formula(full)), direction="both", trace = 0)

library(olsrr)
ols_step_both_aic(model, progress = FALSE, details = FALSE)
ols_step_backward_aic(model, progress = FALSE, details = FALSE, ...)
ols_step_forward_aic(model, progress = FALSE, details = FALSE, ...)
```









