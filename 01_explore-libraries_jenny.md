01\_explore-libraries\_jenny.R
================
Dario
Wed Jan 31 14:06:21 2018

``` r
## how jenny might do this in a first exploration
## purposely leaving a few things to change later!
```

Which libraries does R search for packages?

``` r
.libPaths()
```

    ## [1] "/Library/Frameworks/R.framework/Versions/3.4/Resources/library"

``` r
## let's confirm the second element is, in fact, the default library
.Library
```

    ## [1] "/Library/Frameworks/R.framework/Resources/library"

``` r
library(fs)
path_real(.Library)
```

    ## /Library/Frameworks/R.framework/Versions/3.4/Resources/library

Installed packages

``` r
library(tidyverse)
```

    ## ── Attaching packages ─────────────────────────────────────────────────────────────────────────────────────── tidyverse 1.2.1 ──

    ## ✔ ggplot2 2.2.1.9000     ✔ purrr   0.2.4     
    ## ✔ tibble  1.4.1          ✔ dplyr   0.7.4     
    ## ✔ tidyr   0.7.2          ✔ stringr 1.2.0     
    ## ✔ readr   1.1.1          ✔ forcats 0.2.0

    ## ── Conflicts ────────────────────────────────────────────────────────────────────────────────────────── tidyverse_conflicts() ──
    ## ✖ dplyr::filter() masks stats::filter()
    ## ✖ dplyr::lag()    masks stats::lag()

``` r
ipt <- installed.packages() %>%
  as_tibble()

## how many packages?
nrow(ipt)
```

    ## [1] 334

Exploring the packages

``` r
## count some things! inspiration
##   * tabulate by LibPath, Priority, or both
ipt %>%
  count(LibPath, Priority)
```

    ## # A tibble: 3 x 3
    ##   LibPath                                                 Priority       n
    ##   <chr>                                                   <chr>      <int>
    ## 1 /Library/Frameworks/R.framework/Versions/3.4/Resources… base          14
    ## 2 /Library/Frameworks/R.framework/Versions/3.4/Resources… recommend…    15
    ## 3 /Library/Frameworks/R.framework/Versions/3.4/Resources… <NA>         305

``` r
##   * what proportion need compilation?
ipt %>%
  count(NeedsCompilation) %>%
  mutate(prop = n / sum(n))
```

    ## # A tibble: 3 x 3
    ##   NeedsCompilation     n   prop
    ##   <chr>            <int>  <dbl>
    ## 1 no                 159 0.476 
    ## 2 yes                155 0.464 
    ## 3 <NA>                20 0.0599

``` r
##   * how break down re: version of R they were built on
ipt %>%
  count(Built) %>%
  mutate(prop = n / sum(n))
```

    ## # A tibble: 4 x 3
    ##   Built     n   prop
    ##   <chr> <int>  <dbl>
    ## 1 3.4.0   196 0.587 
    ## 2 3.4.1    49 0.147 
    ## 3 3.4.2    66 0.198 
    ## 4 3.4.3    23 0.0689

Reflections

``` r
## reflect on ^^ and make a few notes to yourself; inspiration
##   * does the number of base + recommended packages make sense to you?
##   * how does the result of .libPaths() relate to the result of .Library?
```

Going further

``` r
## if you have time to do more ...

## is every package in .Library either base or recommended?
all_default_pkgs <- list.files(.Library)
all_br_pkgs <- ipt %>%
  filter(Priority %in% c("base", "recommended")) %>%
  pull(Package)
setdiff(all_default_pkgs, all_br_pkgs)
```

    ##   [1] "abind"               "acepack"             "alluvial"           
    ##   [4] "animation"           "assertthat"          "AUC"                
    ##   [7] "backports"           "base64enc"           "BH"                 
    ##  [10] "bindr"               "bindrcpp"            "binman"             
    ##  [13] "bit"                 "bit64"               "bitops"             
    ##  [16] "blob"                "blogdown"            "bookdown"           
    ##  [19] "brew"                "broom"               "callr"              
    ##  [22] "car"                 "caTools"             "cellranger"         
    ##  [25] "checkmate"           "classInt"            "cli"                
    ##  [28] "clipr"               "clisymbols"          "coda"               
    ##  [31] "colorspace"          "commonmark"          "config"             
    ##  [34] "cowplot"             "crayon"              "crosstalk"          
    ##  [37] "curl"                "data.table"          "DBI"                
    ##  [40] "dbplyr"              "deldir"              "DEoptimR"           
    ##  [43] "desc"                "devtools"            "dichromat"          
    ##  [46] "digest"              "diptest"             "doParallel"         
    ##  [49] "dotCall64"           "dplyr"               "e1071"              
    ##  [52] "enc"                 "evaluate"            "expm"               
    ##  [55] "fields"              "flexdashboard"       "flexmix"            
    ##  [58] "forcats"             "foreach"             "forecast"           
    ##  [61] "formatR"             "Formula"             "fpc"                
    ##  [64] "fracdiff"            "fs"                  "gam"                
    ##  [67] "gbm"                 "gdata"               "geoparser"          
    ##  [70] "geosphere"           "ggalluvial"          "GGally"             
    ##  [73] "gganimate"           "ggdendro"            "ggformula"          
    ##  [76] "ggmap"               "ggplot2"             "ggpolypath"         
    ##  [79] "ggpubr"              "ggRandomForests"     "ggrepel"            
    ##  [82] "ggsci"               "ggseas"              "ggsignif"           
    ##  [85] "ggthemes"            "ggvis"               "gh"                 
    ##  [88] "git2r"               "glm2"                "glue"               
    ##  [91] "gmodels"             "goftest"             "googleAuthR"        
    ##  [94] "gplots"              "gridExtra"           "gridSVG"            
    ##  [97] "gtable"              "gtools"              "haven"              
    ## [100] "here"                "hexbin"              "highr"              
    ## [103] "Hmisc"               "hms"                 "htmlTable"          
    ## [106] "htmltools"           "htmlwidgets"         "httpuv"             
    ## [109] "httr"                "influence.ME"        "ini"                
    ## [112] "instaR"              "ISLR"                "iterators"          
    ## [115] "jpeg"                "jsonlite"            "kableExtra"         
    ## [118] "kernlab"             "knitLatex"           "knitr"              
    ## [121] "labeling"            "Lahman"              "latticeExtra"       
    ## [124] "lazyeval"            "leaps"               "LearnBayes"         
    ## [127] "lme4"                "lmtest"              "lubridate"          
    ## [130] "magick"              "magrittr"            "mapdata"            
    ## [133] "mapmate"             "mapproj"             "maps"               
    ## [136] "maptools"            "markdown"            "MatrixModels"       
    ## [139] "mclust"              "memoise"             "microbenchmark"     
    ## [142] "mime"                "miniUI"              "minqa"              
    ## [145] "mnormt"              "modelr"              "modeltools"         
    ## [148] "mosaic"              "mosaicCore"          "mosaicData"         
    ## [151] "multcomp"            "munsell"             "mvtnorm"            
    ## [154] "nloptr"              "NLP"                 "nycflights13"       
    ## [157] "openssl"             "packrat"             "pander"             
    ## [160] "parsedate"           "pbkrtest"            "pillar"             
    ## [163] "pkgconfig"           "PKI"                 "placeholder"        
    ## [166] "plogr"               "plotly"              "plotrix"            
    ## [169] "plotROC"             "plyr"                "png"                
    ## [172] "polspline"           "polyclip"            "prabclus"           
    ## [175] "praise"              "prettyunits"         "printr"             
    ## [178] "pROC"                "progress"            "proto"              
    ## [181] "psych"               "purrr"               "purrrlyr"           
    ## [184] "quadprog"            "quantmod"            "quantreg"           
    ## [187] "R.methodsS3"         "R.oo"                "R.utils"            
    ## [190] "R6"                  "randomForest"        "randomForestSRC"    
    ## [193] "randomUniformForest" "RANN"                "rappdirs"           
    ## [196] "raster"              "RColorBrewer"        "Rcpp"               
    ## [199] "RcppArmadillo"       "RcppEigen"           "RcppRoll"           
    ## [202] "RCurl"               "readr"               "readxl"             
    ## [205] "rematch"             "rematch2"            "reprex"             
    ## [208] "reprtree"            "repurrrsive"         "reshape"            
    ## [211] "reshape2"            "rgdal"               "rgeos"              
    ## [214] "RgoogleMaps"         "rjson"               "RJSONIO"            
    ## [217] "rlang"               "rlist"               "rmarkdown"          
    ## [220] "rms"                 "RMySQL"              "ROAuth"             
    ## [223] "robustbase"          "ROCR"                "RoogleVision"       
    ## [226] "roxygen2"            "rprojroot"           "rrefine"            
    ## [229] "rsconnect"           "RSelenium"           "RSocrata"           
    ## [232] "RSQLite"             "rstudioapi"          "rticles"            
    ## [235] "rtweet"              "rvest"               "rworldmap"          
    ## [238] "sandwich"            "scales"              "SDMTools"           
    ## [241] "searchConsoleR"      "seasonal"            "selectr"            
    ## [244] "semver"              "servr"               "sf"                 
    ## [247] "shiny"               "slam"                "SnowballC"          
    ## [250] "sourcetools"         "sp"                  "spam"               
    ## [253] "sparklyr"            "SparseM"             "spatialEco"         
    ## [256] "SpatialPack"         "spatstat"            "spatstat.data"      
    ## [259] "spatstat.utils"      "spData"              "spdep"              
    ## [262] "stargazer"           "streamR"             "stringdist"         
    ## [265] "stringi"             "stringr"             "styler"             
    ## [268] "subprocess"          "telegram"            "tensor"             
    ## [271] "testthat"            "TH.data"             "tibble"             
    ## [274] "tidyr"               "tidyselect"          "tidyverse"          
    ## [277] "tigris"              "timeDate"            "tm"                 
    ## [280] "topicmodels"         "translations"        "tree"               
    ## [283] "trimcluster"         "tseries"             "TTR"                
    ## [286] "twitteR"             "udunits2"            "units"              
    ## [289] "usethis"             "utf8"                "uuid"               
    ## [292] "viridis"             "viridisLite"         "wdman"              
    ## [295] "whisker"             "withr"               "wordcloud"          
    ## [298] "x13binary"           "XML"                 "xml2"               
    ## [301] "xtable"              "xts"                 "yaImpute"           
    ## [304] "yaml"                "zipcode"             "zoo"

``` r
## study package naming style (all lower case, contains '.', etc

## use `fields` argument to installed.packages() to get more info and use it!
ipt2 <- installed.packages(fields = "URL") %>%
  as_tibble()
ipt2 %>%
  mutate(github = grepl("github", URL)) %>%
  count(github) %>%
  mutate(prop = n / sum(n))
```

    ## # A tibble: 2 x 3
    ##   github     n  prop
    ##   <lgl>  <int> <dbl>
    ## 1 F        191 0.572
    ## 2 T        143 0.428
