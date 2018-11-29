Manipulating files via bash
===
___
# 1. Awk & sed fastq/a manipulation
## 1.1 Convert .fastq to .fasta

* using awk, sed for file manipulation
* also includes creating fasta oneliners

```bash
# converting fastq to fasta
sed -n '1~4s/^@/>/p;2~4p' INFILE.fastq > OUTFILE.fasta
```

## 1.2 Converting .fasta to one liner
**One line is fasta header, one line is sequence**

* it removes the "sequence wraps"
* perfect to extract sequences, e. g. `grep "blaCMY" -A1 sequencelist.fasta`

```bash
# make fasta files to one liner
sed ':a;N;/^>/M!s/\n//;ta;P;D' Input.fasta > oneliner.fasta
```

## 1.3 Remove sequences by length

```bash
# filter multi fasta by a seq length - in this case 1000 bp
awk '/^>/ { getline seq } length(seq) >1000 { print $0 "\n" seq }' oneliner.fasta > online_grt1000.fasta
```

### Summary

* as loops:

```bash
# lazy way
    for x in *.fastq; do sed -n '1~4s/^@/>/p;2~4p' $x > ${x%.fastq}.fasta ; done
    for x in *.fasta; do sed ':a;N;/^>/M!s/\n//;ta;P;D' $x > ${x%.fasta}_oneliner.fasta ; done
    for x in *_oneliner.fasta; do awk '/^>/ { getline seq } length(seq) >1000 { print $0 "\n" seq }' $x > ${x%_onliner.fasta}_clean.fasta ; done

# check all reads
for x in *.fasta ; do echo  grep -c ">" $x
```

# 2. Convert .gfa to .fasta

* extracts the sequences out of  a gfa file

```bash
awk '/^S/{print ">"$2"\n"$3}' file_in.gfa | fold > file_out.fasta
```
