---
title: 猪的exon有多大(R版本)
date: 2017-09-26 16:51:11
tags:
---


# How big are pig exons?

<!--more-->

当我坐在办公室，我需要研究一下猪的exon的信息。那么猪的exon有多大？我们可以用R轻松的实现：(请保存Rstudio中的其他信息防止R崩溃)

```R
# load the biomaRt library
# install it if it doesn't exist
# source("http://www.bioconductor.org/biocLite.R")
# biocLite("biomaRt")
library(biomaRt)

# get data from Ensembl
mart <- useMart("ensembl")

# we are interested in the S scrofa (pig) genome
mart <- useDataset("sscrofa_gene_ensembl",mart)

# set the attributes we want and fetch them
myatt <- c("ensembl_transcript_id", "chromosome_name","ensembl_exon_id", "exon_chrom_start", "exon_chrom_end")
mydat <- getBM(myatt, mart=mart, uniqueRows=TRUE)

# calculate the exon length
mydat$el <- mydat$exon_chrom_end - mydat$exon_chrom_start + 1

# get a unique set of exons
exons <- unique(mydat[,c("ensembl_exon_id","exon_chrom_start","exon_chrom_end","el")])

# plot
hist(exons$el, breaks=10000, col="blue", border="blue", main="Histogram of S scrofa exon lengths", xlab="length")

# how long in Mb?

sum(exons$el)/1000000
```

