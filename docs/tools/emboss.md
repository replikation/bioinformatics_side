# EMBOSS Open Software Suite
+ `wossname` is your first to go command to search for all the tools
```bash
wossname "sequence alignment"
```
* total of around 100 tools, including data retrivel from web (`fastq-dump`)
* install EMBOSS:
`sudo apt-get install emboss`
* You may need libx11-dev module (visualisation of graphs)
`sudo apt-get install libx11-dev`
* [Web based Version](http://wemboss.sourceforge.net)

> **Tools included (selection):**
>
> `prophet` gapped alignment
>
> `water` smith waterman local alignment
>
> `infoseq` get information of a sequence
>
> `showfeat` get features of a seq, coils, bindingsites etc.
>
> `eprimer3` or primer3, primers and hybridization oligos, best one
>
>  `water` is highly accurate alignment but very slow
>
>  `seqret` fetches sequences from databases
>
>  `getorf` finds open reading frames
>
>  `transseq` translate nucleotides seq to proteins
>
>  `dottup` dot plot of pairwise alignment good image for publication
>
>  `needle` global alignment
>
>  `prettyplot` multiple seq. alignment and graphics
>
>  `equicktandem` microsatellites, palindroms, repeats

# wEMBOSS
* you need a EMBOSS server (own or a other available somewhere (like a workstation))
* you access this with wEMBOSS
* all results can be created in html
* wEMBOSS needs to know if DNA or Protein sequence, then it shows associated programs
* GUI is user friendly but less flexible
* program shows you the associated "command line
