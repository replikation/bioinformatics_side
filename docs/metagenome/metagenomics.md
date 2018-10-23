# Metagenomics
## Overview & Terminology

+ collected genomic information of a sample
+ unbiased targeting (no special marker like metabarcoding)
+ Metabarcoding (16S) is used for large number of samples, i.e., multiple patients, longitudinal studies, etc. but offers limited taxonomical and functional resolution in comparision
* **Whole Metagenome sequencing (WMS)**, or shotgun metagenome sequencing, provides insight into community biodiversity and function in contrast to Metabarcoding, where only a specific region of the bacterial community (the 16s rRNA) is sequenced
 *  **WMS** aims at sequencing all the genomic material present in the environment
* **WMS** is more expensive but offers increased resolution, and **allows the discovery of viruses as well as other mobile genetic elements**

**Microbiota**
* organism within a set environment
* mainly focused on bacteria
* exluding viruses & mobile elements

**Microbiome**
* includes all factors of an environment
* (organism, genetic elements, metabolites, proteins)

**Holobiont**
* assemblages of different species that form a ecological unit
* divided into: virome, microbiome and macrobiological hosts

## Whole Metagenome Sequencing - practical

* comparing samples from the Pig Gut Microbiome against the Human Gut Microbiome

Software used:
-   [FastQC](http://www.bioinformatics.babraham.ac.uk/projects/fastqc/)
-   [Kraken](https://ccb.jhu.edu/software/kraken/)
-   [R](https://www.r-project.org/)
-   [Pavian](https://github.com/fbreitwieser/pavian)

Data used:
```bash
curl -O -J -L https://osf.io/h9x6e/download
tar xvf subset_wms.tar.gz
```
* we `ssh` using additionally the `-X` flag to allow a graphical interface
* we used `fastqc` do check the quality of our data

### 1. Taxonomic read assignment with `Kraken`
* first we need the Kraken database

```bash
#create a databases directory and download the database
wget https://ccb.jhu.edu/software/kraken/dl/minikraken_20171019_4GB.tgz
tar xzf minikraken_20171019_4GB.tgz
```
* running `kraken` on the reads
* **produces a tab-delimited file with an assigned TaxID for each read**

```bash
# for the script you need to declare the KRAKEN_DB path
KRAKEN_DB="/mnt/databases/minikraken_20171013_4GB"
for i in *_1.fastq
do
    prefix=$(basename $i _1.fastq)
    # print which sample is being processed
    echo $prefix
    kraken --db $KRAKEN_DB --threads 2 --fastq-input ${prefix}_1_corr.fastq ${prefix}_2_corr.fastq > /home/student/wms/results/${prefix}.tab
    kraken-report --db $KRAKEN_DB /home/student/wms/results/${prefix}.tab > /home/student/wms/results/${prefix}_tax.txt
done
```
### 2. Visualization with Pavian in R
* Installing and run Pavian in R with those commands:

```R
#one time only installations and setup
options(repos = c(CRAN = "http://cran.rstudio.com"))
if (!require(remotes)) { install.packages("remotes") }
remotes::install_github("fbreitwieser/pavian")
#Start up command
pavian::runApp(port=5000)
```
* creates a new R-tab were you can open the kraken files
* creates a nice sankey chart of your organism within your sample
* its scalable, allows comparision between samples (as a table) and indepth analysis of samples (see figure)

> Example results in one sample as a sankey chart using pavian
> ![](https://i.imgur.com/Lmvswjo.png)

* KRONA chart is also for visualization of metagenomic data
* good for explanation of your sample
* but PAVIAN is way better

## Metagenome assembly and binning - practical

AIM: inspect and assemble metagenomic data and retrieve draft genomes

* using a mock community of 20 bacteria simulating Illumina HiSeq created with [InSilicoSeq](http://insilicoseq.readthedocs.io)
* selected from the [Tara Ocean study](http://ocean-microbiome.embl.de/companion.html) that recovered 957 distinct Metagenome-assembled-genomes (or MAGs) that were previsouly unknown

### 1. Correction and assembly of the illumina reads
* check the fastq data with `fastqc`
* do `sickle` and `scythe` as needed; see [HTS](../sequencing/illumina.md)
* assembly here with `megahit`

```bash
sickle pe -f tara_reads_R1.fastq.gz -r tara_reads_R2.fastq.gz -t sanger -o tara_trimmed_R1.fastq -p tara_trimmed_R2.fastq -s /dev/null
megahit -1 tara_trimmed_R1.fastq -2 tara_trimmed_R2.fastq -o tara_assembly
```

### 2. Binning
* First map the reads back against the assembly (for coverage information)
* bowtie2 is a ultra fast short read mapper/aligner

```bash
ln -s tara_assembly/final.contigs.fa . #link contigs to current folder
bowtie2-build final.contigs.fa final.contigs #index
bowtie2 -x final.contigs -1 tara_trimmed_R1.fastq -2 tara_trimmed_R2.fastq | samtools view -bS -o tara_to_sort.bam #alignment and bam conversion
samtools sort tara_to_sort.bam -o tara.bam
samtools index tara.bam
```
* then we run `metabat`

```bash
runMetaBat.sh -m 1500 final.contigs.fa tara.bam
mv final.contigs.fa.metabat-bins1500 metabat
```
### 3. QC of the bins
* first time you run `checkm` you have to create the database

```bash
sudo checkm data setRoot ~/.local/data/checkm
```
```bash
checkm lineage_wf -x fa metabat checkm/
checkm bin_qa_plot -x fa checkm metabat plots
```
### 4. Get organism information and plot it
```bash
#tells you which organism, saves under checkm/lineage needs folder checkm
checkm qa checkm/lineage.ms checkm
#plots its
checkm bin_qa_plot -x fa checkm metabat plots
```
* you get something like this, which should represent 10 different species (theoretically):
![](https://i.imgur.com/RG2mmkR.png)
* now you can annotate each bin (fasta files are in metabat dir)
* more informations can be found [here](https://www.nature.com/articles/s41564-017-0012-7) and [here](https://www.nature.com/articles/sdata2017203)

### 5. Barrnap
* **BA**sic **R**apid **R**ibosomal **RNA** **P**redictor
* isnt tsuitable for novel stuff since it has to be in the database, but seems still to work (see figure above of unknown marine bacteria)
* you can download it [here](https://github.com/tseemann/barrnap)
