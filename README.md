# tcherry

The package is meant for learning the structure of the type of graphical models called t-cherry trees from data. The functions can only be used for categorical data. The purpose of the package is only to provide functions for learning the structure of the graph from given data with no missing values. The functions are attempting to find structures of maximal likelihood. If the corresponding graphical model is to be used in connection with probability propagation or prediction other packages are needed (see the vignette for examples).

## Install
Without vignettes
`remotes::install_github("nvihrs14/tcherry")`

With vignettes
`remotes::install_github("nvihrs14/tcherry",  build_opts = c("--no-resave-data", "--no-manual"))`

If there are problems with viewing documentation or vignettes, it is recommended to restart the R session.

Note that the package requres the following R-packages, which are automatically installed with the package:
    Rdpack, utils, gRbase, compare and stats.

## Main functions (see vignette for more details)
-__`increase_order2`__: Determine a (k + 1)'th order t-cherry tree from a k'th order t-cherry tree by a greedy search.

-__`increase_order_complete_search`__: Determine the (k + 1)'th order t-cherry tree from a k'th order t-cherry tree by a complete search.

-__`k_tcherry_p_lookahead`__: Determine a k'th order t-cherry tree from data by adding p cliques at a time by a greedy search. Not that if p is the total number of cliques in a k'th order t-cherry tree with the desired number of vertices this is a complete search.

-__`thinning_edges`__: Thinning of edges in a graphical model with a triangulated graph.

## Example usage
To demonstrate the main functions in this package consider the car evaluation data set from UCI Machine Learning Repository (Dau & Graff 2017). This data set contains 7 variables (all categorical) with 1728 observations for each and no missing values. The variables are describing different aspects of the car such as the estimated safety of the car, the number of doors etc. To find a graphical structure of a third order t-cherry tree for this data the function k_tcherry_p_lookahead is used. It is chosen to add just one clique at a time in the greedy search procedure.

``` r
library(tcherry)
car <- read.table("https://archive.ics.uci.edu/ml/machine-learning-databases/car/car.data", header = FALSE, sep = ",", dec = ".")
names(car) <- c("buying", "maint", "doors", "persons", "lug_boot",
                  "safety", "class")
tch3 <- k_tcherry_p_lookahead(data = car, k = 3, p = 1, smooth = 0.001)
tch3$adj_matrix
#>            buying maint doors persons lug_boot safety class
#> buying        0     1     0       0        0      1     1
#> maint         1     0     0       0        0      0     1
#> doors         0     0     0       0        1      0     1
#> persons       0     0     0       0        0      1     1
#> lug_boot      0     0     1       0        0      1     1
#> safety        1     0     0       1        1      0     1
#> class         1     1     1       1        1      1     0

```
Note that the smooth argument is added to cell count when estimating probabilities to avoid zero probabilities, which would make some calculations invalid.
## For more help

See documentation included in package (vignettes and man) at <https://github.com/nvihrs14/tcherry>

## References
Dua, D. & Graff, C. (2017). UCI machine learning repository.
