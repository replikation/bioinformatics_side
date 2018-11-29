# Important Software

* here i include a few basic programs that are Important
* this list is not complete, because it changes usually quite fast

## SRA-toolkit
* [link to sra website](https://www.ncbi.nlm.nih.gov/sra/docs/toolkitsoft/)

```bash
wget https://ftp-trace.ncbi.nlm.nih.gov/sra/sdk/current/sratoolkit.current-ubuntu64.tar.gz
tar xf sratoolkit.current-ubuntu64.tar.gz
cd sratoolkit.2.8.2-1-ubuntu64/bin
mv * ~/.local/bin
```
## Assembly-stats
* for installation see [github](https://github.com/sanger-pathogens/assembly-stats)

## porechop
* Adapter removal and demultiplexing of nanopore data
* for installation see [github](https://github.com/rrwick/Porechop)

## minimap2 & miniasm
* fast but error-prone long read assembler

> Use this only with pilon and Illumina correction

* [github link minimap2](https://github.com/lh3/minimap2)
* [github link miniasm](https://github.com/lh3/miniasm)

```bash
# Install minimap and miniasm (requiring gcc and zlib)
git clone https://github.com/lh3/minimap && (cd minimap && make)
git clone https://github.com/lh3/miniasm && (cd miniasm && make)
```
## MUMmer

* [github link](https://github.com/mummer4/mummer)

```bash
wget https://github.com/mummer4/mummer/releases/download/v4.0.0beta2/mummer-4.0.0beta2.tar.gz
tar xf mummer-4.0.0beta2.tar.gz
cd mummer-4.0.0beta2/
./configure LDFLAGS=-static
make
sudo make install
```
## Unicycler
* Hybrid assembler pipeline
* [github link](https://github.com/rrwick/Unicycler)
