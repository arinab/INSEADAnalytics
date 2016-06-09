---
title: "Sessions 5-6"
author: "T. Evgeniou"
date: "9 Jan 2016"
output: html_document
---
<br>

In this session (and report) we combine the report of [sessions 3-4](http://inseaddataanalytics.github.io/INSEADAnalytics/Session2inclass.html) to complete the  [Boats cases A](http://inseaddataanalytics.github.io/INSEADAnalytics/Boats-A-prerelease.pdf) and  [B](http://inseaddataanalytics.github.io/INSEADAnalytics/Boats-B-prerelease.pdf). 

The purpose of this session is to become familiar with:

1. Understand the use of segmentation in practice;
2. Learn about classification and scoring methods;
3. See how these tools can be used in practice;
4. Introduce advanced methods for data analytics

The first part of this report is identical to that of [sessions 3-4](http://inseaddataanalytics.github.io/INSEADAnalytics/Session2inclass.html), hence you can move to the last part . The content is repeated so that we have the complete solution of the Boats case all in one report 


As always, before starting, make sure you have pulled the [session 5-6 files](https://github.com/InseadDataAnalytics/INSEADAnalytics/tree/master/CourseSessions/Sessions56)  (yes, I know, it says sessions 4-5, but it is 5-6 - need to update all filenames some time, but till then we use common sense and ignore a bit the filenames) on your github repository (if you pull the course github repository you also get the session files automatically). Moreover, make sure you are in the directory of this exercise. Directory paths may be complicated, and sometimes a frustrating source of problems, so it is recommended that you use these R commands to find out your current working directory and, if needed, set it where you have the main files for the specific exercise/project (there are other ways, but for now just be aware of this path issue). For example, assuming we are now in the "MYDIRECTORY/INSEADAnalytics" directory, we can do these: 


```r
getwd()
setwd("CourseSessions/Sessions45")
list.files()
rm(list = ls())  # Clean up the memory, if we want to rerun from scratch
```
As always, you can use the `help` command in Rstudio to find out about any R function (e.g. type `help(list.files)` to learn what the R function `list.files` does).

Let's start.

<hr>
<hr>

### Survey Data for Market Segmentation

We will be using the [boats case study](http://inseaddataanalytics.github.io/INSEADAnalytics/Boats-A-prerelease.pdf) as an example. At the end of this class we will be able to develop (from scratch) the readings of sessions 3-4 as well as understand the tools used and the interpretation of the results in practice - in order to make business decisions. The code used here is along the lines of the code in the session directory, e.g. in the [RunStudy.R](https://github.com/InseadDataAnalytics/INSEADAnalytics/blob/master/CourseSessions/Sessions23/RunStudy.R) file and the report [doc/Report_s23.Rmd.](https://github.com/InseadDataAnalytics/INSEADAnalytics/blob/master/CourseSessions/Sessions23/doc/Report_s23.Rmd) There may be a few differences, as there are many ways to write code to do the same thing. 

Let's load the data: 


```
## Warning in library(package, lib.loc = lib.loc, character.only = TRUE,
## logical.return = TRUE, : there is no package called 'pryr'
```

```
## Error in contrib.url(repos, "source"): trying to use CRAN without setting a mirror
```


```r
ProjectData <- read.csv("data/Boats.csv", sep = ";", dec = ",")  # this contains only the matrix ProjectData
ProjectData = data.matrix(ProjectData)
colnames(ProjectData) <- gsub("\\.", " ", colnames(ProjectData))
ProjectDataFactor = ProjectData[, c(2:30)]
```
<br>
and do some basic visual exploration of the first 50 respondents first (always necessary to see the data first):
<br>


```
## Error in eval(expr, envir, enclos): could not find function "gvisTable"
```

```
## Error in print(m1, "chart"): object 'm1' not found
```
<br>

This is the correlation matrix of the customer responses to the 29 attitude questions - which are the only questions that we will use for the segmentation (see the case):
<br>


```
Error in eval(expr, envir, enclos): could not find function "gvisTable"
```

```
Error in print(m1, "chart"): object 'm1' not found
```
<br>

#### Questions

1. Do you see any high correlations between the responses? Do they make sense? 
2. What do these correlations imply?

##### Answers:
<br>
<br>
<br>
<br>

<hr>

### Key Customer Attitudes

Clearly the survey asked many reduntant questions (can you think some reasons why?), so we may be able to actually "group" these 29 attitude questions into only a few "key factors". This not only will simplify the data, but will also greatly facilitate our understanding of the customers.

To do so, we use methods called [Principal Component Analysis](https://en.wikipedia.org/wiki/Principal_component_analysis) and [factor analysis](https://en.wikipedia.org/wiki/Factor_analysis) as discussed in the [session readings](http://inseaddataanalytics.github.io/INSEADAnalytics/Report_s23.html). We can use two different R commands for this (they make slightly different information easily available as output): the command `principal` (check `help(principal)` from R package [psych](http://personality-project.org/r/psych/)), and the command `PCA` from R package [FactoMineR](http://factominer.free.fr) - there are more packages and commands for this, as these methods are very widely used.  

Here is how the `principal` function is used:
<br>

```r
UnRotated_Results <- principal(ProjectDataFactor, nfactors = ncol(ProjectDataFactor), 
    rotate = "none", score = TRUE)
```

```
## Error in eval(expr, envir, enclos): could not find function "principal"
```

```r
UnRotated_Factors <- round(UnRotated_Results$loadings, 2)
```

```
## Error in eval(expr, envir, enclos): object 'UnRotated_Results' not found
```

```r
UnRotated_Factors <- as.data.frame(unclass(UnRotated_Factors))
```

```
## Error in as.data.frame(unclass(UnRotated_Factors)): object 'UnRotated_Factors' not found
```

```r
colnames(UnRotated_Factors) <- paste("Component", 1:ncol(UnRotated_Factors), 
    sep = " ")
```

```
## Error in ncol(UnRotated_Factors): object 'UnRotated_Factors' not found
```

<br>
<br>

Here is how we use `PCA` one is used:
<br>


```r
Variance_Explained_Table_results <- PCA(ProjectDataFactor, graph = FALSE)
```

```
## Error in eval(expr, envir, enclos): could not find function "PCA"
```

```r
Variance_Explained_Table <- Variance_Explained_Table_results$eig
```

```
## Error in eval(expr, envir, enclos): object 'Variance_Explained_Table_results' not found
```

```r
Variance_Explained_Table_copy <- Variance_Explained_Table
```

```
## Error in eval(expr, envir, enclos): object 'Variance_Explained_Table' not found
```

```r
row = 1:nrow(Variance_Explained_Table)
```

```
## Error in nrow(Variance_Explained_Table): object 'Variance_Explained_Table' not found
```

```r
name <- paste("Component No:", row, sep = "")
```

```
## Error in paste("Component No:", row, sep = ""): cannot coerce type 'closure' to vector of type 'character'
```

```r
Variance_Explained_Table <- cbind(name, Variance_Explained_Table)
```

```
## Error in cbind(name, Variance_Explained_Table): object 'name' not found
```

```r
Variance_Explained_Table <- as.data.frame(Variance_Explained_Table)
```

```
## Error in as.data.frame(Variance_Explained_Table): object 'Variance_Explained_Table' not found
```

```r
colnames(Variance_Explained_Table) <- c("Components", "Eigenvalue", "Percentage_of_explained_variance", 
    "Cumulative_percentage_of_explained_variance")
```

```
## Error in colnames(Variance_Explained_Table) <- c("Components", "Eigenvalue", : object 'Variance_Explained_Table' not found
```

```r
eigenvalues <- Variance_Explained_Table[, 2]
```

```
## Error in eval(expr, envir, enclos): object 'Variance_Explained_Table' not found
```

<br>
Let's look at the **variance explained** as well as the **eigenvalues** (see session readings):
<br>
<br>



















































































