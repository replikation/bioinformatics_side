# Nextflow

# Installation

````bash
# Java run time
sudo apt-get install -y default-jre
# nextflow, download and create executable
wget -qO- https://get.nextflow.io | bash
# move to $PATH for convienence
sudo mv nextflow /usr/bin
````

##

nextflow clean -f

* removes leftovers from pipeline

## channel quick report

accession_list.subscribe { println "Got: ${it}" }



## split Channel
you have a input channel with one value and want to split into multiple channels

you have for this:
splitCsv
splitText
splitFasta
splitFastq

the operator has to be added to the input stream.
you never modify the output stream, you are doing this with the input stream!!

```groovy
input:
  val(x) from test_ch.splitText()
```
