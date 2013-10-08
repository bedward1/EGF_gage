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
```

```
## Warning: cannot open compressed file
## '/Volumes/Documents-1/R_onAir/EGF_gage
## analysis/tests/init_analysis_output.Rdata', probable reason 'No such file
## or directory'
```

```
## Error: cannot open the connection
```

```r

# make data generic for next steps WARNING: row names and data must be
# correct for the next line

# for this data, need to get rid of bracketint ' ' '
row.names(ws) = sub("'", "", sub("'", "", row.names(ws)))
```

```
## Error: object 'ws' not found
```

```r
data.G1 = as.matrix(ws)
```

```
## Error: object 'ws' not found
```

```r

# identify control and test column for comparison
control.idx = grep("high.ctrl.none", colnames(data.G1), ignore.case = T)
```

```
## Error: object 'data.G1' not found
```

```r
test.idx = grep("high.egf.none", colnames(data.G1), ignore.case = T)
```

```
## Error: object 'data.G1' not found
```

```r
# check
check.control = print(colnames(data.G1)[control.idx])
```

```
## Error: object 'data.G1' not found
```

```r
check.treatment = print(colnames(data.G1)[test.idx])
```

```
## Error: object 'data.G1' not found
```

** gernalized from here on **

#### Gage analysis

```r
# options: called answers
require(gage)
ans.same.dir = F
ans.use.fold = F  # or F is t-statistic
ans.rank.test = F
ans.ref = control.idx
```

```
## Error: object 'control.idx' not found
```

```r
ans.samp = test.idx
```

```
## Error: object 'test.idx' not found
```

```r
ans.saaTest = gs.KSTest  # non-parametric
ans.use.stouffer = T  # p-value normalization method
ans.compare = "unpaired"  # 'paired', 'unpaired', '1ongroup','as.group'

# ****name output file names (describe to identify later )

filename.desc = paste(check.control, check.treatment, ans.same.dir, ans.use.fold, 
    ans.rank.test, ans.use.stouffer, ans.compare, sep = "_")
```

```
## Error: object 'check.control' not found
```

```r
# this makes 2 characters. use only 1 TODO fix

gage.run <- gage(data.G1, gsets = path.lib, ref = ans.ref, samp = ans.samp, 
    same.dir = ans.same.dir, use.fold = ans.use.fold, rank.test = ans.rank.test, 
    saaTest = ans.saaTest, compare = ans.compare, use.stouffer = ans.use.stouffer)
```

```
## Error: object 'data.G1' not found
```

```r

# to select essential genes in group# these are used in KeggVis.Rmd for
# pathway viewing
essential.greater <- esset.grp(gage.run$greater, data.G1, gsets = path.lib, 
    ref = ans.ref, samp = ans.samp, output = F, make.plot = F, compare = ans.compare, 
    test4up = T, samedir = ans.same.dir, use.fold = ans.use.fold)
```

```
## Error: object 'data.G1' not found
```

```r

essential.less <- esset.grp(gage.run$less, data.G1, gsets = path.lib, ref = ans.ref, 
    samp = ans.samp, output = F, make.plot = F, compare = ans.compare, test4up = T, 
    samedir = ans.same.dir, use.fold = ans.use.fold)
```

```
## Error: object 'data.G1' not found
```

### output  
1)"gage.run" has the tables 'greater' and 'less', and 'stats' that show the diff pathways    
2) essential.greater and essential.less have the pathways that are regulated higher (greater) or lower (less) than controls #TODO figure this out    
  a) use these as inputs to Kegg.Vis if kegg is use for pathway analysis. TODO, map to other pathways types?   
  b) each of these has lists that are the core and essential gene groups (see Vignette)      
3) output this as a table

```r
# change location as necessary for project TODO automate
write.table(rbind(gage.run$greater, gage.run$less), file = paste("/Volumes/Documents-1/R_onAir/EGF_gage analysis/tests/", 
    filename.desc[1], ".xls", sep = ""), sep = "\t")
```

```
## Error: object 'gage.run' not found
```


#save to allow load for next steps

```r
save.image("/Volumes/Documents-1/R_onAir/EGF_gage analysis/tests/gageS1_output.Rdata ")
```



