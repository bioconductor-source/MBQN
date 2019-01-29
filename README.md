
<!-- README.md is generated from README.Rmd. Please edit that file -->
MBQN Package
============

Mean/Median-balanced quantile normalization for preprocessing omics data

Description
-----------

This package supplies a modified quantile normalization for preprocessing omics or other matrix-like organized data with intensity values biased by global, columnwise distortions of intensity mean and scale. The modification balances the mean intensity of features (i.e. rows) which are rank invariant (RI) or nearly rank invariant (NRI) across samples/columns, before quantile normalization in order to reduce systematics in downstream analysis \[1\].

Installation
------------

To install this package, you need R version &gt;= 3.3.3.

For installation from Github run in R:

``` r
# install.packages("devtools")
devtools::install_github("arianeschad/mbqn")
```

or

``` r
# install.packages("githubinstall")
githubinstall::githubinstall("mbqn")
```

For installation from Bioconductor run in R:

``` r
#if (!requireNamespace("BiocManager", quietly = TRUE))
#    install.packages("BiocManager")
BiocManager::install("MBQN")
```

Dependencies
------------

The MBQN package uses `normalizeQuantiles()` from the package `limma`\[2\] for computation of the quantile normalization, available from <https://bioconductor.org/packages/release/bioc/html/limma.html>. <br/>

Optionally, `normalize.quantiles()` from the package `preprocessCore`\[3\], available from <https://github.com/bmbolstad/preprocessCore>, can be used for quantile normalization. <br/>

The optional example function `mbqnExample()` in MBQN needs to collect data from PRIDE experiments, which requires the R package `rpx` \[4\]. <br/>

To install these packages in R run: <br/>

``` r
BiocManager::install(pkgs = c("preprocessCore","limma","rpx"))
```

Additional packages needed to run MBQN examples: <br/>

``` r
install.packages(pkgs = c("filesstrings"), dependencies = TRUE)
```

After installation, check the `mbqnDemo` and the `mbqnExample` for full working examples with further documentation.

Basic Usage
-----------

The package provides two basic functions: `mbqn()` applies quantile normalization or mean-balanced quantile normalization to a matrix. `mbqnNRI()` applies quantile normalization and mean balanced quantile normalization only to selected nearly rank invariant and rank invariant features, specified by a threshold or manually. The input matrix may contain NAs. To run one of these functions you will need to provide an input matrix similar to the data matrix in `mbqnDemo()` or `mbqnExample()`. The argument `FUN` is used to select between classical quantile normalization (default), and mean or median balanced quantile normalization. The function `mbqnCheckSaturation()` can be used to check a data matrix for rank or nearly rank invariant features. It provides a list of potential RI/NRI features, a rank invariance frequency, and a graphical output.

Examples
--------

Example 1: Generate a simple matrix, apply median-balanced quantile normalization, generate a boxplot of normalized features and check for rank invariant (RI) or nearly rank invariant (NRI) features:

``` r
## basic example
mtx <- matrix(c(5,2,3,NA,4,1,4,2,3,4,6,NA),ncol=3)
mtx.norm <- MBQN::mbqn(x = mtx, FUN = median) # mbqn of full matrix
mtx.nri.norm <- MBQN::mbqnNRI(x = mtx, FUN = median, low_thr = 0.6) # qn of matrix - apply median balancing to NRI features with rank invariance frequency >0.6
MBQN::mbqnBoxplot(mtx.norm, irow = 1)
MBQN::mbqnCheckSaturation(mtx,FUN = median,low_thr = 0.1, filename = "simple_mtx",feature_index = 1)
```

Example 2: This example will download data with LFQ intensities from the PRIDE repository, normalize the data, identifies RI/NRI features, and give graphical output. One can choose between four data sets. By default data files are stored in the current working directory currentdir/examples/PXDxxx.

``` r
## Normalize LFQ intensity data from the PRIDE repository
mbqnExample(which.example = 3)
```

In order to run `mbqnExample()`, the respective proteinGroups.txt file must be first downloaded from the PRIDE repository to the folder currentdir/examples/PXDxxxx/ in advance or you can directly download the data by running the code included in `mbqnExample()`.

Figures
-------

Figures created with MBQN are saved as pdf in the current working directory.

References
----------

\[1\] A. Schad and C. Kreutz, MBQN: R package for mean balanced quantile normalization. In prep. 2019 <br/> \[2\] Ritchie, M.E., Phipson, B., Wu, D., Hu, Y., Law, C.W., Shi, W., and Smyth, G.K. (2015). limma powers differential expression analyses for RNA-sequencing and microarray studies. Nucleic Acids Research 43(7), e47. <br/> \[3\] Ben Bolstad (2018). preprocessCore: A collection of pre-processing functions. R package version 1.44.0. <https://github.com/bmbolstad/preprocessCore> <br/> \[3\] Laurent Gatto (2019). rpx: R Interface to the ProteomeXchange Repository. R package version 1.18.1. <https://github.com/lgatto/rpx>
