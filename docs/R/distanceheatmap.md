heatmaps
===


* fast distance estimation via mash

´´´´bash
# better as a all versus all
mash sketch -o reference *.fasta
mash info reference.msh
mash dist reference.msh *.fasta
````
