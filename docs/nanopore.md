# Nanopore - practical

* a few example workflows
* **in general always check the `--help` for each program**

## 0. Base calling using

```bash
# part of my skript so variables are asigned to the options
read_fast5_basecaller.py -i $currDir/$i -f $flowcell -t $CPU -q 100000 -o fastq -k $kittype -r -s $currDir/FASTQ/
```

## 1. QC of reads
* Using the assembly-stats to get a summary of the reads:

`assembly-stats ERR1147227.fastq`

## 2. Automated removal of adapter and demultiplexing with `porechop`

* porechop needs seperate outputs depending if barcodes were present or not

```bash
porechop -i <input>.fastq -b <output_foulder>
```

* You can redo `assembly-stats` to validate

## 3. Assembly with `minimap2` and `miniasm`

* use `canu` for de novo assemblies

* `minimap2` creates a files that `miniasm` needs for assembly
* `minimap2` creates a first draft map. However its __not a good minion only assembler__, we use it here to combine it with illumina data

```bash
minimap2 -x ava-ont ERR1147227_trimmed.fastq ERR1147227_trimmed.fastq | gzip -1 > ERR1147227.paf.gz

miniasm -f ERR1147227_trimmed.fastq ERR1147227.paf.gz > ERR1147227.gfa
```

* canu example

```bash
canu -d run_e.coli -p e.coli genomeSize=6m -nanopore-raw 7718_E.coli_sum.fastq
```

## 4. Polishing

* We do: index build, mapping reads to assembly, piping to samtools for bam conversion, sorting, indexing
* `-p4` for 4 threads (CPU) as `bowtie2` takes time

```bash
bowtie2-build ERR1147227.fasta ERR1147227

bowtie2 -p4 -x ERR1147227 -1 ecoli_hiseq_R1.fastq.gz -2 ecoli_hiseq_R2.fastq.gz | samtools view -bS -o ERR1147227.bam

samtools sort ERR1147227.bam -o ERR1147227.sorted.bam

samtools index ERR1147227.sorted.bam
```
+ Now we can do the actual polishing with `pilon`

```bash
pilon --genome ERR1147227.fasta --frags ERR1147227.sorted.bam --output ERR1147227_improved
```
