Advanced shell
===
___
# 1. Remote/cloud computing
## 1.1 ssh & keys

* tutorials for key generation and stuff [here](https://www.digitalocean.com/community/tutorials/how-to-copy-files-with-rsync-over-ssh)
* `ssh`, `-i ~/.ssh/key`(key location) and username@IP, use the `-K` flag to allow fastqc graphical interface

```bash
# examples
ssh -i ~/.ssh/replikation.pem ubuntu@ec2-11-218-160-32.us-east-2.compute.amazonaws.com
ssh -i ~/.ssh/azure_rsa student@13.45.57.169
```
## 1.2 file transfer

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

# 2. Screen

**virtual shell session in the cloud**

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


# 3. Advanced Scripting for deploying Software
* use [fabric](http://www.fabfile.org/) for software deploying scripts
* take a look at Hadriens [github for examples](https://github.com/SGBC/course/tree/master/fabfiles)
* makes sense to just create a simple sudo apt install script for all the important libraries


# 4. mounting devices

* get the name of the disk to mount via `df`

```bash
# mount <name of disk> to <target>
sudo mount /dev/nvme1n1p1 /mnt/
```
