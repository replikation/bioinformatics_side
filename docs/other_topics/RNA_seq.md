# RNAseq
## Overview
* identifying novel transcripts and splicing variants
* Measuring gene expression (cellular identity, expression in response to stimuli, differences between health/sick, correlation to diseases, etc.)
* microRNA's, RNA editing
+ low background noise, high technical reproducibility

### Typical RNAseq workflow
**Questions:**

*Quantification*: Expression rates

*Splicing variants*: exon arrangements

*Novel transcripts*

*RNA editing*: single nucleotide variations (SNV)

**Practical:**
 1. poly-A RNA extraction (mRNA extraction Kit)
 2. reverse transcription into c-DNA
 3. shearing (for illumina, not for nanopore)
 4. library prep (depends on which seq. technique)
 5. sequencing and getting the reads (fastq)

**Bioinformatics:**

 1. **Preprocessing** fastq files using: `Trimmomatic, cutadapt, trimgalore`
    1. adapter removal
    2. trimming of bad quality
 2. **Mapping**
     1. no genome: *de novo* transcriptom construction
     2. known genome: align reads against it
         1. *Quantification*: accept only known/described transcripts
         2. *Splicing variants*: allow alternate arrangements of known transcripts
         3. *Novel transcripts*: allow alignment to exons and other regions
 3. **Mapping QC** with `RNA-SeQC`, discard reads if:
    1. cannot be uniquely mapped
    2. alignment overlaps with several genes
    3. bad alignment quality score
    4. paired-end reads (illumina) do not map to the same genes
 4. **Expression Quantification** (normalization of read counts)
    1. correct read length bias (longer genes produce more reads)
    2. correct Batch effects (labs and preparation time - different coverage between samples)
    3. correct technical biases (use of different machines)

### Types of normalization
You need to know:
* How was RNA extracted and the library build
* Which Sequencing platform
* single or basepaired ends?
* How many reads per sample?
* QC reports?

Normalization for: **library size**, **gene length**, **RNA composition** (comparison between samples)

* Methods:
    * RPKM/FPKM (size and length)
        * not recommended if you compare between different samples
        * **R**eads **p**er **k**ilo base per **m**illion mapped reads (RPKM)
        * use only to report expression values NOT differential expression
    * TPM (size and length)
        * similar to RPKM but uses **t**ranscripts instead of reads
        * the presenter doesn't use this method
    * TMM and DESeq (size and composition)
        * TMM considers the tissue type expression variation
        * suitable for differential expression between sample groups

**Data visualization is done with MA-plot or vulcano plot**(check the tutorial afterwards)

> ![](https://raw.githubusercontent.com/wiki/trinityrnaseq/trinityrnaseq/images/MA_and_volcano_plot.png)
Figure: Black is not significant, red are significant changes
* heatmaps is the most common representation for differential expression analysis (R/Bioconducter heatmap and ggplot package)
* Another way to see how samples cluster is with PCA plots

Function enrichment analysis:
* GOSeq/topGO/GAGE (R package)
* [DAVID](https://david.ncifcrf.gov/)

## Differential Expression - practical
For this tutorial we used the test data from [this paper](http://journals.plos.org/ploscompbiol/article?id=10.1371/journal.pcbi.1004393). This is a highly comprehensive tutorial paper for RNAseq.

**Summary:**

>* We have two **two commercially available RNA samples**:
>   * Universal Human Reference (UHR)
>       * 10 cancer cell lines
>   * Human Brain Reference (HBR)
>       * isolated from brains of 23 Caucasians (m & f) mostly 60-80 years old
> * **a spike-in control was added to each sample** (ERCC ExFold RNA). **Spike-in consists of 92 transcripts** that are present in known concentrations across a wide abundance range (from very few copies to many copies). This **allows to test** the degree to which the **RNA-seq assay** (including all laboratory and analysis steps) accurately **reflects the relative abundance** of transcript species within a sample
>
> * **two 'mixes' of spike-in control** to allow assessment of differential expression output between samples
>  * **3 complete experimental replicates** for each sample to assess the technical variability of our overall process of producing RNA-seq data in the lab
>
>-   UHR + ERCC Spike-In Mix1, Replicate 1
>-   UHR + ERCC Spike-In Mix1, Replicate 2
>-   UHR + ERCC Spike-In Mix1, Replicate 3
>-   HBR + ERCC Spike-In Mix2, Replicate 1
>-   HBR + ERCC Spike-In Mix2, Replicate 2
>-   HBR + ERCC Spike-In Mix2, Replicate 3
> * For technical details, like Kits, rRNA removal, sample concentrations etc. read the [S2 Data](https://doi.org/10.1371/journal.pcbi.1004393.s002) file from the paper.

Data available at:
```bash
curl -O -J -L https://osf.io/7zepj/download
tar xzf toy_rna.tar.gz
```
### `salmon` for indexing and quantification of reads
* Salmon produces highly-accurate, transcript-level quantification estimates from RNA-seq data (you need another - splice aware program - if you are interested in new transcripts or splice sides.

**Quantify reads with `salmon`**

* First Indexing:

```bash
salmon index -t chr22_transcripts.fa -i chr22_index
```
* Now quantifying with this loop:
* simply goes through each sample and invokes salmon using basic options:
* `-i` index file from the command before
* `--libType` A tells salmon that it should automatically determine the library type of the sequencing reads (e.g. stranded vs. not stranded etc.)
* the -1 and -2 arguments tell salmon where to find the left and right reads for this sample (salmon accepts gzipped FASTQ files)
* `-o` output

```bash
for i in *_R1.fastq.gz
do
   prefix=$(basename $i _R1.fastq.gz)
   salmon quant -i chr22_index --libType A \
          -1 ${prefix}_R1.fastq.gz -2 ${prefix}_R2.fastq.gz -o quant/${prefix};
done
```
* in the output folder (quant folder) is the quant.sf file we use for Rstudio.

### R-studio analysis
* In R-studio set your working directory to the data (chr22_genes.gtf is visible)
* load the necessary modules

```R
library(tximport)
library(GenomicFeatures)
library(readr)
```
* Salmon did the quantification of the transcript level. We want to see which genes are differentially expressed, so we need to link the transcript names to the gene names. We can use our .gtf annotation for that, and the GenomicFeatures package:

```R
txdb <- makeTxDbFromGFF("chr22_genes.gtf")
k <- keys(txdb, keytype = "TXNAME")
tx2gene <- select(txdb, keys = k, keytype = "TXNAME", columns = "GENEID")
```
* Now we can import the salmon quantification.

```R
# we imported a file were each sample name is stored under the column "sample"
    samples <- read.table("samples.txt", header = TRUE)
# we save the path to each quant.sf file using the name of each sample in the samples file
    files <- file.path("quant", samples$sample, "quant.sf")
# we renamed the columns name of each path we extracted
    names(files) <- paste0(samples$sample)
# imports the quant files, txi.salmon is a datalist e.g. salmon$counts for the counts list-entry
    txi.salmon <- tximport(files, type = "salmon", tx2gene = tx2gene)
```
**Differential expression using DESeq2**
+ `DESeqDataSet` is a subclass of `RangedSummarizedExperiment`, used to store the input values, intermediate calculations and results of an **analysis of differential expression**.

```R
library(DESeq2) #loading module
```
* Instantiate the DESeqDataSet and generate result table. See `?DESeqDataSetFromTximport` and `?DESeq` for more information.

```R
dds <- DESeqDataSetFromTximport(txi.salmon, samples, ~condition)
dds <- DESeq(dds)
res <- results(dds) #summary(res) to see the results
```
* DESeq uses a negative binomial distribution. Such distributions have two parameters: **mean** and **dispersion**.
* So we can do a dispersion plot with the dispersion data:

```R
plotDispEsts(dds, main="Dispersion plot")
```
* Explanations about dispersion and DESeq2 can be found in this very good tutorial [here](https://hbctraining.github.io/DGE_workshop/lessons/04_DGE_DESeq2_analysis.html).

>![](https://i.imgur.com/4q4cmxH.png)
>Figure:  The red line in the figure plots the estimate for the **expected dispersion value for genes of a given expression strength**. Each black dot is a gene with an associated mean expression level and maximum likelihood estimation (MLE) of the dispersion.
___
**Heatmap**
* For clustering and heatmaps, we need to log transform our data:

```R
rld <- rlogTransformation(dds) #log of data from "dds"
#loading librarys for plot
library(RColorBrewer)
library(gplots)
```
* parameters of the heatmap:

```R
(mycols <- brewer.pal(8, "Dark2")[1:length(unique(samples$condition))])
sampleDists <- as.matrix(dist(t(assay(rld))))
heatmap.2(as.matrix(sampleDists), key=F, trace="none",
          col=colorpanel(100, "black", "white"),
          ColSideColors=mycols[samples$condition],
          RowSideColors=mycols[samples$condition],
          margin=c(10, 10), main="Sample Distance Matrix")
```
>![](https://i.imgur.com/i6NEzsu.png)
>Figure: Differential Expression Analysis: Comparison between each samples as a heatmap, black is identical, white not identical
___
**PCA Plot**
```R
DESeq2::plotPCA(rld, intgroup="condition")
```
![](https://i.imgur.com/HOizOAe.png)
___
**MA plot and the Volcano Plot**

* we create, merge and sort data, so its readable for the plots

```R
table(res$padj<0.05) #extract data out of res
res <- res[order(res$padj), ] #sorting the res variable
#merging data so its ready for plots
resdata <- merge(as.data.frame(res), as.data.frame(counts(dds, normalized=TRUE)), by="row.names", sort=FALSE)
names(resdata)[1] <- "Gene"
```
* You can take a look at the p-values we sorted in the variable `res`, using these plots:

```R
hist(res$pvalue, breaks=50, col="grey") #a hist plot
DESeq2::plotMA(dds, ylim=c(-1,1), cex=1) #an MA plot
```
* now we do the actual vulcano plot

```R
# Volcano plot
with(res, plot(log2FoldChange, -log10(pvalue), pch=20, main="Volcano plot", xlim=c(-2.5,2)))
# second one highlighting certain points
with(subset(res, padj<.05 ), points(log2FoldChange, -log10(pvalue), pch=20, col="red"))
```
___
### KEGG pathway analysis
**KEGG PATHWAY** is a collection of manually drawn [pathway maps](http://www.genome.jp/kegg/kegg3a.html) representing our knowledge on the molecular interaction, reaction and relation networks for:
* We need these library's:

```R
library(AnnotationDbi)
library(org.Hs.eg.db)
library(pathview)
library(gage)
library(gageData)
```
* Using the `mapIds` function to add more columns to the results
* the row.names of our results table has the Ensembl gene ID (our key), so we need to specify `keytype=ENSEMBL`
* the column argument tells the `mapIds` function which information we want
* the `multiVals` argument tells the function what to do if there are multiple possible values for a single input value (we take only the first entry if we have more than two)

```R
res$symbol <- mapIds(org.Hs.eg.db, keys=row.names(res), column="SYMBOL", keytype="ENSEMBL", multiVals="first")
res$entrez <- mapIds(org.Hs.eg.db, keys=row.names(res), column="ENTREZID", keytype="ENSEMBL", multiVals="first")
res$name <- mapIds(org.Hs.eg.db, keys=row.names(res), column="GENENAME", keytype="ENSEMBL", multiVals="first")
```
* We’re going to use the [gage](https://bioconductor.org/packages/release/bioc/html/gage.html) package for pathway analysis, and the [pathview](https://bioconductor.org/packages/release/bioc/html/pathview.html) package to draw a pathway diagram.
* The gageData package has pre-compiled databases mapping genes to KEGG pathways and GO terms for common organisms:

```R
data(kegg.sets.hs)
data(sigmet.idx.hs)
kegg.sets.hs <- kegg.sets.hs[sigmet.idx.hs]
head(kegg.sets.hs, 3)
```
* Run the pathway analysis

```R
foldchanges <- res$log2FoldChange
names(foldchanges) <- res$entrez
keggres <- gage(foldchanges, gsets=kegg.sets.hs, same.dir=TRUE)
lapply(keggres, head)
```
* Now we need to identify which pathway we have and whats the ID of it:

```R
library(dplyr)
# Get the pathways
keggrespathways <- data.frame(id=rownames(keggres$greater), keggres$greater) %>%
  tbl_df() %>%
  filter(row_number()<=5) %>%
  .$id %>%
  as.character()
keggrespathways
# Get the IDs.
keggresids <- substr(keggrespathways, start=1, stop=8)
```

* Finally, the `pathview()` function in the pathview package makes the plots. Let’s write a function so we can loop through and draw plots for the top 5 pathways we created above.

```R
# Define plotting function for applying later
    plot_pathway <- function(pid) pathview(gene.data=foldchanges, pathway.id=pid, species="hsa", new.signature=FALSE)

# Unload dplyr since it conflicts with the next line
    detach("package:dplyr", unload=T)
# plot multiple pathways (plots saved to disk andreturns a throwaway list object)
    tmp <- sapply(keggresids, function(pid) pathview(gene.data=foldchanges, pathway.id=pid, species="hsa"))
```
**The results are shown here (but only for 2 pathways and only the KEGG output):**

>![](https://i.imgur.com/5MJxT3b.png)
>![](https://i.imgur.com/6jWXmGt.png)

Another tutorial about this pathway stuff can be found [here](http://www.gettinggeneticsdone.com/2015/12/tutorial-rna-seq-differential.html).
