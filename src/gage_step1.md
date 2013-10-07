Gage pipeline, Step I- calcs
========================================================
Started: Bryan Linggi October 2, 2013  
Updated:  
Input: Preprocessed data. 'Preproc.Rdata' from 'diagnostics'  
Output: results of gage      

[link to Gage manuscript](http://www.biomedcentral.com/1471-2105/10/161) 


### Get pathway library   
if is new pathway library, make in separate script and test

```r
setwd("/Volumes/Documents-1/R_onAir/EGF_gage analysis/")

require("gage")
```

```
## Loading required package: gage
```

```r

data(go.gs)
data(kegg.gs)
data(egSymb)
# make library generic for next steps
path.lib = kegg.gs  #kegg.gs
# convert entrez to sym

path.lib = lapply(path.lib, eg2sym)
```


### Import data from preproccessing folder

```r
load("/Volumes/Documents-1/R_onAir/EGF_gage analysis/tests/init_analysis_output.Rdata")

# make data generic for next steps WARNING: row names and data must be
# correct for the next line

# for this data, need to get rid of bracketint ' ' '
row.names(ws) = sub("'", "", sub("'", "", row.names(ws)))
data.G1 = as.matrix(ws)

# identify control and test column for comparison
control.idx = grep("low.ctrl.none", colnames(data.G1), ignore.case = T)
test.idx = grep("low.egf.none", colnames(data.G1), ignore.case = T)
# check
check.control = print(colnames(data.G1)[control.idx])
```

```
## [1] "low.ctrl.none.1" "low.ctrl.none"
```

```r
check.treatment = print(colnames(data.G1)[test.idx])
```

```
## [1] "low.egf.none"   "low.egf.none.1"
```

** gernalized from here on **

#### Gage analysis

```r
# options: called answers
ans.same.dir = T
ans.use.fold = T  # or F is t-statistic
ans.rank.test = F
ans.ref = control.idx
ans.samp = test.idx
ans.saaTest = gs.KSTest  # non-parametric
ans.use.stouffer = T  # p-value normalization method
ans.compare = "unpaired"  # 'paired', 'unpaired', '1ongroup','as.group'

analysis.1 <- gage(data.G1, gsets = path.lib, ref = ans.ref, samp = ans.samp, 
    same.dir = ans.same.dir, use.fold = ans.use.fold, rank.test = ans.rank.test, 
    saaTest = ans.saaTest, compare = ans.compare, use.stouffer = ans.use.stouffer)
```

### output
#### text output to /report location. change name as necessary


```r
# output primary analysis data results to file, TODO, create labels

write.table(rbind(analysis.1$greater, analysis.1$less), file = "/Volumes/Documents-1/R_onAir/EGF_gage analysis/tests/samedir_fold_unpaird_egf.xls", 
    sep = "\t")
```


#save to allow load for next steps

```r
save.image("/Volumes/Documents-1/R_onAir/EGF_gage analysis/tests/gageS1_output.Rdata ")
```



