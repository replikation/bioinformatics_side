Biological Databases
===
____

# Major Databases
**Nucleic ([ENA](https://www.ebi.ac.uk/ena), [Genbank](https://www.ncbi.nlm.nih.gov/genbank/),
[DDBJ](http://www.ddbj.nig.ac.jp/))**

* ENA European Nucleotide Archive (go here to add DNA)
* they all share the same Accessionnumber

**Protein (SwissProt, RefSeq, TREMBL)**

* all proteins go through Interpro

**Genomic (ENSEMBL)**

* Older DB-Versions are always available
* only curated and publicized Genomes
* highly accurate
* Looking for a Species? go to ENSEMBL
* includes Cytogenic Banding (shows how stained Chromosomes have to look)
* EST shows were the gene is expressed (e. g. liver)

**Protein structure (PDB)**

# DB for protein analysis
* Sequences from: Uniprot (Swissprot; trEMBL); Structure PDB (rcsb.org)
* Usually similar seq/structure = similar function
* Sequence motifs explained:
>{L} = everything except L; [LM] = M or L; x = everything [color=lightgreen]
* Use sequence profiles (e.g. motifs) to get more distant but related proteins, instead of just using BLAST
* Physiochemical properties (weight, electric load, hydrophili profile etc.) are taken into account for alignments

## Secondary structure prediction
* use a combination of methods, not only one
* try get a consensus "prediction pattern" of all prediction methods

> Best Site for this is "NPSA-pbil", its called consensus sec. structure prediction [**search here**](https://npsa-prabi.ibcp.fr/cgi-bin/npsa_automat.pl?page=/NPSA/npsa_seccons.html).

* other:

>http://expasy.org/
>
>http://www.unoriprot.g/
>
>http://www.ebiokit.eu/
>
>http://blast.ncbi.nlm.nih.gov/Blast.cgi
>
>https://www.ebi.ac.uk/interpro
>
>http://www.rcsb.org/pdb

* Protein Visualization with: **UGENE Suite**, DeepView, VMD (good one), Pymol, Rasmol/Jmol

## Tridimensional structure prediction (homology modeling)

* You need to find a protein template (use BLAST or PSI-BLAST) with a similarity of at least 30 % and a structure
* Do a pairwaise alignment then
* Build a predicted structure with:
    * Rigid body assembly, segment matching (SWISS - MODEL, SegMOD)
    * Satisfaction of spatial restraints (distance and angles) => MODELLER, Geno3D
    * Artificial evolution (template - based methods with ab initio - like energy minimization principles) => NEST
* Check quality afterwards (PROCHECK, PDBSum etc. to see if the model seems possible)
