# MaSigPro 
## Introduction
MaSigPro follows a two-step regression strategy to find genes with significant temporal expression changes and significant differences between experimental groups. The method defines a general regression model for the data where the experimental groups are identified by dummy variables. The procedure first adjusts this global model by the least-squared technique to identify differentially expressed genes and selects significant genes applying false discovery rate control procedures. Secondly, stepwise regression is applied as a variable selection strategy to study differences between experimental groups and to find statistically significant different profiles. The coefficients obtained in this second regression model will be useful to cluster together significant genes with similar expression patterns and to visualize the results.

## Prerequisites
### Rstudio
Rstudio is an integrated development environment for R that includes a console with tools for plotting, history, debugging, and workspace management. It is recommended, but not necessary, to operate this pipeline on a local computer with R and Rstudio installed. The Rstudio interface allows the user to easily monitor each step of the pipeline. Download sources for R and Rstudio are below.

```
 https://cran.cnr.berkeley.edu/
 https://www.rstudio.com/products/rstudio/download/#download
``` 
 
### Data object
The data object should be a tab delimited text file that contains the normalized count data. Within the data frame, the row names should be an array of gene names, and the column names should be an array of the individual observation points per time point per replicate.

### Experimental Design Object
The experimental design object will provide the information required to construct the regression model that will be used to identify significant genes. The data frame should also be a tab delimited text file that contains (2 + # of groups) columns not including the row names. The row names should be the same array as the column names in the data object. The order of observation points must remain consistent. The first official column of the edesign object should be the time points corresponding to the observation row. The second column should be the replicate identifier. Each replicate of a single group per time point should be categorized under one number. For example, each replicate of group 1 at time point 0 should have the same replicate identifier value. The remaining columns categorize the observation points into the experimental groups. Each group column represents one experimental group. The elements of these columns are binary values 0 or 1 representing FALSE or TRUE. An observation point that belongs to group 1 will have a 1 value for the “group 1” column and a 0 value for the other group coloumns.    

## Installing
MaSigPro can be obtained on the bioconductor page:

```
https://bioconductor.org/packages/release/bioc/html/maSigPro.html
```

or through executing in R the command

```
install.packages("maSigPro") 
```

Once installed, execute this command to load the package

```
library(maSigPro)
```

## Step 1: Run Defining_Regression_Model.R line by line

## Step 2: Run Finding_Significant_Genes.R line by line

## Step 3: Run Finding_Significant_Differences.R line by line

## Step 4: Run Obtaining_Lists.R line by line
