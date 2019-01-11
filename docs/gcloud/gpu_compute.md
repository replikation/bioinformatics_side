

# 1. Instanz erstellen mit tesla P100
* costs around 1 Dollar per hour
* 500 GB "nicht flüchtiger HD" costs around 20 Euro per month
  * 1TB 40 Euro

# 2. To do
* create 1 TB. diskspace
  * see if you can do this as a script
    * e.g. total-space + (0.2 * total space)
    * so it creates enough for the fast5 and space for fastq
    * and uploades the data via rsync
* upload - or combination via the diskspace
* create the gpu compute engine
* see how much RAM or cpu is needed
* check the ONT tutorial for google cloud tutorials
  * first check with the micro instance how docker works up there
  * also create a gpu docker for guppy
* attach disk to gpu machine and let it run



# Observing gpu
* install lshw-gtk
* excute sudo lshw -C display
