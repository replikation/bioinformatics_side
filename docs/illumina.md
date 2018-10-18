# Illumina - practical

* example workflow with a few steps included
* part of the SLU bioinformatics course
* **in general always check the `--help` for each program**

## 1. Cleaning up Reads (Illumina)
* `fastqc` gives you an overview whats may be wrong with your reads

>N50 is defined as the minimum contig length needed to cover 50% of the genome. It means, half of the genome sequence is in contigs larger than or equal the N50 contig size

>PHRED quality score: gives the probability of an incorrect base call for each base. Score 10 = 1 error in 10 bases. Score 20 = 1 error in 100. Score 30 = 1 in 1000 and so on. It's encoded from 0 to 93 as ASCII symbols in the fastq format.
* `scythe` removes adapters and or barcodes, or remove the "overrepresented sequence" that fastqc is reporting
  * [Download Illumina adapter files here](https://support.illumina.com/downloads/illumina-customer-sequence-letter.html)
* `sickle` for removal of low quality sequences; `-s /dev/null` moves singleton files to trash
* `multiqc` takes all fastqc reports in this directory and creates and combined html file

```bash
fastqc <reads.fastq>

scythe -a <adapters.fasta> <reads.fastq>

sickle pe -f <fileinputfwd> -r <fileinputrev> -o <outputfwd> -p <outpudrev> -t sanger -s /dev/null -q 25

fastqc <trimmed_reads.fastq>

multiqc .
```

## 2.a `megahit` assembly
```bash
megahit -1 fwd.fastq.gz -2 rwd.fastq.gz -o m_genitalium
```
## 2.b or `spades` assembly
```bash
spades.py -k 21,33,55,77 --careful --only-assembler -1 <fwd read>.fastq.gz -2 <rws reads>.fastq.gz -o spade_output -m 16
```
### Quality control with `quast`
    quast.py m_genitalium.fasta -o m_genitalium_report

## 3. Index creation with `bowtie` and  `bowtie`/`samtool` pipe
```bash
bowtie2-build *ASSEMBLYFILE* *m_genitalium123*
bowtie2 -x *m_genitalium123* -1 ERR486840_1.fastq.gz -2 ERR486840_2.fastq.gz | samtools view -bS -o m_genitalium.bam
```
* `-x` is the foldername (created with the first bowtie2 command)

## 4. Sort with samtools and build a index
    samtools sort m_genitalium.bam -o m_genitalium.sorted.bam
    samtools index m_genitalium.sorted.bam

## 5. Starting pylon for missmatch correction
    pilon --genome m_genitalium.fasta --frags m_genitalium.sorted.bam --output m_genitalium_improved

## 6. Quality control with `busco.py`
* needs a database (`wget` from busco homepage)
* tells you how many conserved genes could be found in the assembly (QC)

```bash
wget http://busco.ezlab.org/datasets/bacteria_odb9.tar.gz

BUSCO.py -i m_genitalium_improved.fasta -l bacteria_odb9 -o busco_genitalium -m genome
```
`-i input` = output of pylon `-l` is the database from `wget` `-m genome` = which type
