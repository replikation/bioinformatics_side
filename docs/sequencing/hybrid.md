# Hybridassemblies

* the currently to go standard is nanopore and illumina reads combined
* this is easily done via unicycler
* [github link](https://github.com/rrwick/Unicycler)

What it does (briefly):
* spades Assembly with illumina reads only
* closing the gaps using only the nanopore Reads
* polishing via pilon
* and a few more corrections and stuff
* So we only go with `porechop` for long reads and `scythe` `sickle` for short reads

## Unicycler

```bash
unicycler -1 <fwd>_R1.fastq.gz -2 <rev>_R2.fastq.gz -l nanopore_reads.fastq -o outputfolder
```

`-1` and `-2` are the illumina inputs `-l` is the long read input `-o` is the output


## Assembly visualization
* with `Bandage`
[Bandage](https://github.com/rrwick/Bandage/) is a GUI to interact with assembly graphs made by *de novo* assemblers such as Velvet, SPAdes, MEGAHIT and others.

> ![](http://sepsis-omics.github.io/tutorials/modules/cmdline_assembly/images/illumina_assembly_bandage.png)
> Figure: Connects Contigs; Repeats are region were many contigs connect; nanopore is one line
