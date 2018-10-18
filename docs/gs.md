# Genomic selection (GS)
## Overview (MAS versus GS)
**Marker assisted selection (MAS)**: A small number of molelcular markers are used to tag genes-of-interest, but the overall impact on enhancing the efficiency of breeding is limited. MAS has been successfully used to incorporate major genes and/or QTLs. But, most traits of interest are not controlled by just a few large-effect genes, but by many genes of small effect and/or by a combination of major and minor genes. MAS is far less suitable for these types of trait genetic architectures. Epistatic interactions and the effects of genetic background make molecular breeding even more complicated.
>**Traditional EBV** (estimated breeding values): based on Mendelian Sampling
* for MAS you use the program **MAS-BLUP**

**Genomic selection (GS)**, introduced in 2001, presents a new alternative to traditional MAS that actually improve gain per selection in a breeding program per unit time, and thus breeding efficiency. In a GS breeding schema, genome-wide DNA markers are used to predict which individuals in a breeding population are most valuable as parents of the next generation of offspring. GC can get breeding values on newborns or embryos, therefore we can do selection way earlier than waiting for a specific trait to show. The generation intervall is faster and the genetic progress per year increases significantly.

>**gEBV** (genomic-based estimated breeding values): Selection on SNP effects, includes Mendelian Sampling [color=lightgreen]
+ for GS you use the program **GBLUP**

A good [overview of this can be found here.](https://www.illumina.com/content/dam/illumina-marketing/documents/products/research_reviews/genomic-selection-in-agriculture.pdf)

|MAS|Genomic Selection|
|-|-|
|Find QTL or genes and select specifically for favourable alleles|Estimate effects across all markers and select on the sum of effects
|Need strategy to combine with EBV|Replaces EBV (gEBV)
|LE-MAS computationally intensive|Can be very computationally intensive
|LE-MAS has major genotyping requirements|Major genotyping requirements

## MAS - marker assisted selection
### 3 starting points of MAS
**GAS (gene assisted selection)**
*Functional mutations - known genes*
* QTL Genotype and effect
* Genotype selection candidates
* For application in unrelated breeds, effect needs to be verified

**LD-MAS (linkage disequilibrium- marker assisted selection)**
*Markers in pop.-wide LD with functional mutation*
* As straightforward as GAS, more markers when using haplotype
* Monitor association over time
* Application in other breeds my require confirmation study

**LE-MAS (linkage equilibrium- marker assisted selection)**
*Markers in pop.-wide LE with functional mutation*
* Only within family LD between marker and **QTL (quantitative trait locus)**
* IBD and QTL variance
* Requires extensive genotyping and statistical analysis
* Can be implemented directly after QTL detection

### Strategies for MAS and Uptake
How to balance selection on markers and standard EBV?
* Tandem Selection
  * First select animals with favourable genotype
  * The best EBV within that group
* Index Selection
  * Select on weighted sum of EBV and Marker Score
* Pre-selection
  * Select on markers in early life
  * Then EBV later in life

**Uptake:**
+ Mainly for single gene defects (GAS or LD-MAS)
    + BLAD, CVM, RYR
+ Some candidate genes
    + IGF2, Myostatin, MC4R

## GS - genomic selection
## Overview
* Use genome-wide SNP information
* Rather than detecting QTL, estimate effects for all SNPs or SNP haplotypes
* Genomic breeding value (gEBV) is sum of all haplotype effects

### Principle/Concepts for GS
* You have a training population
  * Population with known phenotypes (traits)
  * thousands of genetic markers
  * Used to create the gEBV
* You have a selection population, were you apply the gEBV
  * so only the genotypes are known
  * estimate the phenotype based on the gEBV
* Based on this you choose parents for the next "generation" of traits you want

![](http://www.cell.com/cms/attachment/2007959821/2030624957/gr4.jpg)

### GBLUP
**BLUP** means **best linear unbiased prediction**, and this method allows breeders of _livestock_ to predict the breeding value of their herd animals. GBLUP is the genetic-based BLUP.
* Special Case: each genome region contributes equally to the trait
  * Rather that summing BLUP estimates across SNPs
  * Estimate BLUP at animal level using average marker based relationships.
* Same as traditional EBV but using a marker-derived relationship matrix rather than a pedigree (*Stammbaum*) derived A matrix.
* No need for pedigree recording (all genome selected), so may be a solution for species or systems where pedigree recording is difficult

**Basic Formula for Genetic gain per year:**

* some errors here since i need a formular generation

$$
G = {r * i * \sigma a \over L_G}
$$

**G** = Genetic gain per year
**i** = Selection intensity e. g. i take the top 10 % of all animals with my trait
**σa** = additative genetic standard derivation
**L~G~** = Generation intervall (average year of the parents when their offspring is born)
**r** = its a normalization of D

$$
r = \sqrt{N_p h²\over N_p h²+min(N_{QTL},M_e)}
$$

**N~P~** = Number of phenotypes
**h²** = heritability
**N~QTL~** =
**M~e~** is calculated with:

$$
M_e = {2N_eL \over ln(4N_eL) }
$$

**L** = Genome size in Morgan (you need to google the value)
**N~e~** = Population size

**Software  is available for these calculations e. g. SelAction for Windows; or use Excel Spreadsheet**

Rule of thumb: for an accuracy of around 0.99:
  * 10 * population size * L markers
  * 100 * population size * L phenotypes in training set

## Pros and Cons of GS
|Pro|Contra|
|-|-|
|Increases accuracy|Genotyping  not cheap
|Don’t need records on all animals|Need new evaluation tools
|May not need pedigree|So far, mainly simulation results
|Can monitor inbreeding|Long term effects unknown
| |Only accounts for additive SNP effects

## Other information about breeding
Pig and chicken Breeding are using pure lines for selection but crossbreds for the "consumer products" like meat and eggs

**Heterosis** - crossbreading performes better then the mean of parents. (e.g. A³⁰⁰ + B⁴⁰⁰ is not C³⁵⁰ its C³⁵⁰⁻³⁷⁵)

**Aquaculture** has mass spawning, and you need populations for breeding (e.g. 30 female and 30 male fish's) -> gene marker are really useful here
