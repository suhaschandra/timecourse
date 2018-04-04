# MaSigPro simplified pipeline
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

Reference "data.abiotic.txt" for an example

### Experimental Design Object
The experimental design object will provide the information required to construct the regression model that will be used to identify significant genes. The data frame should also be a tab delimited text file that contains (2 + # of groups) columns not including the row names. The row names should be the same array as the column names in the data object. The order of observation points must remain consistent. The first official column of the edesign object should be the time points corresponding to the observation row. The second column should be the replicate identifier. Each replicate of a single group per time point should be categorized under one number. For example, each replicate of group 1 at time point 0 should have the same replicate identifier value. The remaining columns categorize the observation points into the experimental groups. Each group column represents one experimental group. The elements of these columns are binary values 0 or 1 representing FALSE or TRUE. An observation point that belongs to group 1 will have a 1 value for the “group 1” column and a 0 value for the other group coloumns.    

Reference "edesign.abiotic.txt" for an example

## Installing
MaSigPro can be obtained with the following commands:

```
## try http:// if https:// URLs are not supported
source("https://bioconductor.org/biocLite.R")
biocLite("maSigPro")
```
Once installed, execute this command to load the package

```
library(maSigPro)
```

## Step 1: Importing files into R
```
data <- read.delim("~/directory/pathway/filename.txt", row.names = 1)
edesign <- read.delim("~/directory/pathway/filename.txt", row.names = 1)
```

## Step 2: Defining the Regression Model
```
design <- make.design.matrix(edesign, degree = degree)
```
This function uses the experimental design data frame and degrees of freedom to define the regression model that will be used to identify significant genes.

Required parameters
1. **edesign**: experimental design data frame
2. **degree**: degrees of freedom; equal to 1 less than the # of time points

Optional parameters
1. **time.col**: column number in edesign containing time values; default is first column
2. **repl.col**: column number in edesign containing coding for replicate arrays; default is second column
3. **groups.col**: column numbers in edesign indicating the codinf for each experimental group

## Step 3: Finding Significant Genes
```
fit <- p.vector(data, design, Q = 0.05, min.obs = min.obs)
```
The function "p.vector" computes a regression fit for each gene as well as the p-value associated with the F-statistic of the model which is used to select siginificant genes. By default, this p-value is corrected for multiple comparisons using the linear step-up false discovery rate procedure developed by Benjamini and Hochberg. 

Parameters
1. **data**: data object
2. **design**: design object from previous step
3. **Q**: level of FDR control
4. **min.obs**: For every gene calculates # of non-0 observations (say Y). If Y < min obs, get rid of that gene. Recommended = (# groups)(# time points) +1

## Step 4: Finding Significant Differences
```
tstep <- T.fit(fit, step.method = "backward")
```
The function "T.fit" uses stepwise regression to identify siginificant variables in each gene. Stepwise regression evaluates the each variable for its contribution to increasing the R-squared value for the model.

Required parameters
1. **fit**: p.vector object from previous step
2. **step.method**: "backward", "forward", "two.ways.backward", "two.ways.forward"; default is "backward"

Optional parameters
1. **alfa**: significance value; default uses input for "Q" in previous step

## Step 5: Obtaining Lists of Significant Genes
```
sigs <- get.siggenes(tstep, rsq = 0.7, vars = "groups")
```
The function "get.siggenes" generates lists of significant genes according to user input

Parameters
1. **rsq**: cutoff value from 0 - 1 for the R-squared of the regression model; higher rsq equates to a less populated list
2. **vars**: indicates how to group variables to show results:
 * *"groups"*: generates one list of significant genes for each group; the first group will be treated as the reference group and compared to a zero change profile; the remaining groups will be compared to the reference group 
 * *"all"*: generates one list of significant genes for the entire experiment
 * *"each"*: generates one list of significant genes for each variable in the regression model
 
## Step 6: Exporting Lists of Significant Genes
```
write.table(sigs$summary, "~/directory/pathway/filename.txt", sep = "\t", quote = FALSE)
```
## Useful Functions in Rstudio
To access help window
```
help()
```
To access underlying code of a function
```
getAnywhere()
```
## References
* [maSigPro user guide](https://bioconductor.org/packages/release/bioc/vignettes/maSigPro/inst/doc/maSigProUsersGuide.pdf)
* [bioconductor](https://bioconductor.org/packages/release/bioc/html/maSigPro.html)
