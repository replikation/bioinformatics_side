Conda
===

* Package, dependency and environment management for any language—Python, R, Ruby, Lua, Scala, Java, JavaScript, C/ C++, FORTRAN
* check here: https://conda.io/docs/user-guide/install/linux.html

# Installation

* miniconda should be enough, the others are "bigger"

````bash
wget https://repo.continuum.io/miniconda/Miniconda3-latest-Linux-x86_64.sh
bash Miniconda3*.sh
````

**add conda channels**

* add bioconda to the channel https://bioconda.github.io/

````bash
conda config --add channels defaults #usually already installed
conda config --add channels bioconda
conda config --add channels conda-forge
````
