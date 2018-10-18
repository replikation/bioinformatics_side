# GWAS (Genome wide association study)
## QTL (quantitative trait locus)
+ More than one gene is responsible for certain traits
+ genes can interact **between** loci (epistatic effects) or **within** the loci (dominance effects)
* **QTL** is a section of DNA (locus) which correlates with a variation in a phenotype
* it is linked to, or contains, the genes which control that phenotype
* QTLs are mapped by identifying which SNPs correlate with an observed trait
* one QTL can affect multiple traits, and can be dominant or recessive

### How to identify QTLs
|1. QTL/Linkage Mapping|2. GWAS (newer)|
|-|-|
|study pop. is families or experimental crosses|uses a population sample
|hundreds of DNA markers|Tens of thousands DNA marker
|Current recombinations|historical recombinations

**Linkage disequilibrium (LD)**
(non random association of allels between locis, LD "decays" - the faster the decay the higher the marker density has to be)

**Pro**: can scan genome with fewer markers

**Cons**: Can only detect alleles with large effect; limited resolution (identify broad region, not individual genes); requires data on multiple family members

**Association (GWAS)**

**Pros**: can detect subtle effects; very fine resolution

**Cons**: requires 0.5 to 1 million markers to cover whole genome; requires large sample size

#### 1. QTL/Linkage Mapping (inferior to GWAS)
* Determining the location of a gene in the genome
* Estimating the effects of the alleles and mode of action
* Individuals inherit different marker variants
 * Individuals vary according to their marker genotype
* You have to know what you are looking for (e. g. certain diseases)
* Costly since you have to analyze each generation (good for disease reconstructions)
* The QTL has to be inferred from knowledge and prediction (e. g. color of the fur/hide)

For QTL Linkage Mapping you need:
> * Well recorded population with high quality DNA samples
> * DNA Markers across the genome
> * Map of Marker positions
> * High quality genotype

#### 2. GWAS (best method)
+ In genetics, a **genome-wide association study** (**GWAS**), is an observational study of a genome-wide set of genetic variants in different individuals to see if ==any variant is associated with a trait==
+ GWASs typically focus on associations between SNPs and traits (e.g. SNPs associated with milk production)
a) Take a large (thousands), representative, sample of the population
b) Characterised for a very large number of DNA variants
c) Estimate a putative effect of every DNA variant on the trait of interest

## QC of SNP Genotyping
### For samples
* Blind duplicates
* genders
* unsuspected twinning or cryptic relatedness
* DNA degradation or fragmentation
* Call rate (> 80-90%)
* Heterozygosity: outliers, Plate/batch calling effects

### For SNPs
* Duplicate concordance (CEPH samples)
* Mendelian errors (typically < 1)
* Hardy-Weinberg errors (often > 10-5 )
* Heterozygosity (outliers)
* Call rate (typically > 98%)
* Minor allele frequency (often > 1%)
* Validation of most critical results on independent genotyping platform

**Association analysis is often quickest way to find genotyping errors (PLINK)**

## Signifikant SNP results and QC
* signifikant results in an manhatten plot or a Q-Q plot
* A significant marker associated with a QT does not imply a causative **quantitative trait nucleotide** (QTN)
* significant result indicates:
    * The marker is in linkage disequilibrium with a QTN
    * false positive result: by chance or due to **Population stratification**
* positive results have to be backed up with functional data and/or a replication study in a different population

### Problems that bias you results (confounding factors)
* SNPs can correlate with latent variables
* the variables can have an effect!
* SNP then gives **false positive result**
* Examples:
    * Population structure Admixture
    * breed or even family
    * Geographical effects
    * Batch effects

**Solutions:**
a) Genomic control - inflating factor to control p values
b) Structure - estimate a population structure
c) Principle components - genome wide IBS matrix (in Plink its the `--cluster` command)

## PLINK (Software) - practical
* whole genome wide association (GWAS) analysis tool
* Install by google PLINK and get the precompiled *.zip file. `mv plink ~/.local/bin`, done

**The workflow is usually:**
 1. Plan the study (choose design to target your hypothesis)
 2. Collect data (based on design, number of markers)
 3. Remove problematic data
 4. Identify other pedigree related problems (population stratification)
 5. Association analysis
 6. Correction of results to minimize false positives
 7. further validation of the region (e. g. identify possible genes)

### Correction and binary file creation
#### Without phenotype file
* using `.ped` and `.map` files
    * `--ped wolf.ped` and `--map wolf.map`
    * `.ped` contains Family-, Individual-, Paternal-, Maternal-ID, Sex and phenotype
    * `.map` contains chromosome, rs# or snp identifier, Genetic distance (morgans) and bp position
* we correct the individuals and SNPs with the flags: `--geno 0.25 --maf 0.05 --mind 0.25`
* we use the `--dog` flag (or `--cow`)
* `--out` specifies outputname, ==NEVER CHANGE IT== unless written and highlighted here
* we save the data also in binary using the `--make-bed`; this creates 3 files:
    * `wolf.bed`, `wolf.fam` and `wolf.bin`
    * these files are now used with `--bfile wolf` so `--ped, --map, --geno, --maf, --mind` fall away

```
plink --ped wolf.ped --map wolf.map --out wolf --geno 0.25 --maf 0.05 --mind 0.25 --dog --noweb --allow-no-sex --make-bed
```
* this creates a log file: `wolf.log`
    * states amount of ==individuals== and how many excluded
    * states how many ==SNPs== are included/excluded

### With phenotype file
* same command and usage as above, but we add the flag `--no-pheno` and the flag/file `--pheno cow.phe` to the command
* example with cow data:

```bash
plink --ped cow.ped --map cow.map --out cow --geno 0.25 --maf 0.05 --mind 0.25 --cow --noweb --allow-no-sex --make-bed --no-pheno --pheno cow.phe
```

### Various analysises
`--freq` - Allel frequency, creates a `.frq` file
`--missing` -
`--hardy` -
`--asso` -
* add a flag to this basic command:

```bash
plink --bfile wolf --out wolf --dog --noweb --allow-no-sex
```
### Population stratification
* used to check if you may have more than one population (pedigree related problems), details [here](https://www.cog-genomics.org/plink/1.9/strat#mds_plot)
* creating a 2D-MDS plot (more under **09 - "β diversity analysis"**)

```bash
 plink --bfile wolf --out wolf --dog --noweb --allow-no-sex --cluster --mds-plot 2
```
**Plotting in excel, we see 3 distinct populations:**
![](https://i.imgur.com/0RJdPBR.png)

### Basic reports, case/control phenotype
* for this to work you need the additional phenotype datafile `.phe`
* see also [here](https://www.cog-genomics.org/plink/1.9/assoc#model)
* first correcting the results (p-values) for multiple testing with 10,000 permutations which creates an `.assoc` and `.assoc.mperm` file:

```
plink --bfile cow --allow-no-sex --cow --out cow --assoc --noweb --mperm 10000 --no-pheno --pheno cow.phe
```
* adjustment of results (e. g. Bonferroni adjusted significance value)
* **change the `--out` to something else**

```bash
plink --bfile cow --allow-no-sex --cow --out cow2 --assoc --noweb --adjust
```
* use the new file `cow2.qassoc` for plotting

### Ploting the data
* Using manhatten plot, install with `install.packages("qqman")`
* the `sep=""` of read.table threats multiple space as one

```R
library(qqman)
x <- read.table("cow2.qassoc", sep="", header = TRUE)
manhattan(x, chr="CHR", bp="BP", snp="SNP", p="P")
```
* to highlight SNPs of interest add `, highlight= y` to the manhatten command, y is this:

```R
y <- c("rs3001", "SNP205", "rs3003")
```
* search via excel the `.qassoc` for SNPs of interest (closer to 0 the more significant so smaller than 1e⁻⁷), see line above blue in the plot

**Results:**
* check [this](http://www.gettinggeneticsdone.com/2014/05/qqman-r-package-for-qq-and-manhattan-plots-for-gwas-results.html) out for how to get GWAS into a plot or this figure
![](https://i.imgur.com/dMvRGGi.png)
