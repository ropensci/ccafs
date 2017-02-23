ccafs
=====



<!-- README.md is generated from README.Rmd. Please edit that file -->

[![Build Status](https://travis-ci.org/ropensci/ccafs.svg?branch=master)](https://travis-ci.org/ropensci/ccafs)
[![Build status](https://ci.appveyor.com/api/projects/status/rd3u4qqmlcloh5j7?svg=true)](https://ci.appveyor.com/project/sckott/ccafs)
[![codecov](https://codecov.io/gh/ropensci/ccafs/branch/master/graph/badge.svg)](https://codecov.io/gh/ropensci/ccafs)


R client for Climate Change, Agriculture, and Food Security (CCAFS)
General Circulation Models (GCM) data.

## About CCAFS

CCAFS website: <http://ccafs-climate.org/>

CCAFS GCM data for this package comes from Amazon S3
[root path](http://cgiardata.s3.amazonaws.com). Amazon S3 stands for "Simple
Storage Service" - it's like a file system, and they give you links to the files
and metadata around those links.  More about Amazon S3 below.

CCAFS data can be used for studying climate change, and how climate impacts
various aspects of the earth. Search google scholar with `"CCAFS" "GCM"` to
see example uses.

As far as I can tell, CCAFS GCM data comes from IPCC data.

## About Amazon S3

Amazon S3 stands for "Simple Storage Service" - it's like a file system,
and they give you links to the files and metadata around those links.

S3 is split up into buckets, essentially folder. All CCAFS data is in
one bucket. Within the CCAFS bucket on S3 are a series of nested folders.
To get to various files we need to navigate down the tree of folders.
Keys are file paths with all their parent folders, e.g., "/foo/bar/1/2".
Unfortunately, there's no meaningful search of the CCCAFS data as they
have on their website <http://ccafs-climate.org/>. However, you can
set a prefix for a search of these keys, e.g., "/foo/bar" for the key
above.

Check out <https://aws.amazon.com/s3/> for more info.

## About the package

`ccafs` is a client to work with the data CCAFS provides via Amazon Web
Services S3 data.

The `ccafs` data has access to is the "Spatial Downscaling" data that you see
on the <http://ccafs-climate.org/data/> page. The other data sets are not
open.

Currently, we don't provide a way to search for what data is available.
You have to know what you want, or you can list what is available, and then
pick files from the list. Though there's not a lot of information in the
metadata returned from S3.

We'll work on incorporating a way to search - currently there is no solution.

## Citations

Cite CCAFS data following their guidelines at <http://ccafs-climate.org/about/>

Get a citation for this package like `citation(package = 'ccafs')` after
installing the package.

## raster

The main useful output are `raster` package objects of class `RasterLayer` or `RasterBrick` - so in general have `raster` loaded in your session to maximize
happiness.

## Install

CRAN version


```r
install.packages("ccafs")
```

Development version


```r
devtools::install_github("ropensci/ccafs")
```


```r
library("ccafs")
```

## Fetch files

Note, files are not loaded as they can be very large


```r
key <- "ccafs/ccafs-climate/data/ipcc_5ar_ciat_downscaled/rcp2_6/2030s/bcc_csm1_1_m/10min/bcc_csm1_1_m_rcp2_6_2030s_prec_10min_r1i1p1_no_tile_asc.zip"
(res <- cc_data_fetch(key = key))
#> 
#> <CCAFS GCM files>
#>    12 files
#>    Base dir: /bcc_csm1_1_m_rcp2_6_2030s_prec_10min_r1i1p1_no_tile_asc
#>    File types (count): 
#>      - .asc: 12
```

## Load data

Can load in a single file (gives `RasterLayer`), or many (gives `RasterBrick`)


```r
cc_data_read(res[1])
#> class       : RasterLayer 
#> dimensions  : 900, 2160, 1944000  (nrow, ncol, ncell)
#> resolution  : 0.1666667, 0.1666667  (x, y)
#> extent      : -180, 180, -60, 90  (xmin, xmax, ymin, ymax)
#> coord. ref. : NA 
#> data source : /Users/sacmac/Library/Caches/ccafs/bcc_csm1_1_m_rcp2_6_2030s_prec_10min_r1i1p1_no_tile_asc/prec_1.asc 
#> names       : prec_1 
#> values      : -2147483648, 2147483647  (min, max)
```


```r
cc_data_read(res[1:2])
#> class       : RasterStack 
#> dimensions  : 900, 2160, 1944000, 2  (nrow, ncol, ncell, nlayers)
#> resolution  : 0.1666667, 0.1666667  (x, y)
#> extent      : -180, 180, -60, 90  (xmin, xmax, ymin, ymax)
#> coord. ref. : NA 
#> names       :      prec_1,     prec_10 
#> min values  : -2147483648, -2147483648 
#> max values  :  2147483647,  2147483647
```

## Plot


```r
library("raster")
plot(cc_data_read(res[1:3]))
```

![plot of chunk unnamed-chunk-8](inst/img/unnamed-chunk-8-1.png)


## Meta

* Please [report any issues or bugs](https://github.com/ropensci/ccafs/issues).
* License: MIT
* Get citation information for `ccafs` in R doing `citation(package = 'ccafs')`
* Please note that this project is released with a [Contributor Code of Conduct](CONDUCT.md). By participating in this project you agree to abide by its terms.

[![rofooter](https://ropensci.org/public_images/github_footer.png)](https://ropensci.org)
