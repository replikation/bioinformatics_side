Shell Commands
===
___
Terminal shortcuts

* ctrl + C terminates a process
* ctrl + D logout from a ssh remote
* ctrl + Z pauses a process

# 1. Basic Commands

```bash
    cut -d -f3         # -f 3,4 also possible, delimiter tab is default for -d without input
    sort
    head -10
    less
    uniq -c             # a uniq that also counts using -c
    wc -l               # wordcount or -l lines
    tail -n +2          # starts at line 2, reverse "head"
    seq 1 10            # sequence, prints 1 2 3 4 5 (...) 10
    grep -c ">"         # -c counts the ">" in all the files you give grep
    grep -w "ls"        # exact match
    ls -lh              # human readable ls command  
    ln -s ~/data/* .    # creates a link for all data in /data to current folder .      
    zless               # makes a "less" on an gz file
    tar -xvf
    mkdir -p            # creates only folder if it doesnt exist
    chmod u=rw          # [u g o a] [-+=] [rwx] user group other all(=user&group&other)

    htop                # CPU and RAM information    
    echo $PATH          # shows all bin/ directories
    df -h                  # free disk space
```
## 1.1 Variables

* you can manipulate the output of variables directly
* write variables like this `${x}` to make sure it uses the right variable name, if there comes no white space after
* e.g. varible `x` can't be found in `$x_test` since it adds the `_test` to the name, but `${x}_test` would work

```bash
# example for variable x
echo "$x"
# Forward_sequences.fasta

# Remove the something from the end:
echo "${x%.fasta}"
# Forward_sequences

# Remove something in front
echo "${x#Forward_}"
# sequences.fasta
```

## 1.2 Pipes

```bash
# examples
cut -d, -f "$2" "$1" | tail -n +2 | sort | uniq -c
cut -d, -f4 data/survey_data.csv | tail -n +2 | sort | uniq -c | sort -n
```

## 1.3 Loops

```bash
# example 1
for x in $(seq 1977 2002)
 do
        grep $x results/taxa_per_year.txt > results/years/$x-count.txt
 done
# example 2    
for x in *.fasta;
 do
     mkdir ${x%.fasta} #${x%.fasta} removes ".fasta" from the variable $x
     mv $x ${x%.fasta} # so the folder would not be named Seq1.fastq/
 done
# example 3: Sickle loop with "basename" to extract part of filenames
for i in *_1.fastq
 do
    prefix=$(basename $i _1.fastq)
    echo "${prefix} is correcting with sickle"
    sickle pe -f ${prefix}_1.fastq -r ${prefix}_2.fastq -o ${prefix}_1_corr.fastq -p ${prefix}_2_corr.fastq -t sanger -s /dev/null -q 2
done
```

## 2. Installing (apt) & Downloads

```bash
apt search "TERM" # searches apt database
apt show "ncbi-blast+" # described the package
sudo apt install ncbi-blast+
# commands for download
wget <URL>
# sometimes better then wget, can handle redirections
curl -O -J -L <URL>

# to check the md5 sum of the file
md5sum <file>
```
