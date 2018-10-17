Unix
===
____
# Unix Commands
## Short cuts
ctrl + C terminates a process
ctrl + D logout from a ssh remote
ctrl + Z pauses a process

ctrl + alt + arrow for multi desktop
## Standard commands
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
