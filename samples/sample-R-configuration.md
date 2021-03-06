---
layout: withtoc
title: "Sample R Configurations"
---

### Single configuration file with relative paths

- Encouraged:
```
# Use of file.path() for platform neutrality
basepath <- file.path("My Documents","Paper 15")
# alternatively, if using Git
basepath =  rprojroot::find_root(rprojroot::is_git_root)

includes <- file.path(basepath,"includes")
inputdata <- file.path(basepath,"inputdata")
tables <- file.path(basepath,"text/tables")
matsize <- 11000
```

### Use of relative paths in main program for inclusions

 Discouraged:
```
source("C:\My Documents\yesterdays programs\submodule.do")
```
- Encouraged:
```
source(file.path(includes,"submodule.R"))
```
or
```
source(file.path("includes","submodule.R")
```

### Installing packages in a project-specific directory

- using the `packrat` package
- using `containerit` package
- manually resetting the R library locations in the config file, then installing packages
```
# Install packages locally
.libPaths( c( includes , .libPaths() ) )
```

You can then create a single dependency file that is run before all others:
```
00_install.R:
# run this only when updating the packages 
# all packages necessary to run the main.R have 
# been included in this replication package 
include "config.R"
install.packages('rddensity', lib=libpath, destdir=libpath)
install.packages('rdrobust', lib=libpath, destdir=libpath) 
install.packages('gridExtra', lib=libpath, destdir=libpath)
install.packages('ggplot2', lib=libpath, destdir=libpath)
```
and in your main program
```
basepath =  rprojroot::find_root(rprojroot::is_git_root)
source(file.path(basepath,"config.R"))
library(rdrobust)
library(rddensity)
library(gridExtra)
library(ggplot2)
```
will always work, because it no longer requires download, and fixes the version of the packages used.