Gage pipeline, Step I- calcs
========================================================
Started: Bryan Linggi October 2, 2013  
Updated:  
Input: Preprocessed data. 'Preproccessing.Rdata' from 'munge' fold  
Output: results of gage      

[link to Gage manuscript](http://www.biomedcentral.com/1471-2105/10/161) 



### Get pathway library   
if is new pathway library, make in separate script and test

```r
setwd("~/Documents/R_onAir/GageAnalysis/")
data(kegg.gs)
# make library generic for next steps
path.lib = kegg.gs
```


### Import data

```r
load("~/Documents/R_onAir/GageAnalysis/munge/Preprocessed.Rdata")
# make data generic for next steps WARNING: row names and data must be
# correct for the next line
data.G1 = as.matrix(ws[, 2:13])
# identify control and test column for comparison

control.idx = grep("HN", colnames(data.G1), ignore.case = T)
test.idx = grep("DCIS", colnames(data.G1), ignore.case = T)
# check
check.control = print(colnames(data.G1[control.idx]))
```

```
## NULL
```

```r
check.treatment = print(colnames(data.G1[test.idx]))
```

```
## NULL
```

** gernalized from here on **

#### Gage analysis

```r
# options:
ans.same.dir = T
ans.use.fold = T
ans.rank.test = T
ref.ans = control.idx
samp.ans = test.idx
saaTest.ans = gs.KSTest  # non-parametric
compare.ans = "unpaired"  # or 'paried'
use.stouffer.ans = T  # p-value normalization method

analysis.1 <- gage(data.G1, gsets = kegg.gs, ref = ref.ans, samp = samp.ans, 
    same.dir = ans.same.dir, use.fold = ans.use.fold, rank.test = ans.rank.test, 
    saaTest = saaTest.ans, compare = compare.ans, use.stouffer = use.stouffer.ans)
```

### output
#### text output to /report location. change name as necessary



#### heatmap


