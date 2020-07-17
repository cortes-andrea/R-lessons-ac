##Lesson 2: Dealing with Data Frames

**Load Data**: ``` data(“name”) ```


**Idea of Table**:

structure, names, data:
```
str(name)
```

table: ``` View(name) ```

first few rows: ```head(name)```

number of rows: ```nrow(name)```

number of columns (vector): ```ncol(name)```

column names: ```colnames(name)```

row names (numeric or character): ```rowname(name)```


**Changing Names:**

```
colnames(name)[#] <- “new name” - change name of # column
```

*Transposing*

columns by rows to rows by columns: ```t(name)```

-not saved unless in a vector in whole lesson

Access specific cell in a table: ```data[row#, column#]```

*Sorting*

```
name[order(name$Petal.length, decreasing=TRUE),]

name[order(name$Species, name$Petal.length),]

name[order(name$Species, -name$Petal.length),] - decreasing
```
*Filtering*

```
name[name$Petal.length>7,]

name[name$Species==”setosa”,]
		          !=
```

*Summing by row/column*

```
colSums(name[, -ncol*(name)])
* excluding last column with names, not numbers
colSums(name[, -which(colnames(iris)==”Species”)]) - same as top
rowSums(name[, -ncol(name)])
```

Mean value per column: ``` colMeans(name[, -col(name)]) ```


###OTUs
OTU table into a variable:
```
OTUs <- read.csv(“OTUtable.csv”)
```

OTU ID, samples, taxonomy: ```str(OTUs)```


Converting to Relative Abundance:

```
Vec <- colSums(OTUs[, -c(1, ncol(OTUs))]
- excluding OTU ID and Taxonomy

relabun <- OTUs

for (r in 1:nrow(relabun)) {
	relabun[r, -c(1, ncol(relabun))] <- relabun(r, -c(1, ncol(relabun))]/vec
}

Relabun[, -c(1, ncol(relabun))] <- relabun[, -c(1, ncol(relabun))] * 100 - to go into percentages

```

*Same with apply*

```
relabun <- OTUs (starting over)
divByColsum <- function(x) {x/vec}
relabun <- apply(OTUs[, -c(1, ncol(OTUs))], 1*, divByColsum)
* 1 = row by row, 2 = column by column

relabun <- t(apply(OTUs[, -c(1, ncol(OTUs))], 1*, divByColsum)
- This is a matrix

relabun <- as.data.frame(t(apply(OTUs[, -c(1, ncol(OTUs))], 1, divByColsum)))
- This is a data frame
- Doesn’t have OTU ID or Taxonomy

relabun2 <- merge(relabun, OTUs, by=”row.names”)
- Now has OTU and Taxonomy AND counts

relabun2 <- merge(relabun, OTUs[, c(1, ncol(OTUs))], by-”row.names”)
- Only Including OTUs and taxonomy

relabun2 <- relabun2[, -1]
- [, -1] throws out first column of row numbers
- [-1,] throws out first row
- number can be variable

Or...

relabun2$Row.names <- NULL
- no row names


relabun3 <- relabun2[, -ncol(relabun2)]
- OTU ID and samples abundance into numeric rather than string

relabun4 <- relabun2[, c(ncol(relabun2)-1, ncol(relabun2))]
- OTU ID and taxonomy into numeric rather than string

relabun5 <- merge(relabun3, 43labun4, by=”OTU_ID”)

---------------------------------------------------------------------------

* IF different names:
colnames(relabun4)[1] <- “George”
relabun5 <- merge(relabun3, relabun4, by.x = “OTU_ID”, by.y=”George”)

```

