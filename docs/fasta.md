# Manipulating files via bash
## Convert .fastq to .fasta

* using awk, sed for file manipulation
* also includes creating fasta oneliners

```bash
# converting fastq to fasta
sed -n '1~4s/^@/>/p;2~4p' INFILE.fastq > OUTFILE.fasta

# maka fastafiles one liner (good for shell stuff)
sed ':a;N;/^>/M!s/\n//;ta;P;D' Input.fasta > oneliner.fasta

# filter multifasta by a certain length - in this case 1000 bp
awk '/^>/ { getline seq } length(seq) >1000 { print $0 "\n" seq }' oneliner.fasta > online_grt1000.fasta
```

* as loops:

```bash
# lazy way
    for x in *.fastq; do sed -n '1~4s/^@/>/p;2~4p' $x > ${x%.fastq}.fasta ; done
    for x in *.fasta; do sed ':a;N;/^>/M!s/\n//;ta;P;D' $x > ${x%.fasta}_oneliner.fasta ; done
    for x in *_oneliner.fasta; do awk '/^>/ { getline seq } length(seq) >1000 { print $0 "\n" seq }' $x > ${x%._onliner.fasta}_clean.fasta ; done

# check all reads
for x in *.fasta ; do echo  grep -c ">" $x
```

## Convert .gfa to .fasta

```bash
awk '/^S/{print ">"$2"\n"$3}' file_in.gfa | fold > file_out.fasta
```
