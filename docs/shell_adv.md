# Advanced shell
## Remote/cloud computing
### ssh & keys

* tutorials for key generation and stuff [here](https://www.digitalocean.com/community/tutorials/how-to-copy-files-with-rsync-over-ssh)
* `ssh`, `-i ~/.ssh/key`(key location) and username@IP, use the `-K` flag to allow fastqc graphical interface

```bash
# examples
ssh -i ~/.ssh/replikation.pem ubuntu@ec2-18-218-161-33.us-east-2.compute.amazonaws.com
ssh -i ~/.ssh/azure_rsa student@13.95.67.169
```
### file transfer

* normally you use the `scp` command
* however use `rsync` for huge amount of files

```bash
# examples
rsync -avp -e "ssh -i ~/.ssh/cloudgoogle" * replik@35.231.111.99:~/fast5_files
rsync -avpr -e "ssh -i ~/.ssh/cloudgoogle" <USER>@<IP>:~/auto_assembly/ .
# example for synology with different rsync path
# Username@IP is stored in $IP
rsync --rsync-path=/bin/rsync -vr -e "ssh -i ~/.ssh/id_rsa" --remove-source-files --include "*.fast5" --include "*/" --exclude "*" /cygdrive/c/data/reads/ $IP:/volume1/sequencing_data/
```

### Screen - virtual shell session in the cloud

* you want to do this if you work remotely
* than it wont matter if the terminal window is closed sinces its a virtual process running on the host
* Start a virtual shell in a cloud computer:

```bash
screen -R <name> #name of the virtual machine
# works with tab completion after you started a virtual screen
screen -R <tab completion>
```

* A new empty command screen appears (this is the virtual screen)
* start the command in it and close the window
* to see all active screens type `screen -list`
* to get back to your screen type `screen -R <name>` or the number of `screen -list` (works better) so `screen -R 37509`


## Advanced Scripting for deploying Software
* use [fabric](http://www.fabfile.org/) for software deploying scripts
* take a look at Hadriens [github for examples](https://github.com/SGBC/course/tree/master/fabfiles)
* makes sense to just create a simple sudo apt install script for all the important libraries

## pip installation, for python packages

* due to the version chaos that is pip pip3 python and python3, here an explanation
* stick with python 3
* so if a site tells you to do `pip install` do `pip3 install`

```bash
# installing the pip3 from python3.X (not pip from python 2.7)
sudo apt-get install python3-pip ## use the apt-get for now
# example of a pip install, DONT use sudo here
pip3 install pylama pylama-pylint
```
